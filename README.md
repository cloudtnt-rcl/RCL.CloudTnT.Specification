# RCL cloudTnT Specification

## Introduction

This specification outlines the technical requirements for the RCL cloudTnT [W3C Verifiable Credentials](https://www.w3.org/TR/vc-overview/) and [Open Badges 3](https://www.imsglobal.org/spec/ob/v3p0) issuer and wallet  applications. 

Applications that interchange data with the RCL cloudTnT application must follow the requirements of this specifications. Such applications may include external issuer applications that create digital credentials that are to be stored in the RCL cloudTnT wallet application.

### Issuer Application

The issuer application is used by Issuers to issue digital credentials following the requirements of the W3C Verifiable Credentials and Open Badges 3 requirements.

### Wallet Application

The wallet application is used by Holders to store and share their digital credentials with verifiers.

## Roles

### Issuer

An issuer issues digital credentials to holders to recognize their achievement. Examples : training provider, organization, assessor.

### Holder

A holder earns a digital credential as a recognition of his/her achievement. Examples : student, apprentice, employee.

### Verifier

A holder will share their digital credential with a verifier who will make a determination on the validity of the credential. Example : employer. 

## Decentralized Identifiers

[W3C Decentralized Identifiers (DIDs)](https://www.w3.org/TR/did-1.1/) are used to uniquely identify Issuers and Holders.

DIDs are created using the [did:jwk method](https://github.com/quartzjer/did-jwk/blob/main/spec.md). This is the ony method supported.

When creating a did:jwk, the [RSA](https://datatracker.ietf.org/doc/html/rfc8017) cryptographic algorithm should be used with a 2048 bit key size. Only RSA asymmetric keys are supported. The [Json Web Key (JWK)](https://datatracker.ietf.org/doc/html/rfc7517) should be directly included in the did:jwk.

DID should be in the form of a text (.txt) file. Issuers and holders should be allowed to download and save thier DID, as well as the associated private key.

### Sample did:jwk

```bash
did:jwk:eyJrdHkiOiJSU0EiLCJuIjoicHVYb3VRS1Vha2t2X2JUZWQ4dkNYLU9FTG1jUzhqQ21DWE9WZFp2b3l5c0wxTWMyMWZGSzBxMXBIN1dMRU1hOUFhd1hQSk1sckdEdmcxT0FiS1h0TkMwZ2hHMTR2dzVqQXpieldsb3F3c25jaGlQRk5ENWt6aTNfUmNpYzlxZlpGUnN3aUdjUkNtRHNKUnlqX244MDhVZkNGdkRnYVZzVjlNNVJhMmNZMHlYQkJDM29tRkpJNXBkTEEySTFFRFZuMWJkTzFaRXQtUFY3Z3c0MWFQZHdhNzd2cTBNRkFDaTNyS0wtTEdzWkNxYlo1Q0ZsNjJFMWNYMU5KZmd1d3BoMDJHMEdRSjZIMnhBSFdnU3BkVUtXcTNQdXJrWVl3VGkwTFJXR015Mk5sSzdwVUxPMlFwem1GN2tWRmdhbW5fb0VOMFlhVkoxbVgzVE02UEVHVzZFdDFRIiwiZSI6IkFRQUIifQ
```

### Sample public Json Web Key (jwk) used to create the did:jwk

```json
{
  "kty": "RSA",
  "n": "puXouQKUakkv_bTed8vCX-OELmcS8jCmCXOVdZvoyysL1Mc21fFK0q1pH7WLEMa9AawXPJMlrGDvg1OAbKXtNC0ghG14vw5jAzbzWloqwsnchiPFND5kzi3_Rcic9qfZFRswiGcRCmDsJRyj_n808UfCFvDgaVsV9M5Ra2cY0yXBBC3omFJI5pdLA2I1EDVn1bdO1ZEt-PV7gw41aPdwa77vq0MFACi3rKL-LGsZCqbZ5CFl62E1cX1NJfguwph02G0GQJ6H2xAHWgSpdUKWq3PurkYYwTi0LRWGMy2NlK7pULO2QpzmF7kVFgamn_oEN0YaVJ1mX3TM6PEGW6Et1Q",
  "e": "AQAB"
}
```
### Private Keys

In the wallet application, the DID private key for the holder should not be stored in the wallet database or other storage system. Th private should be controlled soley by the holder. No external party should have access to the holder's private key.

When a holder adds an external DID to the wallet application, they must upload the DID as a text (.txt) file, as well as, use their private key to verify that they own the DID. The wallet application should only use the private key to verify the DID and should not store the private key in the database.

In the issuer application, the DID private key for the issuer can be stored in the application's database once the issuer has full and sole control of the issuer application. No external party should have access to the issuer's private key.

## Credential Data Model

### AchievementCredential

| Property         | Type              | Required |
| -----------------| ------------------|----------|
| @context         | List of strings   | Yes      | 
| id               | string            | Yes      |
| type             | List of strings   | Yes      |
| issuer           | Profile           | Yes      |
| validFrom        | string            | Yes      |
| validUntil       | string            | Yes      |
| credentialSubject| AchievementSubject| Yes      |
| iss              | string            | Optional |
| jti              | string            | Optional |
| sub              | string            | Optional |

### Profile

| Property         | Type              | Required |
| -----------------| ------------------|----------|
| id               | string            | Yes      |
| type             | List of strings   | Yes      |
| name             | string            | Yes      |

### AchievementSubject

| Property         | Type              | Required |
| -----------------| ------------------|----------|
| id               | string            | Yes      |
| type             | List of strings   | Yes      |
| achievement      | Achievement       | Yes      |

### Achievement

| Property         | Type              | Required |
| -----------------| ------------------|----------|
| id               | string            | Yes      |
| type             | List of strings   | Yes      |
| name             | string            | Yes      |
| description      | string            | Yes      |
| criteria         | Criteria          | Yes      |

### Criteria

| Property         | Type              | Required |
| -----------------| ------------------|----------|
| id               | string            | Optional |
| narrative        | List of strings   | Optional |

### Example Credential

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://purl.imsglobal.org/spec/ob/v3p0/context-3.0.3.json"
  ],
  "id": "b08d340b24f64fe2b1a4b8af6e9458bc",
  "type": [
    "VerifiableCredential",
    "OpenBadgeCredential"
  ],
  "issuer": {
    "id": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoieElDZGFobElaNVplbngyeVI4VHJfOWdWSi1lcUVnODJnSnd6YUxXZGhId0NmSHFJY1hTbUJjV2w4akpNWWREbmpRdGdwam9FRDlPQk9sazhFZy1IU095QXVkc0FrcXpLcjNwRzIyWUVGY2NGZ0E2N1UzakxGbHQxcERoMmpzbzlYWkVLS1JrclYwS2ZTYmJVM1ZHS2hYOHZTVjB4WmNkZ2pHTEZfZGJJakh0WExDaFF4ZEl3MFU2dVVkODU3VGt6LXNyQVhISXkxeWNueGdMQWlucXkzTDhTZ01iSVZSdEJfZjFMYTNXVlkydVMyVjNUNGJwYkd5VVBRZmk3SkZmR2hqcG5BOTctR0IwZWgzMHoxbkJqZTZTdERGRk1abmJRUXlPWkljemVLS0JfdkNobjBOMGJOMVhtaGIzdER5Y1UxdFRMZEZaVDZLUDFRZVExMGc3OC1RIiwiZSI6IkFRQUIifQ",
    "type": [
      "Profile"
    ],
    "name": "Ray Consulting Limited"
  },
  "validFrom": "2025-05-19T00:00:00Z",
  "validUntil": "2030-05-19T00:00:00Z",
  "credentialSubject": {
    "id": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoicHVYb3VRS1Vha2t2X2JUZWQ4dkNYLU9FTG1jUzhqQ21DWE9WZFp2b3l5c0wxTWMyMWZGSzBxMXBIN1dMRU1hOUFhd1hQSk1sckdEdmcxT0FiS1h0TkMwZ2hHMTR2dzVqQXpieldsb3F3c25jaGlQRk5ENWt6aTNfUmNpYzlxZlpGUnN3aUdjUkNtRHNKUnlqX244MDhVZkNGdkRnYVZzVjlNNVJhMmNZMHlYQkJDM29tRkpJNXBkTEEySTFFRFZuMWJkTzFaRXQtUFY3Z3c0MWFQZHdhNzd2cTBNRkFDaTNyS0wtTEdzWkNxYlo1Q0ZsNjJFMWNYMU5KZmd1d3BoMDJHMEdRSjZIMnhBSFdnU3BkVUtXcTNQdXJrWVl3VGkwTFJXR015Mk5sSzdwVUxPMlFwem1GN2tWRmdhbW5fb0VOMFlhVkoxbVgzVE02UEVHVzZFdDFRIiwiZSI6IkFRQUIifQ",
    "type": [
      "AchievementSubject"
    ],
    "achievement": {
      "id": "015301207aa74f5fa548ac55bb884996",
      "type": [
        "Achievement"
      ],
      "name": "Sample Verifiable Credential",
      "description": "This credential is an example of a Verifiable Credential.",
      "criteria": {
        "narrative": "To achieve this credential, a user can download this Verifiable Credential and use it for demonstration purposes."
      }
    }
  },
  "iss": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoieElDZGFobElaNVplbngyeVI4VHJfOWdWSi1lcUVnODJnSnd6YUxXZGhId0NmSHFJY1hTbUJjV2w4akpNWWREbmpRdGdwam9FRDlPQk9sazhFZy1IU095QXVkc0FrcXpLcjNwRzIyWUVGY2NGZ0E2N1UzakxGbHQxcERoMmpzbzlYWkVLS1JrclYwS2ZTYmJVM1ZHS2hYOHZTVjB4WmNkZ2pHTEZfZGJJakh0WExDaFF4ZEl3MFU2dVVkODU3VGt6LXNyQVhISXkxeWNueGdMQWlucXkzTDhTZ01iSVZSdEJfZjFMYTNXVlkydVMyVjNUNGJwYkd5VVBRZmk3SkZmR2hqcG5BOTctR0IwZWgzMHoxbkJqZTZTdERGRk1abmJRUXlPWkljemVLS0JfdkNobjBOMGJOMVhtaGIzdER5Y1UxdFRMZEZaVDZLUDFRZVExMGc3OC1RIiwiZSI6IkFRQUIifQ",
  "jti": "b08d340b24f64fe2b1a4b8af6e9458bc",
  "sub": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoicHVYb3VRS1Vha2t2X2JUZWQ4dkNYLU9FTG1jUzhqQ21DWE9WZFp2b3l5c0wxTWMyMWZGSzBxMXBIN1dMRU1hOUFhd1hQSk1sckdEdmcxT0FiS1h0TkMwZ2hHMTR2dzVqQXpieldsb3F3c25jaGlQRk5ENWt6aTNfUmNpYzlxZlpGUnN3aUdjUkNtRHNKUnlqX244MDhVZkNGdkRnYVZzVjlNNVJhMmNZMHlYQkJDM29tRkpJNXBkTEEySTFFRFZuMWJkTzFaRXQtUFY3Z3c0MWFQZHdhNzd2cTBNRkFDaTNyS0wtTEdzWkNxYlo1Q0ZsNjJFMWNYMU5KZmd1d3BoMDJHMEdRSjZIMnhBSFdnU3BkVUtXcTNQdXJrWVl3VGkwTFJXR015Mk5sSzdwVUxPMlFwem1GN2tWRmdhbW5fb0VOMFlhVkoxbVgzVE02UEVHVzZFdDFRIiwiZSI6IkFRQUIifQ"
}
```

## Credential Format

Credentials should be presented as a [Json Web Token (JWT)](https://datatracker.ietf.org/doc/html/rfc7519).

The header of the JWT should include the public JWK defined in the issuer's DID.

```json
{
  "alg": "RS256",
  "typ": "JWT",
  "jwk": {
    "kty": "RSA",
    "n": "xICdahlIZ5Zenx2yR8Tr_9gVJ-eqEg82gJwzaLWdhHwCfHqIcXSmBcWl8jJMYdDnjQtgpjoED9OBOlk8Eg-HSOyAudsAkqzKr3pG22YEFccFgA67U3jLFlt1pDh2jso9XZEKKRkrV0KfSbbU3VGKhX8vSV0xZcdgjGLF_dbIjHtXLChQxdIw0U6uUd857Tkz-srAXHIy1ycnxgLAinqy3L8SgMbIVRtB_f1La3WVY2uS2V3T4bpbGyUPQfi7JFfGhjpnA97-GB0eh30z1nBje6StDFFMZnbQQyOZIczeKKB_vChn0N0bN1Xmhb3tDycU1tTLdFZT6KP1QeQ10g78-Q",
    "e": "AQAB"
  }
}
```

The payload of the JWT should contain the credential.

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://purl.imsglobal.org/spec/ob/v3p0/context-3.0.3.json"
  ],
  "id": "b08d340b24f64fe2b1a4b8af6e9458bc",
  "type": [
    "VerifiableCredential",
    "OpenBadgeCredential"
  ],
  "issuer": {
    "id": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoieElDZGFobElaNVplbngyeVI4VHJfOWdWSi1lcUVnODJnSnd6YUxXZGhId0NmSHFJY1hTbUJjV2w4akpNWWREbmpRdGdwam9FRDlPQk9sazhFZy1IU095QXVkc0FrcXpLcjNwRzIyWUVGY2NGZ0E2N1UzakxGbHQxcERoMmpzbzlYWkVLS1JrclYwS2ZTYmJVM1ZHS2hYOHZTVjB4WmNkZ2pHTEZfZGJJakh0WExDaFF4ZEl3MFU2dVVkODU3VGt6LXNyQVhISXkxeWNueGdMQWlucXkzTDhTZ01iSVZSdEJfZjFMYTNXVlkydVMyVjNUNGJwYkd5VVBRZmk3SkZmR2hqcG5BOTctR0IwZWgzMHoxbkJqZTZTdERGRk1abmJRUXlPWkljemVLS0JfdkNobjBOMGJOMVhtaGIzdER5Y1UxdFRMZEZaVDZLUDFRZVExMGc3OC1RIiwiZSI6IkFRQUIifQ",
    "type": [
      "Profile"
    ],
    "name": "Ray Consulting Limited"
  },
  "validFrom": "2025-05-19T00:00:00Z",
  "validUntil": "2030-05-19T00:00:00Z",
  "credentialSubject": {
    "id": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoicHVYb3VRS1Vha2t2X2JUZWQ4dkNYLU9FTG1jUzhqQ21DWE9WZFp2b3l5c0wxTWMyMWZGSzBxMXBIN1dMRU1hOUFhd1hQSk1sckdEdmcxT0FiS1h0TkMwZ2hHMTR2dzVqQXpieldsb3F3c25jaGlQRk5ENWt6aTNfUmNpYzlxZlpGUnN3aUdjUkNtRHNKUnlqX244MDhVZkNGdkRnYVZzVjlNNVJhMmNZMHlYQkJDM29tRkpJNXBkTEEySTFFRFZuMWJkTzFaRXQtUFY3Z3c0MWFQZHdhNzd2cTBNRkFDaTNyS0wtTEdzWkNxYlo1Q0ZsNjJFMWNYMU5KZmd1d3BoMDJHMEdRSjZIMnhBSFdnU3BkVUtXcTNQdXJrWVl3VGkwTFJXR015Mk5sSzdwVUxPMlFwem1GN2tWRmdhbW5fb0VOMFlhVkoxbVgzVE02UEVHVzZFdDFRIiwiZSI6IkFRQUIifQ",
    "type": [
      "AchievementSubject"
    ],
    "achievement": {
      "id": "015301207aa74f5fa548ac55bb884996",
      "type": [
        "Achievement"
      ],
      "name": "Sample Verifiable Credential",
      "description": "This credential is an example of a Verifiable Credential.",
      "criteria": {
        "narrative": "To achieve this credential, a user can download this Verifiable Credential and use it for demonstration purposes."
      }
    }
  },
  "iss": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoieElDZGFobElaNVplbngyeVI4VHJfOWdWSi1lcUVnODJnSnd6YUxXZGhId0NmSHFJY1hTbUJjV2w4akpNWWREbmpRdGdwam9FRDlPQk9sazhFZy1IU095QXVkc0FrcXpLcjNwRzIyWUVGY2NGZ0E2N1UzakxGbHQxcERoMmpzbzlYWkVLS1JrclYwS2ZTYmJVM1ZHS2hYOHZTVjB4WmNkZ2pHTEZfZGJJakh0WExDaFF4ZEl3MFU2dVVkODU3VGt6LXNyQVhISXkxeWNueGdMQWlucXkzTDhTZ01iSVZSdEJfZjFMYTNXVlkydVMyVjNUNGJwYkd5VVBRZmk3SkZmR2hqcG5BOTctR0IwZWgzMHoxbkJqZTZTdERGRk1abmJRUXlPWkljemVLS0JfdkNobjBOMGJOMVhtaGIzdER5Y1UxdFRMZEZaVDZLUDFRZVExMGc3OC1RIiwiZSI6IkFRQUIifQ",
  "jti": "b08d340b24f64fe2b1a4b8af6e9458bc",
  "sub": "did:jwk:eyJrdHkiOiJSU0EiLCJuIjoicHVYb3VRS1Vha2t2X2JUZWQ4dkNYLU9FTG1jUzhqQ21DWE9WZFp2b3l5c0wxTWMyMWZGSzBxMXBIN1dMRU1hOUFhd1hQSk1sckdEdmcxT0FiS1h0TkMwZ2hHMTR2dzVqQXpieldsb3F3c25jaGlQRk5ENWt6aTNfUmNpYzlxZlpGUnN3aUdjUkNtRHNKUnlqX244MDhVZkNGdkRnYVZzVjlNNVJhMmNZMHlYQkJDM29tRkpJNXBkTEEySTFFRFZuMWJkTzFaRXQtUFY3Z3c0MWFQZHdhNzd2cTBNRkFDaTNyS0wtTEdzWkNxYlo1Q0ZsNjJFMWNYMU5KZmd1d3BoMDJHMEdRSjZIMnhBSFdnU3BkVUtXcTNQdXJrWVl3VGkwTFJXR015Mk5sSzdwVUxPMlFwem1GN2tWRmdhbW5fb0VOMFlhVkoxbVgzVE02UEVHVzZFdDFRIiwiZSI6IkFRQUIifQ"
}
```

The signature of the JWT should be created by signing the JWT with the issuer's private key.

In accordance with the JWT specification, the JWT should be derived from:

```bash
Y = Base64URLEncode(header) + '.' + Base64URLEncode(payload)

JWT token = Y + '.' + Base64URLEncode(RSASHA256(Y))
```

The JWT should be in the compact form. Only ``.jwt`` files for credentials are supported by the issuer and wallet applications.

### Example Crededential formatted as a JWT compact

```bash
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImp3ayI6eyJrdHkiOiJSU0EiLCJuIjoieElDZGFobElaNVplbngyeVI4VHJfOWdWSi1lcUVnODJnSnd6YUxXZGhId0NmSHFJY1hTbUJjV2w4akpNWWREbmpRdGdwam9FRDlPQk9sazhFZy1IU095QXVkc0FrcXpLcjNwRzIyWUVGY2NGZ0E2N1UzakxGbHQxcERoMmpzbzlYWkVLS1JrclYwS2ZTYmJVM1ZHS2hYOHZTVjB4WmNkZ2pHTEZfZGJJakh0WExDaFF4ZEl3MFU2dVVkODU3VGt6LXNyQVhISXkxeWNueGdMQWlucXkzTDhTZ01iSVZSdEJfZjFMYTNXVlkydVMyVjNUNGJwYkd5VVBRZmk3SkZmR2hqcG5BOTctR0IwZWgzMHoxbkJqZTZTdERGRk1abmJRUXlPWkljemVLS0JfdkNobjBOMGJOMVhtaGIzdER5Y1UxdFRMZEZaVDZLUDFRZVExMGc3OC1RIiwiZSI6IkFRQUIifX0.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvdjIiLCJodHRwczovL3B1cmwuaW1zZ2xvYmFsLm9yZy9zcGVjL29iL3YzcDAvY29udGV4dC0zLjAuMy5qc29uIl0sImlkIjoiYjA4ZDM0MGIyNGY2NGZlMmIxYTRiOGFmNmU5NDU4YmMiLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwiT3BlbkJhZGdlQ3JlZGVudGlhbCJdLCJpc3N1ZXIiOnsiaWQiOiJkaWQ6andrOmV5SnJkSGtpT2lKU1UwRWlMQ0p1SWpvaWVFbERaR0ZvYkVsYU5WcGxibmd5ZVZJNFZISmZPV2RXU2kxbGNVVm5PREpuU25kNllVeFhaR2hJZDBObVNIRkpZMWhUYlVKalYydzRha3BOV1dSRWJtcFJkR2R3YW05RlJEbFBRazlzYXpoRlp5MUlVMDk1UVhWa2MwRnJjWHBMY2pOd1J6SXlXVVZHWTJOR1owRTJOMVV6YWt4R2JIUXhjRVJvTW1wemJ6bFlXa1ZMUzFKcmNsWXdTMlpUWW1KVk0xWkhTMmhZT0haVFZqQjRXbU5rWjJwSFRFWmZaR0pKYWtoMFdFeERhRkY0WkVsM01GVTJkVlZrT0RVM1ZHdDZMWE55UVZoSVNYa3hlV051ZUdkTVFXbHVjWGt6VERoVFowMWlTVlpTZEVKZlpqRk1ZVE5YVmxreWRWTXlWak5VTkdKd1lrZDVWVkJSWm1rM1NrWm1SMmhxY0c1Qk9UY3RSMEl3Wldnek1Ib3hia0pxWlRaVGRFUkdSazFhYm1KUlVYbFBXa2xqZW1WTFMwSmZka05vYmpCT01HSk9NVmh0YUdJemRFUjVZMVV4ZEZSTVpFWmFWRFpMVURGUlpWRXhNR2MzT0MxUklpd2laU0k2SWtGUlFVSWlmUSIsInR5cGUiOlsiUHJvZmlsZSJdLCJuYW1lIjoiUmF5IENvbnN1bHRpbmcgTGltaXRlZCJ9LCJ2YWxpZEZyb20iOiIyMDI1LTA1LTE5VDAwOjAwOjAwWiIsInZhbGlkVW50aWwiOiIyMDMwLTA1LTE5VDAwOjAwOjAwWiIsImNyZWRlbnRpYWxTdWJqZWN0Ijp7ImlkIjoiZGlkOmp3azpleUpyZEhraU9pSlNVMEVpTENKdUlqb2ljSFZZYjNWUlMxVmhhMnQyWDJKVVpXUTRka05ZTFU5RlRHMWpVemhxUTIxRFdFOVdaRnAyYjNsNWMwd3hUV015TVdaR1N6QnhNWEJJTjFkTVJVMWhPVUZoZDFoUVNrMXNja2RFZG1jeFQwRmlTMWgwVGtNd1oyaEhNVFIyZHpWcVFYcGllbGRzYjNGM2MyNWphR2xRUms1RU5XdDZhVE5mVW1OcFl6bHhabHBHVW5OM2FVZGpVa050UkhOS1VubHFYMjQ0TURoVlprTkdka1JuWVZaelZqbE5OVkpoTW1OWk1IbFlRa0pETTI5dFJrcEpOWEJrVEVFeVNURkZSRlp1TVdKa1R6RmFSWFF0VUZZM1ozYzBNV0ZRWkhkaE56ZDJjVEJOUmtGRGFUTnlTMHd0VEVkeldrTnhZbG8xUTBac05qSkZNV05ZTVU1S1ptZDFkM0JvTURKSE1FZFJTalpJTW5oQlNGZG5VM0JrVlV0WGNUTlFkWEpyV1ZsM1ZHa3dURkpYUjAxNU1rNXNTemR3VlV4UE1sRndlbTFHTjJ0V1JtZGhiVzVmYjBWT01GbGhWa294YlZnelZFMDJVRVZIVnpaRmRERlJJaXdpWlNJNklrRlJRVUlpZlEiLCJ0eXBlIjpbIkFjaGlldmVtZW50U3ViamVjdCJdLCJhY2hpZXZlbWVudCI6eyJpZCI6IjAxNTMwMTIwN2FhNzRmNWZhNTQ4YWM1NWJiODg0OTk2IiwidHlwZSI6WyJBY2hpZXZlbWVudCJdLCJuYW1lIjoiU2FtcGxlIFZlcmlmaWFibGUgQ3JlZGVudGlhbCIsImRlc2NyaXB0aW9uIjoiVGhpcyBjcmVkZW50aWFsIGlzIGFuIGV4YW1wbGUgb2YgYSBWZXJpZmlhYmxlIENyZWRlbnRpYWwuIiwiY3JpdGVyaWEiOnsibmFycmF0aXZlIjoiVG8gYWNoaWV2ZSB0aGlzIGNyZWRlbnRpYWwsIGEgdXNlciBjYW4gZG93bmxvYWQgdGhpcyBWZXJpZmlhYmxlIENyZWRlbnRpYWwgYW5kIHVzZSBpdCBmb3IgZGVtb25zdHJhdGlvbiBwdXJwb3Nlcy4ifX19LCJpc3MiOiJkaWQ6andrOmV5SnJkSGtpT2lKU1UwRWlMQ0p1SWpvaWVFbERaR0ZvYkVsYU5WcGxibmd5ZVZJNFZISmZPV2RXU2kxbGNVVm5PREpuU25kNllVeFhaR2hJZDBObVNIRkpZMWhUYlVKalYydzRha3BOV1dSRWJtcFJkR2R3YW05RlJEbFBRazlzYXpoRlp5MUlVMDk1UVhWa2MwRnJjWHBMY2pOd1J6SXlXVVZHWTJOR1owRTJOMVV6YWt4R2JIUXhjRVJvTW1wemJ6bFlXa1ZMUzFKcmNsWXdTMlpUWW1KVk0xWkhTMmhZT0haVFZqQjRXbU5rWjJwSFRFWmZaR0pKYWtoMFdFeERhRkY0WkVsM01GVTJkVlZrT0RVM1ZHdDZMWE55UVZoSVNYa3hlV051ZUdkTVFXbHVjWGt6VERoVFowMWlTVlpTZEVKZlpqRk1ZVE5YVmxreWRWTXlWak5VTkdKd1lrZDVWVkJSWm1rM1NrWm1SMmhxY0c1Qk9UY3RSMEl3Wldnek1Ib3hia0pxWlRaVGRFUkdSazFhYm1KUlVYbFBXa2xqZW1WTFMwSmZka05vYmpCT01HSk9NVmh0YUdJemRFUjVZMVV4ZEZSTVpFWmFWRFpMVURGUlpWRXhNR2MzT0MxUklpd2laU0k2SWtGUlFVSWlmUSIsImp0aSI6ImIwOGQzNDBiMjRmNjRmZTJiMWE0YjhhZjZlOTQ1OGJjIiwic3ViIjoiZGlkOmp3azpleUpyZEhraU9pSlNVMEVpTENKdUlqb2ljSFZZYjNWUlMxVmhhMnQyWDJKVVpXUTRka05ZTFU5RlRHMWpVemhxUTIxRFdFOVdaRnAyYjNsNWMwd3hUV015TVdaR1N6QnhNWEJJTjFkTVJVMWhPVUZoZDFoUVNrMXNja2RFZG1jeFQwRmlTMWgwVGtNd1oyaEhNVFIyZHpWcVFYcGllbGRzYjNGM2MyNWphR2xRUms1RU5XdDZhVE5mVW1OcFl6bHhabHBHVW5OM2FVZGpVa050UkhOS1VubHFYMjQ0TURoVlprTkdka1JuWVZaelZqbE5OVkpoTW1OWk1IbFlRa0pETTI5dFJrcEpOWEJrVEVFeVNURkZSRlp1TVdKa1R6RmFSWFF0VUZZM1ozYzBNV0ZRWkhkaE56ZDJjVEJOUmtGRGFUTnlTMHd0VEVkeldrTnhZbG8xUTBac05qSkZNV05ZTVU1S1ptZDFkM0JvTURKSE1FZFJTalpJTW5oQlNGZG5VM0JrVlV0WGNUTlFkWEpyV1ZsM1ZHa3dURkpYUjAxNU1rNXNTemR3VlV4UE1sRndlbTFHTjJ0V1JtZGhiVzVmYjBWT01GbGhWa294YlZnelZFMDJVRVZIVnpaRmRERlJJaXdpWlNJNklrRlJRVUlpZlEifQ.S4VDYLi4SviluK8IBdeE4SLTUFCk1OMQLRmp6zI5RK8ZTM3TbgXUWeOTUX6C5NtO7EaNx0wXmbqEGUkoiU9kY_dutKF1Kv2DG4MTqwNcU_skivo2Dt9g1atBRlF5Al4aEpIqThRKf0U2LWe80dvKwODki2TI1_kxsochleNLPETzrbqB9bMbiQ6JcKOsvkV8puIuGzuDdlhmmyH7wG1ySFy4bsPq8DoiBW_hRMJSxuW1go71v4Di2HxoqZuV9nJUO-vNvApiGYw3eSTzwTvV-TH7mdBlvxEXa3-42FreJQiQ7bsK48WqQ1jllGVoJYYE1FKEV-0rpEWYlIl3Shx7lg
```

The file extension for the credential in JWT compact format should be ``.jwt``.

Holders should be able to download their credential as a ``.jwt`` file from the issuer application.

The file can then be uploaded to the wallet application. Alternatively, the credential file can be programtically saved to the wallet application using the ``Credentials API``.

## Wallet Credentials API

A holder using an external issuer application can programtically save a credential file to the wallet application using the wallet's ``Credentials API``.

### Authorization Server

The issuer application must use an authorization server to obtain a JWT access token to use the ``Credentials API``. 

The holder will first login to their account with the authorization server and obtain a JWT access token. The holder must login with the same account that they use to login to the wallet application. Authorization should follow the [OAuth 2](https://datatracker.ietf.org/doc/html/rfc6749) ``Authorization Code`` flow method.

### POST Request

The issuer application should then make a ``POST`` request to the following endpoint:

```bash
https://<api-endpoint>/api/v1/credentials
```

The request must inlcude the JWT Access Token obtained from the authorization server in the Authorization header as a bearer token.

The credential in JWT compact format should be placed in the body of the request with the content type of ``text/plain``.

### Example Request

```bash
POST /api/v1/credentials HTTP/1.1
Host: localhost:7028
Content-Type: text/plain
Authorization: eyJsjejdbbf.....
Content-Length: 4653

eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImp3ayI6eyJrdHkiOiJSU0EiLCJuIjoieElDZGFobElaNVplbngyeVI4VHJfOWdWSi1lcUVnODJnSnd6YUxXZGhId0NmSHFJY1hTbUJjV2w4akpNWWREbmpRdGdw
```

### Response

If the credential does not exist in the wallet, a new credential will be saved in the wallet and a ``201`` response will be returned with the credential in jwt compact format in the body of the request with content type ``text plain``.

If the credential does exist, it will be updated and a ``200`` response will be returned in the response with the credential in the body.
