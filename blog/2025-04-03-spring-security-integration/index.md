---
slug: spring-security-intergration
title: Spring Boot JWT Authentication with Spring Security — Complete Step-by-Step Guide
authors: [sahil]
tags: [springboot, jwt-authentication, spring-security, rest-api-security]
---

In this tutorial, we'll walk through how to build a **secure JWT authentication system** using **Spring Boot**, **Spring Security**, and the **`jjwt`** library. We'll cover everything from login to securing routes using JWTs — all with full code examples and explanations.

<!-- truncate -->

## Tech Stack

- Spring Boot `3.4.4`
- Spring Security
- jjwt (`0.12.6`)
- Maven
- Java 17+

## 1. Add Maven Dependencies

Add the following dependencies to your `pom.xml`:

```xml
<!-- JWT dependencies -->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.6</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.6</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.6</version>
    <scope>runtime</scope>
</dependency>

<!-- Spring Security -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
    <version>3.4.4</version>
</dependency>
```

**Explanation**: These dependencies bring in support for Spring Security and the `jjwt` (Java JWT) library to help us create and validate tokens.

## 2. JWT Utility Class

### `JwtUtil.java`

```java
@Component
public class JwtUtil {

    @Value("${jwt.secret}")
    private String secretKey;

    @Value("${jwt.expiration}")
    private long jwtExpirationMs;

    public String generateToken(String email) {
        return Jwts.builder()
                .subject(email)
                .issuedAt(new Date())
                .expiration(new Date(System.currentTimeMillis() + jwtExpirationMs))
                .signWith(getSigningKey())
                .compact();
    }

    public String extractEmail(String token) {
        return Jwts.parser()
                .verifyWith(getSigningKey())
                .build()
                .parseSignedClaims(token)
                .getPayload()
                .getSubject();
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parser()
                .verifyWith(getSigningKey())
                .build()
                .parseSignedClaims(token);
            return true;
        } catch (Exception e) {
            return false;
        }
    }

    private SecretKey getSigningKey() {
        return Keys.hmacShaKeyFor(secretKey.getBytes());
    }

    public String extractToken(HttpServletRequest request) {
        String bearer = request.getHeader("Authorization");
        return (bearer != null && bearer.startsWith("Bearer ")) ? bearer.substring(7) : null;
    }
}
```

**Explanation**:
- `generateToken()`: creates a JWT using the user's email.
- `extractEmail()`: reads the email back from a token.
- `validateToken()`: verifies the token’s integrity.
- `getSigningKey()`: uses your configured secret to create a secure signing key.

🛠️ Add to `application.properties`:
```properties
jwt.secret=your-256-bit-secret-value-here
jwt.expiration=3600000
```

## 3. JWT Filter

### `JwtAuthenticationFilter.java`

```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    private final JwtUtil jwtUtil;
    private final UserDetailsService userDetailsService;

    public JwtAuthenticationFilter(JwtUtil jwtUtil, UserDetailsService userDetailsService) {
        this.jwtUtil = jwtUtil;
        this.userDetailsService = userDetailsService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {

        String token = jwtUtil.extractToken(request);

        if (token != null && jwtUtil.validateToken(token)) {
            String email = jwtUtil.extractEmail(token);
            UserDetails userDetails = userDetailsService.loadUserByUsername(email);

            UsernamePasswordAuthenticationToken authToken =
                new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());

            SecurityContextHolder.getContext().setAuthentication(authToken);
        }

        filterChain.doFilter(request, response);
    }
}
```

**Explanation**:
- This filter intercepts every request.
- It checks for a JWT in the `Authorization` header.
- If valid, it loads the user and sets the authentication in Spring Security's context.

## 4. Custom UserDetailsService

### `UserDetailsServiceImpl.java`

```java
@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    private final UserRepository userRepository;

    public UserDetailsServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    public UserDetails loadUserByUsername(String email) throws UsernameNotFoundException {
        User user = userRepository.findByEmail(email)
            .orElseThrow(() -> new UsernameNotFoundException("User not found with email: " + email));

        return org.springframework.security.core.userdetails.User.builder()
                .username(user.getEmail())
                .password(user.getPasswordHash())
                .roles(user.getRole().name())
                .build();
    }
}
```

**Explanation**:
- Loads the user from the database using the email address.
- Converts your custom `User` entity to Spring Security’s `UserDetails`.

## 5. Spring Security Configuration

### `SecurityConfig.java`

```java
@Configuration
public class SecurityConfig {

    private final JwtAuthenticationFilter jwtAuthenticationFilter;

    public SecurityConfig(JwtAuthenticationFilter jwtAuthenticationFilter) {
        this.jwtAuthenticationFilter = jwtAuthenticationFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.csrf(csrf -> csrf.disable())
            .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/login", "/swagger-ui/**", "/v3/api-docs/**").permitAll()
                .anyRequest().authenticated())
            .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }

    @Bean
    public AuthenticationManager authenticationManager(UserDetailsServiceImpl userDetailsService) {
        DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
        authProvider.setUserDetailsService(userDetailsService);
        authProvider.setPasswordEncoder(passwordEncoder());
        return new ProviderManager(List.of(authProvider));
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

**Explanation**:
- Disables CSRF (since we use tokens).
- Stateless session (no server-side session tracking).
- Allows unauthenticated access only to `/api/auth/login`.
- Adds our custom `JwtAuthenticationFilter`.

## 6. Login Endpoint

### `AuthController.java`

```java
@RestController
@RequestMapping("/api/auth")
public class AuthController {

    private final AuthenticationManager authenticationManager;
    private final JwtUtil jwtUtil;

    public AuthController(AuthenticationManager authenticationManager, JwtUtil jwtUtil) {
        this.authenticationManager = authenticationManager;
        this.jwtUtil = jwtUtil;
    }

    @PostMapping("/login")
    public ResponseEntity<JwtResponse> login(@RequestBody LoginRequest request) {
        Authentication authentication = authenticationManager.authenticate(
                new UsernamePasswordAuthenticationToken(request.getEmail(), request.getPassword()));

        String token = jwtUtil.generateToken(request.getEmail());
        return ResponseEntity.ok(new JwtResponse(token));
    }
}
```

### DTOs

```java
public class LoginRequest {
    private String email;
    private String password;
    // getters and setters
}

public class JwtResponse {
    private String token;
    public JwtResponse(String token) { this.token = token; }
    // getter
}
```

**Explanation**:
- Authenticates user using Spring Security.
- Generates a JWT token on success.
- Sends the token in the response.

## 7. Secure API Example

### `UserController.java`

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{userId}")
    public ResponseEntity<User> getUser(@PathVariable UUID userId,
                                        @AuthenticationPrincipal UserDetails userDetails) {
        if (userDetails == null) {
            return ResponseEntity.status(403).build();
        }

        return ResponseEntity.ok(userService.getUserById(userId));
    }
}
```

**Explanation**:
- Uses `@AuthenticationPrincipal` to access the current authenticated user.
- Only allows access if a valid JWT is provided.

## Test Your API

1. Send `POST /api/auth/login` with valid credentials:
```json
{
  "email": "user@example.com",
  "password": "secret123"
}
```

2. Copy the returned token and include it as:
```
Authorization: Bearer <token>
```

3. Call secured endpoints like `GET /api/users/{id}` with the token in headers.

## Conclusion

With these steps, you've built a fully functional JWT authentication system in Spring Boot with:
- Secure login  
- Stateless API  
- Token-based authorization
