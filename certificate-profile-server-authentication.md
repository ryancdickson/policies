### Server Authentication Certificate Profile

This profile for Server Authentication Certificates contains two (2) certificate types:

- _Domain Validation_ TLS Server Authentication Certificates
- _Organization Validation_ TLS Server Authentication Certificates

There are two (2) differences in the certificate profile implementations between Domain Validation and Organization Validation. The differences are in the _Subject Identity Information_ and the _Certificate Policies_.

| **Field or Extension** | **Domain Validation** | **Organization Validation**  |
| :-------- | :---: | :---: |
| Subject Identity Information  | cn=\<one domain name>[,c=US] | cn=\<one domain name>,S=District of Columbia,O=U.S.Government,c=US |
| Certificate Policies   | Asserts both the US Government and CAB Forum policy oid for Domain Validation      | Asserts both the the US Government and CAB Forum policy oid for Organization Validation  |

Below is the full server authentication certificate profile with _all_ fields and extensions.

| **Field** | **Value and Requirements** |
| :-------- | :------ |
| Serial Number   |  Serial number shall be a unique positive integer with a minimum of 64 bits (minimum of 8 octets), not to exceed 20 octets. <br> Serial numbers shall be non-sequential. |
| Issuer Signature Algorithm   |  sha256 WithRSAEncryption {1.2.840.113549.1.1.11}  |
| Issuer Distinguished Name   |  Unique X.500 Issuing CA DN as specified in Section 7.1.4 of this CP |
| Validity Period   | Validity Period dates shall be encoded as UTCTime for dates through 2049 and GeneralizedTime for dates thereafter <br> Validity Period shall be no longer than 395 days from date of issue. |
| Subject Distinguished Name   | Geo-political SDNs:<br> CN (required) shall contain a Fully-Qualified Domain Name that is one of the values contained in the Certificate's subjectAltName extension <br><br> Organization Name, and State or Province (optional): If present, shall contain both Organization Name, and State or Province.  organizationName shall be U.S. Government, and StateorProvince shall be District of Columbia. <br><br>Country (required) and shall be c=US <br>All other attributes, for the subject field, shall not be included. |
| Subject Public Key Information   | Public key algorithm associated with the public key.<br>May be either RSA or Elliptic curve.<br>rsaEncryption {1.2.840.113549.1.1.1}<br> Elliptic curve key {1.2.840.10045.2.1}<br><br>**Parameters:**<br>For RSA, parameters field is populated with NULL.<br>For ECC Implicitly specify parameters through an OID associated with a NIST approved curve referenced in 800-78-1:<br>Curve P-256  {1.2.840.10045.3.1.7} <br>Curve P-384 {1.3.132.0.34} <br>Curve P-521 {1.3.132.0.35}<br><br>For RSA public keys, modulus must be 2048, 3072, or 4096 bits.  Public exponent _e_ shall be an odd positive integer such that 2^16+1 < =_e_ < 2^256-1. |
| Issuer Signature   |  sha256 WithRSAEncryption {1.2.840.113549.1.1.11}    |

| **Extension** |  **Required**   | **Critical** | **Value and Requirements** |
| :-------- | :----------------|:----------------|:----------------|
| Authority Key Identifier  | Mandatory | False |  Octet String<br>Derived using the SHA-1 hash of the Issuer’s public key in accordance with RFC 5280.  Must match SKI of issuing CA Certificate|
| basicConstraints   | Mandatory | True | cA=False |
| Subject Key Identifier   | Mandatory | False |  Octet String <br> Derived using SHA-1 hash of the public key in accordance with RFC 5280 |
| Key Usage   | Mandatory | True | **Required Key Usage:** <br> digitalSignature <br><br> **Optional Key Usage:** <br> keyEncipherment for RSA Keys <br> keyAgreement for Elliptic Curve <br><br>**Prohibited Key Usage:** <br> keyCertSign and cRLSign |
| Extended Key Usage   | Mandatory | False | **Required Extended Key Usage:** <br> Server Authentication id-kp-serverAuth {1.3.6.1.5.5.7.3.1} <br><br> **Optional Extended Key Usage:** <br> Client Authentication id-kp-clientAuth {1.3.6.1.5.5.7.3.2} <br> <br>**Prohibited Extended Key Usage:** <br> anyEKU EKU {2.5.29.37.0} <br> all others |
| Certificate Policies   |  Mandatory  | False | **Required Certificate Policy Fields:** <br>See Section 7.1.6.4. One US Government certificate policy OID listed in Section 7.1.6.1 asserting compliance with this CP, and one CAB Forum certificate policy OID listed in Section 7.1.6.1 asserting compliance with the CAB Forum Baseline Requirements.  <br><br>**Optional Certificate Policy Fields:** <br> certificatePolicies:policyQualifiers <br> policyQualifierId   id-qt 1 <br> qualifier:cPSuri |
| Subject Alternative Name   | Mandatory | False  | This extension shall contain at least one entry. Each entry shall be a dNSName containing the Fully-Qualified Domain Name of a server. This extension shall not include any Internal Name values. <br> All entries shall be validated in accordance with Section 3.2.2.4. |
| Authority Information Access   | Mandatory | False | **Required AIA Fields:** <br> **OCSP** <br> Publicly accessible URI of Issuing CA's OCSP responder accessMethod = {1.3.6.1.5.5.7.48.1} <br><br> **Id-ad-caIssuers** <br> Publicly accessible URI of Issuing CA’s certificate accessMethod = {1.3.6.1.5.5.7.48.2} <br> All instances of this access method shall include the HTTP URI name form to specify an HTTP accessible location containing either a single DER encoded certificate, or a BER or DER encoded “certs-only” CMS message as specified in [RFC5272]. |
| CRL Distribution Points   | Mandatory | False | At least one HTTP URI to the location of a publicly accessible, full and complete CRL. The reasons and cRLIssuer fields must be omitted.  |
| Private Extensions        | Optional | False | Only extensions that have context for use on the public Internet are allowed.  Private extensions must not cause interoperability issues.  CA must be aware of and defend reason for including in the certificate, and use of Private Extensions shall be approved by the FPKI Policy Authority. |
| Transparency Information  | Mandatory | False | Must include two or more SCTs or inclusion proofs. <br> From RFC 6962, contains one or more "TransItem" structures in a "TransItemList".|
