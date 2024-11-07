# <img src="logo.svg" alt="OpenDID" height="40px">


[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://opendid.io)
[![license](https://img.shields.io/badge/license-MIT-red.svg)](https://mit-license.org/)
[![Docs](https://img.shields.io/badge/docs-%F0%9F%93%84-yellow)](https://github.com/OpenDID-Labs/developer-docs)



**This document is an explanation of the data structure of `data` in the request and response of the OpenDID Oracle contract called by the application contract.** The `data` is a value of type `string`. It is a standard JSON structure that conforms to [RFC 8259]( https://dl.acm.org/doi/10.17487/RFC8259). Here are two examples of data fields of `data`.

- {"website": "www.opendid.io","email": "contact@opendid.io"}
- {"result": "true"}

## Table of Contents

- [JobID Summary](#jobid-summary)
- [Passport XYZ](#passport-xyz)
  - [Verify the Holder](#verify-the-holder)
- [ENS](#ens)
  - [Verify the Holder](#verify-the-holder)
- [Privado ID](#privado-id)
  - [Query Verification Results](#query-verification-results)      
- [HashKey DID](#hashkey-did)
  - [Verify the Holder](#verify-the-holder)
  - [Query DID](#query-did)
- [Farcaster ID](#farcaster-id)
  - [Verify the Holder](#verify-the-holder)
  - [Register ID](#register-id)
- [Terminal3 Identity](#terminal3-identity)
  - [Verify User Information](#verify-user-information)
  - [Verify User's Wallet Address](#verify-user's-wallet-address)
  - [Verify the Issuer's Identity](#verify-the-issuer's-identity)
- [World ID](#world-id)
  - [Verify the World ID](#verify-the-world-id)
- [China RealDID](#china-realdid)
  - [Verify the DID](#verify-the-did)
  - [Query DID Document](#query-did-document)



## JobID Summary

The following table is a summary of all JobIDs currently supported by OpenID Oracle:

|              JobID                   |         Service Type                               |      ID System Name    |   Access Method  |
| ------------------------------------ | ---------------------------------------------------|----------------------- | ---------- |
| `5e4dd148f004f6790bd54d5f7fbd0a38`   | Account Holder Authentication Service              | `Passport XYZ`         |    Contract     |
| `1028e09602ca4bdd89a6bba67212001f`   | Account Holder Authentication Service              | `ENS`                  |    Contract     |
| `d1926a6028d746c89e791bf7ac1b028b`   | Encrypted PII Verification Service                 | `Privado ID`           |    Contract     |
| `05b3af7569d74983bd0895ff273ffc6f`   | Account Holder Authentication Service              | `HashKey DID`          |    Contract     |
| `152872f465eb47e09a8047359055e152`   | DID Document Retrieval Service                     | `HashKey DID`          |    API     |
| `e9692ce7e0de4c03b1d816f42f55b44d`   | Account Holder Authentication Service              | `Farcaster ID`         |    Contract     |
| `8e93566e07fd44ba8ba002f0fe4e7eb8`   | identity Registration Service                      | `Farcaster ID`         |    Contract     |
| `48fee62acfaf46f693edce89860369d5`   | Real-name Authentication Service                   | `Terminal3 Identity`   |    API     |
| `a795b5cb935f49b68b47687e0071751e`   | Account Holder Authentication Service              | `Terminal3 Identity`   |    API     |
| `509e3db7758f4b61ba35036575f3f3f0`   | Verifiable Credential Issuer Verification Service  | `Terminal3 Identity`   |    Contract     |
| `910529f3dc394bbcad0b3ddd656d2be5`   | Account Holder Authentication Service              | `World ID`             |    Contract     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Real-name Authentication Service                   | `China RealDID`        |    API     |
| `785bc6ee5a0c4feb9c422cdc233c510c`   | DID Document Retrieval Service                     | `China RealDID`        |    API     |


> [!WARNING]
> If the access method is API, it means that the verification performed by OpenDID Oracle is through calling the APIs provided by the ID System. If the access method is contract, it means that the verification performed by OpenDID Oracle is through calling the functions of the smart contract provided by the ID System.


## Passport XYZ

### Verify the Holder

Verify whether the account holds a passport by verifying the signature of the wallet address signed by the corresponding private key with secp256k1 algorithm.

- JobID：5e4dd148f004f6790bd54d5f7fbd0a38

- Service type：Account Holder Authentication Service 

- Reference：Passport XYZ [getPassport](https://docs.passport.xyz/building-with-passport/smart-contracts/contract-reference) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `address `   | string      |       Y     |   User's wallet address                  |
| `signature`  | string      |       Y     |   The signature of address signed by the private key   |


- Oracle Response Data：


|           Name                |       Type      |      Required      |                     Description               |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |     Verification result              |



## ENS

### Verify the Holder

Verify whether the the user is the holder of the domain by verifying the signature of the domain signed by the corresponding private key with secp256k1 algorithm.

- JobID：1028e09602ca4bdd89a6bba67212001f

- Service type：Account Holder Authentication Service 

- Reference：ENS [owner](https://docs.ens.domains/learn/deployments) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| --------------|-------------|-----------|--------------------------|
| `domain`      | string      |       Y   |   Domain                           |
| `signature`   | string      |       Y   |   The signature of domain signed by the private key   |


- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |    Verification result              |




## Privado ID

### Query Verification Results

For validators who already know the values of `requestId` and `sender`, they can verify whether the user has already been verified to hold a certain VC.

- JobID：d1926a6028d746c89e791bf7ac1b028b

- Service type：Encrypted PII Verification Service 

- Reference：Privado ID [isProofVerified](https://docs.privado.id/docs/smart-contracts) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------|--------|--------|--------------------------|
| `requestId `      | uint64     |       Y     |           Request ID                           |
| `sender `         | address   |       Y     |              The user's wallet address to be queried   |



- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |    Verification result              |




## HashKey DID

### Verify the Holder

Verify whether the account holds HashKey DID by verifying the signature of the wallet address signed by the corresponding private key with secp256k1 algorithm.

- JobID：05b3af7569d74983bd0895ff273ffc6f

- Service type：Account Holder Authentication Service 

- Reference：HashKey DID [addrClaimed](https://docs.hashkey.id/protocol/deployments) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| -------------|-------------|-------------|------------------------|
| `account `   | address     |       Y     |   Wallet address              |
| `signature`  | string      |       Y     |   The signature of account signed by the private key   |


- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |    Verification result             |



### Query DID

Query relevant data based on the user's HashKey DID identifier.

- JobID：152872f465eb47e09a8047359055e152

- Service type：DID Document Retrieval Service

- Reference：HashKey DID [Get DID info](https://docs.hashkey.id/developers/api-reference/openapi) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------| ----------|----------| --------------------|
| `did`       | string    |       Y  |    DID identifier         |


- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
|-----------|--------------|---------------|--------------|
| `result`  | string       |       Y       |    Related data     |




## Farcaster ID

### Verify the Holder

Verify whether the account holds a Farcaster ID by using the FID, public key, and EIP-712 signature signed by the private key.

- JobID：e9692ce7e0de4c03b1d816f42f55b44d

- Service type：Account Holder Authentication Service 

- Reference：Farcaster ID [validate](https://docs.farcaster.xyz/reference/contracts/reference/signed-key-request-validator) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `fid`                    | uint256           |       Y         |    The primary FID corresponding to the public key          |
| `key`                     | bytes            |       Y         |   The public key to be verified               |
| `sig`                     | bytes            |       Y         |   The signature of EIP-712 entity             |


- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |   Verification result           |



### Register ID

Users who have not yet owned a FID can register a new FID.

> [!WARNING]
> You need to register the new FID at a specific address and pay the storage fee. The receiving address must sign the EIP-712 registration message to approve registration. The recipient must not yet possess the FID.

- JobID：8e93566e07fd44ba8ba002f0fe4e7eb8

- Service type：identity Registration Service

- Reference：Farcaster ID [registerFor](https://docs.farcaster.xyz/reference/contracts/reference/id-gateway) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `weiValue`                    | wei           |       Y         |    The amount to be paid for registration        |
| `to`                     | address            |       Y         |   The address to register the FID       |
| `recovery`                     | address            |       Y         |   New FID's recovery address    |
| `deadline`                    | uint256           |       Y         |    The timestamp of the deadline of the signature         |
| `sig`                     | bytes            |       Y         |  The EIP-712 signature of the receiving address       |
| `extraStorage`                     | uint256           |       N        |   Extra storage unit    |

- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    Rgistration result         |




## Terminal3 Identity


### Verify User Information




- JobID：48fee62acfaf46f693edce89860369d5

- Service type：Real-name Authentication Service

- Reference：Terminal3 Identity

- Oracle Request Data：

- Oracle Response Data：




### Verify User's Wallet Address

By verifying the userId, walletAddress, and the signature of walletAddress signed by the corresponding private key with secp256k1 algorithm to confirm that the user is indeed the holder of that userId.

- JobID：a795b5cb935f49b68b47687e0071751e

- Service type：Account Holder Authentication Service

- Reference：Terminal3 Identity [Get Wallet Addresses](https://terminal3.readme.io/reference/get-user-wallet-addresses) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `userId`                    | int32           |       Y         |    User ID          |
| `walletAddress`                    | string           |       Y         |    User's wallet address         |
| `signature`                    | string           |       Y         |    The signature of WalletAddress signed by the private key of the user's wallet address        |

- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
|          result          | boolean           |       Y         |    Verification Result          |



### Verify the Issuer's Identity

Verify the issuer's identity of the user ID through the user ID and issuer address.


- JobID：509e3db7758f4b61ba35036575f3f3f0

- Service type：Verifiable Credential Issuer Verification Service

- Reference：Terminal3 Identity [Create User](https://terminal3.readme.io/reference/create-user-from-client) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | string           |       Y         |    User ID          |
| `issuerAddress`                    | address           |       Y         |   The issuer's address to be verified         |


- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
|          result          | boolean           |       Y         |    Verification Result          |




## World ID

### Verify the World ID

Verify whether the World ID is the actual person through the World ID root and proof data.

- JobID：910529f3dc394bbcad0b3ddd656d2be5

- Service type：Account Holder Authentication Service 

- Reference：World ID [verifyProof](https://docs.world.org/world-id/reference/contracts) function.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `root`                    | uint256           |       Y         |    The World ID root to be verified       |
| `groupId`                     | uint256            |       Y         |   The credential type to be verified. Currently is `1` only     |
| `signalHash`                     | uint256            |       Y         |   The  keccak256 hash value of the signal to be verified      |
| `nullifierHash`                    | uint256           |       Y         |    The anonymous User ID      |
| `externalNullifierHash`                     | uint256            |       Y         |   The keccak256 hash value that identifies the application and operation being verified by the user       |
| `proof`                     | uint256[8]            |       Y         |   The zero-knowledge-proof data to verify the hexadecimal string obtained from IDKit    |

- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |   Verification result           |




## China RealDID

### Verify the DID

Use the public key of China RealDID to encrypt the user's `name` + `ID number` using the sm2p256v1 algorithm, and use the sm3 algorithm to hash the string of `name` + `ID number` to check whether the user has applied for a China RealDID. When requesting, you can also pass in the public key index in the DID document. If the user has applied for a RealDID, the corresponding public key value of that index will be returned at the same time.


- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- Service type：Real-name Authentication Service 

- Reference：The `Query DID` interface of [China RealDID](https://did.bsnbase.com) API document.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `authApplyRetainData`                    | string           |       Y         |    Natrual person user information encryption field          |
| `publicKeyIndex`                     | Integer            |       N         |   Public key index in realName DID document       |
| `authHash`    | string            |       Y        |    The hash value after hashing `name` + `ID number` using SM3 algorithm |


- authApplyRetainData：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `idNo`                    | String           |       Y         |    User's ID number         |
| `name`                    | String           |       Y         |    User's real name on the ID card         |


- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | String           |       Y         |    RealDID identifer          |
| `publicKey`                    | String           |       N         |    The publicKeyIndex corresponds to the public key in the DID document          |



### Query DID Document

Use the RealDID identifier to query the corresponding DID document and status.

- JobID：785bc6ee5a0c4feb9c422cdc233c510c

- Service type：DID Document Retrieval Service

- Reference：The `Query and Downlad DID Document` interface of [China RealDID](https://did.bsnbase.com) API document.

- Oracle Request Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | string           |       Y         |    RealDID identifier         |


- Oracle Response Data：

|           Name                |       Type      |      Required      |                     Description               |
| ------------------| ----------|----------| ----------------------|
| `didDocument`                    | string           |       Y         |   DID document          |
| `status`                    | string           |       Y         |   0: Normal，1: Revoked        |


## Legal

Your use of this Project is governed by the terms found at www.opendid.io (the "Terms").

## Community
Contact us: contact@opendid.io

Website: www.opendid.io
