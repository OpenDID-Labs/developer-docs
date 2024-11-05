# <img src="logo.svg" alt="OpenDID" height="40px">

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://webasedoc.readthedocs.io/zh_CN/latest/docs/WeBASE/CONTRIBUTING.html)
[![Code Lines](https://tokei.rs/b1/github/WeBankBlockchain/WeBASE?category=code)](https://github.com/OpenDID-Labs/contracts)
[![GitHub (pre-)release](https://img.shields.io/github/release/WeBankBlockchain/WeBASE/all.svg)](https://github.com/WeBankBlockchain/WeBASE/releases)
[![license](http://img.shields.io/badge/license-Apache%20v2-red.svg)](http://www.apache.org/licenses/)
[![Docs](https://img.shields.io/badge/docs-%F0%9F%93%84-yellow)](https://docs.openzeppelin.com/contracts)

**用于应用合约如何成功调用OpenDID Oracle合约。** OpenDID Oracle合约分别在Ethereum Mainnet、Polygon Mainnet、Aptos Mainnet 上部署。

 * Ethereum（合约地址）：[0x待补充](https://github.com/OpenDID-Labs)
 * Polygon（合约地址）： [0x待补充](https://github.com/OpenDID-Labs)
 * Aptos（合约地址）：[0x待补充](https://github.com/OpenDID-Labs) 


:building_construction: **想要了解如何序列化OpenDID Oracle合约的data参数?** 请查看[JobID参数定义](https://github.com/OpenDID-Labs) — 包含众多ID System不同JobID请求参数的格式化说明。

> [!IMPORTANT]
> OpenDID Oracle定义的费用，使用Oracle合约所在链的加密货币进行支付。Ethereum和Polygon上的OpenDID Oracle合约，支持应用合约主动取消某笔还未得到验证结果的请求，取消后OpenDID Oracle会即时返还收取到的所有费用。Aptos上的OpenDID Oracle合约，暂不支持此操作。

## 调用OpenDID Oracle合约

### Ethereum和Polygon

#### 查询费用
应用合约调用 OpenDID Oracle 合约的 quote 方法获取需要支付的费用。

```solidity
pragma solidity ^0.8.0;

function quote(bytes32 jobId) external view returns (uint256);
```

> [!WARNING]
> jobId 可通过[JobID参数定义](https://github.com/OpenDID-Labs)查看，不同的jobID对应的费用也不相同。


#### 发送验证数据

应用合约调用 OpenDID Oracle 合约的 oracleRequest 方法发送待验证的数据。

```solidity
pragma solidity ^0.8.0;

function oracleRequest(
        bytes32 jobId,
        address callbackAddress,
        uint256 nonce,
        string memory data
    ) external payable returns (bool);
```

#### 接收验证结果

应用合约里面需实现 OpenDID Oracle 合约接口定义的 oracleResponse 方法，以便 OpenDID Oracle 合约可以将验证结果回送给应用合约。

```solidity
pragma solidity ^0.8.0;

function oracleResponse(bytes32 requestId, string memory data) external;
```

#### 取消验证请求

_如果长时间未收到验证结果，应用合约可以通过调用 OpenDID Oracle 合约的 cancelOracleRequest 方法取消验证。_

```solidity
pragma solidity ^0.8.0;

function cancelOracleRequest(
        bytes32 requestId,
        address refundAddress
    ) external returns (bool);
```    


:mage: **不确定应用合约如何调用OpenDID Oracle合约?** 请查看 [应用合约示例](https://github.com/OpenDID-Labs) — 一个可以直接使用的应用合约。

### Aptos

#### 注册 UA
应用合约内可以通过以下方法注册 UA，并且保存返回的 UaCapability<UA>， 这将在后续的调用中使用。

```
public fun register_ua<UA>(account: &signer): UaCapability<UA>;
```    

> [!WARNING]
> UA：应用合约内的任意 struct，每个应用合约只能注册一个 UA。

> [!WARNING]
> 必须在初始化方法中注册，这样才能验证你使用了自己合约的 UA。


#### 查询费用
应用合约调用 OpenDID Oracle 合约的 get_messaging_fees 方法获取需要支付的费用。

```
public fun get_messaging_fees(job_id: vector<u8>): u64

```

> [!WARNING]
> job_id可通过 [JobID参数定义](https://github.com/OpenDID-Labs) 查看，不同的job_id对应的费用也不相同。


#### 发送验证数据

应用合约调用 OpenDID Oracle 合约的 oracle_request<UA> 方法发送待验证的数据。

> [!WARNING]
> 必须填写接收验证结果的应用合约地址 callback_address 和模块名称 callback_module，并且指定不小于“查询费用”的 fee。

```
public fun oracle_request<UA>(
    job_id: vector<u8>,
    callback_address: address,
    callback_module: String,
    nonce: u64,
    data: String,
    fee: coin::Coin<AptosCoin>,
    _cap: &UaCapability<UA>
): (vector<u8>)

```

#### 接收验证结果

应用合约里面需实现接收 OpenDID Oracle 回送验证结果的方法。

```
public entry fun set_oracle_response(request_id: vector<u8>, data: String)

```

应用合约收到验证结果后，必须调用 OpenDID Oracle 合约的 receive_response<UA> 方法进行验证。

```
public fun receive_response<UA>(request_id: vector<u8>, data: String, _cap: &UaCapability<UA>)

```

:mage: **不确定应用合约如何调用OpenDID Oracle合约?** 请查看 [应用合约示例](https://github.com/OpenDID-Labs) — 一个可以直接使用的应用合约。


## Legal

Your use of this Project is governed by the terms found at www.opendid.io (the "Terms").

## Community
Contact us: contact@opendid.io

Website: www.opendid.io
