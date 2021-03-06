### Subordinate CA Certificate Profile

| **Field** |**Value**                             |
| :-------- | :-------------------------------     |
| Serial Number  | Serial number shall be a unique positive integer with a minimum of 64 bits (minimum of 8 octets), not to exceed 20 octets. <br> Serial numbers shall be non-sequential.  |
| Issuer Signature Algorithm   | sha256 WithRSAEncryption {1 2 840 113549 1 1 11}  |
| Issuer Distinguished Name   | Unique X.500 issuing CA DN as specified in Section 7.1.4 of this CP |
| Validity Period   | Validity Period dates shall be encoded as UTCTime for dates through 2049 and GeneralizedTime for dates thereafter <br> Validity Period shall be no longer than 10 years from date of issue. |
| Subject Distinguished Name   | Subordinate CA Certificate Subject Distiguished Name (DN) shall be a unique X.500 DN as specified in Section 7.1.4 of this CP.  Distinguished Name shall conform to PrintableString string type in ASN.1 notation. <br><br>The Subordinate CA Certificate DN shall be of the following format: <br>cn=US Federal TLS CAx, o=U.S. Government, c=US<br>Where _x_ starts at 1 and is incremented by 1 for each Subordinate CA signed by the Root CA.<br><br>All other attributes, for the CA Certificate Subject fields, shall not be included.  <br><br> Non-production Subordinate CAs signed by non-production Root CA certificates shall include "Test" in the DN. <br>A non-production DN example is: <br>cn=US Federal Test TLS CA1, o=U.S. Government, c=US<br> <br>Subject name shall be encoded exactly as it is encoded in the issuer field of certificates issued by the subject. |
| Subject Public Key Information   | At least 2048 bit modulus, rsaEncryption {1 2 840 113549 1 1 1} |
| Issuer Signature   | sha256 WithRSAEncryption {1 2 840 113549 1 1 11}    |


| **Extension** |  **Required**   | **Critical** | **Value and Requirements** |
| :-------- | :----------------|:----------------|:----------------|
| authorityKeyIdentifier | Mandatory | False |  Octet String<br> Derived using the SHA-1 hash of the Issuer’s public key in accordance with RFC 5280.  Shall match SKI of issuing CA. |
| basicConstraints | Mandatory | True |  cA=True <br> The pathLenConstraint field shall be present and set to zero (0). |
| subjectKeyIdentifier   | Mandatory | False |  Octet String <br> Derived using SHA-1 hash of the public key  |
| keyUsage  | Mandatory | True | Bit positions for keyCertSign and cRLSign shall be set. <br> If the Subordinate CA Private Key is used for signing OCSP responses, then the digitalSignature bit shall also be set. |
| extkeyUsage |  Mandatory  | False | This extension is required for Technically constrained nameConstraints per Section 7.1.2.2 and Section 7.1.5. <br> Required Extended Key Usage: <br> Server Authentication id-kp-serverAuth {1.3.6.1.5.5.7.3.1} <br><br> Optional Extended Key Usage: <br> Client Authentication id-kp-clientAuth {1.3.6.1.5.5.7.3.2} <br><br> Other values may be present consistent with use for server authentication, with approval by the FPKIPA. |
| certificatePolicies |  Mandatory  | False | See Section 7.1.6.3. At least one US Government certificate policy OID listed in Section 7.1.6.1 asserting compliance with this CP, and one CAB Forum certificate policy OID listed in Section 7.1.6.1 asserting compliance with the CAB Forum Baseline Requirements. The certificate shall include all the certificate policy OIDs for all certificates issued by the CA.  |
| subjectAltName | Optional | False  |  |
| authorityInformationAccess | Mandatory | False | OCSP: <br> Publicly accessible URI of Issuing CA's OCSP responder accessMethod = {1.3.6.1.5.5.7.48.1} <br>At least one instance of the OCSP responder access method shall be included. All instances of this access method shall include the HTTP URI name form.<br><br> id-ad-caIssuers: <br> Publicly accessible URI of Issuing CA’s certificate accessMethod = {1.3.6.1.5.5.7.48.2} <br> All instances of this access method shall include the HTTP URI name form to specify an HTTP accessible location containing either a single DER encoded certificate, or a BER or DER encoded “certs-only” CMS message as specified in [RFC5272]. |
| cRLDistributionPoints | Mandatory | False | At least one instance shall be included and shall specify a HTTP URI to the location of a publicly accessible CRL. All URIs included shall be publicly accessible and shall specify the HTTP protocol only.  The reasons and cRLIssuer fields shall be omitted. |
| nameConstraints | Mandatory | True | See Section 7.1.5.  |
