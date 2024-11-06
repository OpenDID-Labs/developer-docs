# <img src="logo.svg" alt="OpenDID" height="40px">


  <a href="https://circleci.com/gh/Open-Attestation/token-registry/tree/master" alt="Circle CI"><img src="https://img.shields.io/circleci/build/github/Open-Attestation/token-registry/master" /></a>
  <a href="https://codecov.io/gh/Open-Attestation/token-registry" alt="Code Coverage"><img src="https://codecov.io/gh/Open-Attestation/token-registry/branch/master/graph/badge.svg?token=Y4R9SWXATG" /></a>
  <a href="https://www.npmjs.com/package/@govtechsg/token-registry" alt="NPM"><img src="https://img.shields.io/npm/dw/@govtechsg/token-registry" /></a>
  <img src="https://img.shields.io/github/license/open-attestation/token-registry" />

**应用合约调用OpenDID Oracle合约的请求和应答`data`数据域的数据结构说明。** `data`整体是一个`string`类型的值，该值是符合[RFC 8259](https://dl.acm.org/doi/10.17487/RFC8259)标准的一个JSON结构。以下是两个完整的`data`数据域的示例。

- {"website": "www.opendid.io","email": "contact@opendid.io"}
- {"result": "true"}

## Table of Contents

- [JobID汇总](#installation)
- [参数说明](#deployment)
    - [Terminal3 Identity](#Terminal3-Identity)
      - [获取用户个人资料](#获取用户个人资料)
      - [获取用户钱包地址](#获取用户钱包地址)
      - [获取用户DID签发者](#获取用户DID签发者)
    - [Passport XYZ](#Passport-XYZ)
      - [验证passport持有者](#验证passport持有者)
    - [ENS](#ENS)
      - [待补充](#待补充)
    - [Privado ID](#Privado-ID)
      - [查询验证结果](#查询验证结果)      
    - [HashKey DID](#HashKey-DID)
      - [验证HashKey DID持有者](#验证HashKey-DID持有者)
      - [查询DID文档](#查询DID文档)
    - [Farcaster ID](#Farcaster-ID)
      - [验证签名密钥请求](#验证签名密钥请求)
      - [注册ID](#注册ID)
    - [World ID](#World-ID)
      - [验证World ID证明](#验证World-ID证明)
    - [China RealDID](#China-RealDID)
      - [查验DID](#查验DID)
      - [查询DID文档](#查询DID文档)

## JobID汇总

以下表格是目前OpenDID Oracle可支持的所有JobID的汇总：

|              JobID                   |         业务分类                                                               |      ID System Name    |   接入方式  |
| ------------------------------------ | ---------------------------------------------------|----------------------- | ---------- |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Real-name Authentication Service                   | `Terminal3 Identity`   |    API     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `Terminal3 Identity`   |    API     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Verifiable Credential Issuer Verification Service  | `Terminal3 Identity`   |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | identity Registration Service                      | `Terminal3 Identity`   |    API     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `Passport XYZ`         |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `ENS`                  |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Encrypted PII Verification Service                 | `Privado ID`           |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `HashKey DID`          |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | DID Document Retrieval Service                     | `HashKey DID`          |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `Farcaster ID`         |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | identity Registration Service                      | `Farcaster ID`         |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `World ID`             |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Real-name Authentication Service                   | `China RealDID`        |    API     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | DID Document Retrieval Service                     | `China RealDID`        |    API     |


> **Note:** 接入方式为 API 表示 OpenDID Oracle 通过 ID System 提供的相应 API 进行的验证，合约表示 OpenDID Oracle 通过调用 ID System 提供的智能合约相关方法进行的验证。


## Terminal3 Identity

### 获取用户个人资料

用于获取加密的用户资料数据，需要用户私钥进行解密。


- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Real-name Authentication Service 

- 接口/合约地址：（待补充）

- 方法名称：（待补充）

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `version`                    | string           |       Y         |    用户签名生成的版本          |
| `nonce`                     | string            |       Y         |   用户签名生成的nonce        |
| `ephem_public_key`    | string            |       Y        |    用户签名生成的ephem公钥 |
| `ciphertext`                | string            |       Y        |    用户签名生成的密文            |

- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `data`                    | object           |       Y         |    数据          |

- data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `encrypted_profile`                    | object           |       Y         |    加密的用户资料数据          |

- encrypted_profile：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `version`                    | string           |       Y         |    用户签名生成的版本          |
| `nonce`                     | string            |       Y         |   用户签名生成的nonce        |
| `ephem_public_key`    | string            |       Y        |    用户签名生成的ephem公钥 |
| `ciphertext`                | string            |       Y        |    用户签名生成的密文            |


### 获取用户钱包地址

用于获取用户所有钱包地址。


- JobID：a795b5cb935f49b68b47687e0071751e

- 业务分类：Account Holder Authentication Service

- 接口/合约地址：https://staging.terminal3.io/v1/user/{user_id}/

- 方法名称：wallet_addresses

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `user_id`                    | int32           |       Y         |    用户ID          |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
|                    | json           |       Y         |    数据          |


### 获取用户DID签发者

用于获取用户所有钱包地址。


- JobID：a795b5cb935f49b68b47687e0071751e

- 业务分类：Verifiable Credential Issuer Verification Service

- 接口/合约地址：Ethereum/0xdcD6e6169aB842A9f9740E12d1DBAf87F2B16777(T3DIDRegistry)

- 方法名称：issuers

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | string           |       Y         |    用户ID          |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
|          `issuerAddress`           | address           |       Y         |   签发者地址          |
|          `string`           | string           |       Y         |   签发者名称（ENS域名）       |




## Passport XYZ

### 验证passport持有者

验证签名、并根据账户地址查询该账户是否持有passport

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 接口/合约地址：Arbitrum/0x2050256A91cbABD7C42465aA0d5325115C1dEB43(GitcoinPassportDecoder)

- 方法名称：getPassport

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `signature`                    | string           |       Y         |    对address的签名数据          |
| `address`                     | address            |       Y         |   签名原文（签名数据私钥钱包地址）        |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |




## ENS

### 待补充

待补充。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 接口/合约地址：待补充

- 方法名称：待补充

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `signature`                    | string           |       Y         |    使用私钥将域名签名后的数据          |
| `domain`                     | string            |       Y         |   需要验证的域名        |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |




## Privado ID

### 查询验证结果

查询用户提交的ZK Proof是否验证通过。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Encrypted PII Verification Service 

- 接口/合约地址：Polygon/0x394d1dad46907bd54d15926A1ab4535EF2BF47b1(UniversalVerifier)

- 方法名称：isProofVerified

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `sender`                    | address           |       Y         |    待查询用户的钱包地址          |
| `requestId`                     | uint64            |       Y         |   验证方设置的验证请求ID        |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |



## HashKey DID

### 验证HashKey DID持有者

根据钱包地址和签名值验证用户是否是HashKey DID持有者。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 接口/合约地址：PlatON/0x7fDd3f96cBDE51737A9E24b461E7E92A057C3BBf(Did)

- 方法名称：addrClaimed

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `signature`                    | string           |       Y         |    使用钱包私钥对account参数的签名值          |
| `account`                     | address            |       Y         |   钱包地址       |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |



### 查询DID文档

根据HashKey DID标识符查询DID文档。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：DID Document Retrieval Service

- 接口/合约地址：https://api.hashkey.id/did/api/did/{did}

- 方法名称：N/A

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | string           |       Y         |    DID标识符         |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | string           |       Y         |    DID文档          |




## Farcaster ID

### 验证签名密钥请求

来验证您的应用创建的签名密钥请求。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 接口/合约地址：Optimism/0x00000000fc700472606ed4fa22623acf62c60553(SignedKeyRequestValidator)

- 方法名称：validate

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `fid`                    | uint256           |       Y         |    与公钥关联的主fid         |
| `key`                     | bytes            |       Y         |   要验证的公钥       |
| `sig`                     | bytes            |       Y         |   请求密钥的实体的EIP-712签名      |

- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |



### 注册ID

将新的 fid 注册到特定地址并支付存储费用。接收地址必须签署 EIP-712 注册消息以批准注册。接收者必须尚未拥有 fid。注册一个新的FID，调用者必须尚未拥有 fid。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：identity Registration Service

- 接口/合约地址：Optimism\0x00000000fc25870c6ed6b6c7e41fb078b7656f69(IdGateway)

- 方法名称：registerFor

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `weiValue`                    | wei           |       Y         |    注册需支付的金额        |
| `to`                     | address            |       Y         |   注册 fid 的地址       |
| `recovery`                     | address            |       Y         |   新 fid 的恢复地址    |
| `deadline`                    | uint256           |       Y         |    签名过期时间戳        |
| `sig`                     | bytes            |       Y         |  来自 to 地址的EIP-712签名       |
| `extraStorage`                     | uint256           |       N        |   额外的存储单元    |

- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    注册的结果          |



## World ID

### 验证World ID证明

待补充。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 接口/合约地址：Ethereum/0x163b09b4fe21177c455d850bd815b6d583732432 (WorldIDRouter)

- 方法名称：verifyProof

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `root`                    | uint256           |       Y         |    要验证的World ID根       |
| `groupId`                     | uint256            |       Y         |   要验证的凭证类型。由于链上仅支持 Orb 凭证（虹膜），因此该值必须为 1。      |
| `signalHash`                     | uint256            |       Y         |   要验证的信号的 keccak256 哈希值。      |
| `nullifierHash`                    | uint256           |       Y         |    此操作的匿名用户 ID。此 ID 是从 IDKit 小部件获取的，应按原样传递。       |
| `externalNullifierHash`                     | uint256            |       Y         |   用于标识用户正在验证哪个应用程序和操作       |
| `proof`                     | uint256[8]            |       Y         |   要验证的零知识证明。这是从 IDKit 小部件获取的十六进制字符串证明。     |

- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |




## China RealDID

### 查验DID

使用China RealDID根据用户的实名信息查验对应的DID。


- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Real-name Authentication Service 

- 接口/合约地址：https://openapi-ctid.bsnbase.com/

- 方法名称：rais/credential/userinfo/verify

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `authApplyRetainData`                    | string           |       Y         |    个人用户信息加密字段          |
| `publicKeyIndex`                     | Integer            |       N         |   实名DID文档中的公钥索引       |
| `authHash`    | string            |       Y        |    使用SM3算法对姓名+身份证号进行哈希计算后的哈希值 |
| `ciphertext`                | string            |       Y        |    用户签名生成的密文            |

- authApplyRetainData说明：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `idNo`                    | String           |       Y         |    个人用户的身份证号码         |
| `name`                    | String           |       Y         |    个人用户身份证上的姓名         |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | String           |       Y         |    实名DID标识          |
| `publicKey`                    | String           |       N         |    publicKeyIndex在DID文档中对应的公钥          |



### 查询DID文档

根据实名DID标识符查询DID文档。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：DID Document Retrieval Service

- 接口/合约地址：https://openapi-ctid.bsnbase.com/

- 方法名称：rais/did/getDocument

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | string           |       Y         |    DID标识符         |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `didDocument`                    | string           |       Y         |    DID文档          |
| `status`                    | string           |       Y         |    DID状态。0：正常，1：注销        |


## Legal

Your use of this Project is governed by the terms found at www.opendid.io (the "Terms").

## Community
Contact us: contact@opendid.io

Website: www.opendid.io
