---
slug: sso-saml-intergration-steos
title: Step-by-Step Guide to Implementing IdP-Initiated SSO SAML Integration
authors: [sahil]
tags: [sso, saml]
---
Single Sign-On (SSO) is a powerful authentication method that enables users to log in once and gain access to multiple applications without needing to re-enter credentials. **SAML (Security Assertion Markup Language)** is one of the most widely used protocols for implementing SSO. 

<!-- truncate -->

In this blog, we will walk through the detailed steps to implement **IdP-initiated SSO SAML integration**. This type of SSO flow is initiated by the Identity Provider (IdP) and does not require an initial authentication request from the Service Provider (SP). Instead, the IdP generates a **SAML Response** and directly redirects the user to the SP.


## Step 1: Create a Private and Public RSA Key
### Why Do You Need RSA Keys?
SAML responses must be **digitally signed** to ensure their authenticity and integrity. To do this, you need a **private key** for signing the SAML assertion and a **public key** for verification by the Service Provider.

### How to Generate RSA Keys Using OpenSSL
1. **Generate a Private Key:**
   ```bash
   openssl genrsa -out private-key.pem 3072
   ```
2. **Generate the Corresponding Public Key:**
   ```bash
   openssl rsa -in private-key.pem -pubout -out public-key.pem
   ```
3. **Generate an X.509 Certificate (Optional, but recommended):**
   ```bash
   openssl req -new -x509 -key private_key.pem -out certificate.pem -days 365
   ```
   This certificate is used to verify the signature of the SAML response.

Now you have:
- **Private Key (private_key.pem)** – Used to sign the SAML response.
- **Public Key (public_key.pem)** – Shared with the SP for verification.
- **Certificate (certificate.pem)** – Used by the SP to validate signatures.


## Step 2: Get the Required SAML Metadata
Before generating the SAML response, you need the necessary metadata from both the IdP and SP. The key details include:

### Identity Provider (IdP) Information
- **IdP Entity ID**: The unique identifier for the IdP.
- **Single Sign-On (SSO) URL**: The URL where the IdP sends the SAML response.

### Service Provider (SP) Information
- **SP Entity ID**: The unique identifier for the SP.
- **SP Assertion Consumer Service (ACS) URL**: The URL where the SAML response should be sent.
- **Target URL**: The URL where users will be redirected after authentication.
- **Destination of the Response**: The expected destination in the SAML response.

Example of metadata retrieval:
- If using **MockSAML**, the SP metadata can be found at:  
[https://mocksaml.com/api/saml/metadata](https://mocksaml.com/api/saml/metadata)

This metadata provides the necessary details to construct the SAML response.

## Step 3: Share the Public Key with the SP
Before the SP can validate SAML responses from the IdP, it needs access to the **public key or certificate** that was used to sign the responses.

### How to Share the Public Key
1. **Provide the public key (public_key.pem) to the SP** manually or through a metadata file.
2. **Use an X.509 certificate (certificate.pem)** to make validation easier.
3. **If the SP supports metadata exchange**, provide an XML metadata file that includes the public key:
   
   ```xml
   <md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" entityID="https://idp.example.com">
       <md:IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
           <md:KeyDescriptor use="signing">
               <ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
                   <ds:X509Data>
                       <ds:X509Certificate>MIIC4jCCAc...</ds:X509Certificate>
                   </ds:X509Data>
               </ds:KeyInfo>
           </md:KeyDescriptor>
       </md:IDPSSODescriptor>
   </md:EntityDescriptor>
   ```
The SP will use this key to validate signatures on the SAML responses.

## Step 4: Generate the SAML Response
Once you have the metadata, it's time to generate the **SAML Response**.

### Structure of a SAML Response
A valid **SAML Response** consists of:
1. **Issuer** – The IdP Entity ID.
2. **Assertion** – Contains authentication details, subject information, and attributes.
3. **Signature** – Ensures the response integrity.

### **Example SAML Response (Before Signing)**
```xml
<samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" 
    Destination="https://sp.example.com/sso" 
    ID="_12345" 
    InResponseTo="" 
    IssueInstant="2025-04-01T12:34:56Z" 
    Version="2.0">
    
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        https://idp.example.com
    </saml:Issuer>

    <saml:Assertion xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_67890">
        <saml:Subject>
            <saml:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress">
                user@example.com
            </saml:NameID>
        </saml:Subject>
    </saml:Assertion>

</samlp:Response>
```

## Step 5: Sign the SAML Response
To ensure the SAML response is trusted, it must be signed using the **private key**.

### Signing the Response in Node.js
```javascript
import { SignedXml } from "xml-crypto";

const sig = new SignedXml();
sig.signingKey = privateKey;
sig.addReference("//*[local-name(.)='Assertion']");
sig.computeSignature(samlResponse);

const signedSAML = sig.getSignedXml();
```

Now your **SAML Response is signed** and ready to be sent.


## Step 6: Send SAML Response Using HTTP POST Binding
Once the SAML response is generated and signed, it needs to be sent to the **SP Assertion Consumer Service (ACS) URL** via an **HTTP POST request**.

### Example HTML Form for Posting SAML Response
```html
<form method="POST" action="https://sp.example.com/sso">
    <input type="hidden" name="SAMLResponse" value="BASE64_ENCODED_SAML">
    <input type="submit" value="Login">
</form>
<script>
    document.forms[0].submit();
</script>
```
This form automatically **redirects the user** to the SP with the SAML response.


## Conclusion
By following these steps, you can successfully implement **IdP-initiated SSO using SAML**. Here’s a recap:
1. **Generate RSA Keys** – Create private and public keys for signing SAML responses.
2. **Retrieve Metadata** – Get Entity IDs, ACS URL, and other details from IdP and SP.
3. **Share the Public Key with the SP** – Ensure the SP can verify your signed SAML responses.
4. **Generate SAML Response** – Construct the XML structure with authentication details.
5. **Sign the Response** – Use OpenSSL or xml-crypto to sign the response.
6. **Send via HTTP POST** – Encode and transmit the response to the SP ACS URL.

By using tools like **OpenSSL, SAML Tracer, and SAMLTool.com**, you can debug and validate your SAML integration efficiently.
