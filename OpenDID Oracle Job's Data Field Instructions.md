# <img src="logo.svg" alt="OpenDID" height="40px">


[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://opendid.io)
[![license](https://img.shields.io/badge/license-MIT-red.svg)](https://mit-license.org/)
[![Docs](https://img.shields.io/badge/docs-%F0%9F%93%84-yellow)](https://github.com/OpenDID-Labs/developer-docs)



**应用合约调用OpenDID Oracle合约的请求和应答`data`数据域的数据结构说明。** `data`整体是一个`string`类型的值，该值是符合[RFC 8259](https://dl.acm.org/doi/10.17487/RFC8259)标准的一个JSON结构。以下是两个完整的`data`数据域的示例。

- {"website": "www.opendid.io","email": "contact@opendid.io"}
- {"result": "true"}

## Table of Contents

- [JobID汇总](#installation)
- [参数说明](#deployment)
    - [Terminal3 Identity](#Terminal3-Identity)
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

|              JobID                   |         业务分类                                    |      ID System Name    |   接入方式  |
| ------------------------------------ | ---------------------------------------------------|----------------------- | ---------- |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `Terminal3 Identity`   |    API     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Verifiable Credential Issuer Verification Service  | `Terminal3 Identity`   |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `Passport XYZ`         |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `ENS`                  |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Encrypted PII Verification Service                 | `Privado ID`           |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `HashKey DID`          |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | DID Document Retrieval Service                     | `HashKey DID`          |    API     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `Farcaster ID`         |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | identity Registration Service                      | `Farcaster ID`         |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Account Holder Authentication Service              | `World ID`             |    合约     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | Real-name Authentication Service                   | `China RealDID`        |    API     |
| `9330d9fc54ab48ada8373493b0ef9cf3`   | DID Document Retrieval Service                     | `China RealDID`        |    API     |


> **Note:** 接入方式为 API 表示 OpenDID Oracle 通过 ID System 提供的相应 API 进行的验证，合约表示 OpenDID Oracle 通过调用 ID System 提供的智能合约相关方法进行的验证。


## Terminal3 Identity


### 验证用户钱包地址

通过userId、walletAddress以及钱包地址的私钥对walletAddress进行Secp256k1算法的签名，验证该用户的确是userId的Holder。


- JobID：a795b5cb935f49b68b47687e0071751e

- 业务分类：Account Holder Authentication Service

- 参考资料：Terminal3 Identity [Get Wallet Addresses](https://terminal3.readme.io/reference/get-user-wallet-addresses)方法。

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `userId`                    | int32           |       Y         |    用户ID          |
| `walletAddress`                    | string           |       Y         |    用户钱包地址         |
| `signature`                    | string           |       Y         |    用户钱包地址的私钥对WalletAddress的签名值        |

- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
|          result          | boolean           |       Y         |    验证结果          |


### 验证签发者身份

通过用户ID和签发者地址，验证用户ID的签发者身份。


- JobID：a795b5cb935f49b68b47687e0071751e

- 业务分类：Verifiable Credential Issuer Verification Service

- 参考资料：Terminal3 Identity [Create User](https://terminal3.readme.io/reference/create-user-from-client)方法。

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | string           |       Y         |    用户ID          |
| `issuerAddress`                    | address           |       Y         |    要验证的签发者地址         |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
|          result          | boolean           |       Y         |    验证结果          |



## Passport XYZ

### 验证持有者

通过用户的链账户地址，以及对应的私钥对链账户地址采用Secp256k1算法进行签名，验证该账户是否持有passport。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 参考资料：Passport XYZ [getPassport](https://docs.passport.xyz/building-with-passport/smart-contracts/contract-reference)方法。

- Oracle Request Data：

|       名称     |     类型    |      必传  |         描述    |
| ------------------| ----------|----------| ----------------------|
| `address `   | string      |       Y     |   用户链账户地址                  |
| `signature`  | string      |       Y     |   用户私钥对address的签名值   |



- Oracle Response Data：


|       名称     |     类型    |      必传  |         描述              |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |    验证的结果              |




## ENS

### 

通过domain以及使用私钥对domain内容进行Secp256k1算法的签名，验证该用户的确是domain的Holder。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 参考资料：ENS [owner](https://docs.ens.domains/learn/deployments)方法。

- Oracle Request Data：

|       名称     |     类型    |      必传  |         描述              |
| --------------|-------------|-----------|--------------------------|
| `domain`      | string      |       Y   |   域名                          |
| `signature`   | string      |       Y   |   用户私钥对domain的签名值   |


- Oracle Response Data：

|       名称     |     类型    |      必传  |         描述              |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |    验证的结果              |




## Privado ID

### 查询验证结果

对于已经知道`requestId`和`sender` 值的验证方，可以查验该用户是否已经做过了持有某个VC的验证。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Encrypted PII Verification Service 

- 参考资料：Privado ID [isProofVerified](https://docs.privado.id/docs/smart-contracts)方法。

- Oracle Request Data：

|       名称          |     类型    |      必传  |         描述                                |
| ------------|--------|--------|--------------------------|
| `requestId `      | uint64     |       Y     |            请求ID                          |
| `sender `         | address   |       Y     |              待查询用户的钱包地址   |



- Oracle Response Data：

|       名称     |     类型    |      必传  |         描述              |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |    验证的结果              |



## HashKey DID

### 验证持有者

通过用户的钱包地址，以及钱包地址对应的私钥对钱包地址采用Secp256k1算法进行签名，验证该账户是否持有HashKey DID。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 参考资料：HashKey DID [addrClaimed](https://docs.hashkey.id/protocol/deployments)方法。

- Oracle Request Data：

|       名称    |     类型    |      必传      |         描述           |
| -------------|-------------|-------------|------------------------|
| `account `   | address     |       Y     |   钱包地址              |
| `signature`  | string      |       Y     |   使用钱包私钥对account参数的签名值   |


- Oracle Response Data：

|       名称     |     类型    |      必传  |         描述              |
| --------------|-------------|-----------|--------------------------|
| `result`      | bool        |       Y   |    验证的结果              |



### 查询DID

根据用户的HashKey DID标识符查询相关数据。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：DID Document Retrieval Service

- 参考资料：HashKey DID [Get DID info](https://docs.hashkey.id/developers/api-reference/openapi)方法。

- Oracle Request Data：

|     名称     |     类型  |      必传 |      描述            |
| ------------| ----------|----------| --------------------|
| `did`       | string    |       Y  |    DID标识符         |


- Oracle Response Data：

|    名称    |       类型   |      必传      |    描述       |
|-----------|--------------|---------------|--------------|
| `result`  | string       |       Y       |    关联数据    |




## Farcaster ID

### 验证持有者

来验证您的应用创建的签名密钥请求。
通过 fid、公钥、以及使用私钥进行EIP-712签名的值，验证该账户是否持有Farcaster ID。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 参考资料：Farcaster ID [validate](https://docs.farcaster.xyz/reference/contracts/reference/signed-key-request-validator)方法。

- Oracle Request Data：

|           名称                |       类型      |      必传      |            描述             |
| ------------------| ----------|----------| ----------------------|
| `fid`                    | uint256           |       Y         |    与公钥关联的主fid          |
| `key`                     | bytes            |       Y         |   要验证的公钥               |
| `sig`                     | bytes            |       Y         |   EIP-712实体的签名值              |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |



### 注册ID

还未拥有 fid 的用户可以注册一个新的 FID。

note：将新的 fid 注册到特定地址并支付存储费用。接收地址必须签署 EIP-712 注册消息以批准注册。接收者必须尚未拥有 fid。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：identity Registration Service

- 参考资料：Farcaster ID [registerFor](https://docs.farcaster.xyz/reference/contracts/reference/id-gateway)方法。

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

### 验证World ID

通过World ID根以及证明数据，验证World ID是否为实际本人。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Account Holder Authentication Service 

- 参考资料：World ID [verifyProof](https://docs.world.org/world-id/reference/contracts)方法。

- Oracle Request Data：

|           名称     |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `root`                    | uint256           |       Y         |    要验证的World ID根       |
| `groupId`                     | uint256            |       Y         |   要验证的凭证类型。目前仅支持填 1      |
| `signalHash`                     | uint256            |       Y         |   要验证的信号的 keccak256 哈希值      |
| `nullifierHash`                    | uint256           |       Y         |    当前操作的匿名用户ID      |
| `externalNullifierHash`                     | uint256            |       Y         |   标识用户正在验证的应用程序和操作的 keccak256 哈希值       |
| `proof`                     | uint256[8]            |       Y         |   从 IDKit 获取的十六进制字符串证明要验证的零知识证明    |

- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `result`                    | bool           |       Y         |    验证的结果          |




## China RealDID

### 查验DID

使用China RealDID的公钥对用户`姓名`+`身份证号码`使用sm2p256v1算法进行加密，并使用sm3算法对`姓名`+`身份证号码`进行哈希，查验该用户是否申领过实名DID。请求时也可传入实名DID文档内的公钥索引，如果该用户申领过实名DID则同时返回该索引对应的公钥值。


- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：Real-name Authentication Service 

- 参考资料：[China RealDID](https://did.bsnbase.com) 技术接口文档`验证DID`接口。

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `authApplyRetainData`                    | string           |       Y         |    个人用户信息加密字段          |
| `publicKeyIndex`                     | Integer            |       N         |   实名DID文档中的公钥索引       |
| `authHash`    | string            |       Y        |    使用SM3算法对姓名+身份证号进行哈希计算后的哈希值 |


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

使用实名DID标识符查询对应的DID文档以及状态。

- JobID：9330d9fc54ab48ada8373493b0ef9cf3

- 业务分类：DID Document Retrieval Service

- 参考资料：[China RealDID](https://did.bsnbase.com) 技术接口文档`查询下载DID文档`接口。

- Oracle Request Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `did`                    | string           |       Y         |    DID标识符         |


- Oracle Response Data：

|           名称                |       类型      |      必传      |                     描述               |
| ------------------| ----------|----------| ----------------------|
| `didDocument`                    | string           |       Y         |    DID文档          |
| `status`                    | string           |       Y         |    0：正常，1：注销        |


## Legal

Your use of this Project is governed by the terms found at www.opendid.io (the "Terms").

## Community
Contact us: contact@opendid.io

Website: www.opendid.io
