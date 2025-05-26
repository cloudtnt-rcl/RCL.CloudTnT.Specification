# RCL cloudTnT Specification

## Introduction

This specification outlines the technical design for the for the [W3C Verifiable Credentials](https://www.w3.org/TR/vc-overview/) and [Open Badges 3](https://www.imsglobal.org/spec/ob/v3p0) issuer and wallet RCL cloudTnT applications. 

Applications that interchange data with the RCL cloudTnT application must follow the requirements of this specifications. Such applications may include external issuer applications that create digital credentials that are to be stored in the RCL cloudTnT wallet application.

### Issuer Application

The issuer application is used by Issuers to issue digital credentials following the requirements of the W3C Verifiable Credentials and Open Badges 3 standard.

### Wallet Application

The wallet application is used by Holders to store and share their digital credentials with verifiers.

## Roles

### Issuer

A issuer issues digital credentials to holders to recognize their achievement. Examples : training provider, organization, assessor.

### Holder

A holder earns a digital credentials as a recognition of his/her achievement. Examples : student, apprentice, employee.

### Verifier

A holder will share their digital credential with a verifier who will make a determination on the validity of the credential. Example : employer. 

## Decentralized Identifiers

[W3C Decentralized Identifiers (DIDs)](https://www.w3.org/TR/did-1.1/) are used to uniquely identify Issuers and Holders.

DIDs are created using the [did:jwk method](https://github.com/quartzjer/did-jwk/blob/main/spec.md). This is the ony method supported.

When creating a did:jwk, the [RSA](https://datatracker.ietf.org/doc/html/rfc8017) cryptographic algorithm should be used with a 2048 bit key size. Only RSA asymmetric keys are supported.

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

In the wallet application, the did private key for the holder should not be stored in the wallet database or other storage system. Th private should be controlled soley by the holder. No external party should have access to the holder's private key.

When a holder adds an external DID to the wallet application, they must upload the DID as a text (.txt) file, as well as, use their private key to verify that they own the DID. The wallet application should only use the private key to verify the DID and should not store the private key in the database.

In the issuer application, the did private key for the holder can be stored in the application's database once the issuer has full and sole control of the issuer application. No external party should have access to the issuer's private key.

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

