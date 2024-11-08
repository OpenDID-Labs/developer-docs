
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://opendid.io)
[![license](https://img.shields.io/badge/license-MIT-red.svg)](https://mit-license.org/)
[![Docs](https://img.shields.io/badge/docs-%F0%9F%93%84-yellow)](https://github.com/OpenDID-Labs/developer-docs)


**Application system developers can call the OpenDID Oracle contract through their application contracts. In this way, they can access the OpenDID service in the form of a web3-based oracle.** So far, we have deployed the OpenDID Oracle contract on three public chains: Ethereum Mainnet, Polygon Mainnet and Aptos Mainnet. Since the data on each blockchain is independent, we recommend that you choose one of the chains to deploy application contracts based on business needs. Below are the addresses of the OpenDID Oracle contract on different chains.

* Ethereum: [0x9bf35821b61fbb66dc1d7557b7cb150ab568cdc6](https://etherscan.io/address/0x9bf35821b61fbb66dc1d7557b7cb150ab568cdc6)

* Polygon: [0xdde4cE6F8484ed882A463BBdF9d691DC8bb4E48E](https://polygonscan.com/address/0xdde4cE6F8484ed882A463BBdF9d691DC8bb4E48E)

* Aptos: [0x9aa6e4e0e6fe912d2a9d413bf4111810c7a4c15bf90e8938ccbcc21f516e93ff](https://explorer.aptoslabs.com/object/0x9aa6e4e0e6fe912d2a9d413bf4111810c7a4c15bf90e8938ccbcc21f516e93ff?network=mainnet)


:building_construction: **In the OpenDID Oracle contract, the parameter defined for application contracts to receive data is `data`, which is of type `string`.** To learn more about the data structure, please refer to [OpenDID Oracle Job's Data Field Instructions](https://github.com/OpenDID-Labs/developer-docs/blob/main/OpenDID%20Oracle%20Job's%20Data%20Field%20Instructions.md). This document contains data structures for request and response parameters for various ID System jobs.

> [!IMPORTANT]
> After the data verification, the OpenDID service will send the result back to your application contract. This action involves gas consumption. Therefore, in the OpenDID Oracle contract, we have defined a service fee. You can query the service fee by `jobId` before sending the request data. If you have not received the verification result within 15 minutes after sending the request, you can cancel the verification service, and we will refund the paid service fee immediately.


## Table of Contents

- [Ethereum and Polygon](#ethereum-and-polygon)
    - [Query Fees](#query-fees)
    - [Send Verification Data](#send-verification-data)
    - [Receive Verification Data](#receive-verification-data)
    - [Cancel Verification Data](#cancel-verification-data)
- [Aptos](#aptos)
    - [Register UA](#register-ua)
    - [Query Fees](#query-fees)
    - [Send Verification Data](#send-verification-data)
    - [Receive Verification Results](#receive-verification-results)
    - [Cancel Verification Data](#cancel-verification-data)     
- [Legal](#legal)
- [Community](#community)


## Ethereum and Polygon

### Query Fees
Your application contract can call the `quote` function of the OpenDID Oracle contract to obtain the service fee that needs to be paid. You will pay this service fee when sending the verification data.

```solidity
pragma solidity ^0.8.0;

function quote(bytes32 jobId) external view returns (uint256);
```

> [!WARNING]
> You can get the parameter of `jobId` in [OpenDID Oracle Job's Data Field Instructions](https://github.com/OpenDID-Labs/developer-docs/blob/main/OpenDID%20Oracle%20Job's%20Data%20Field%20Instructions.md). The service fees corresponding to different jobIds are also different.


### Send Verification Data

Your application contract can call the `oracleRequest` function of the OpenDID Oracle contract to send the data to be verified. After receiving your request, the OpenDID Oracle contract will perform the verification process and generate a unique `requestId`.

```solidity
pragma solidity ^0.8.0;

function oracleRequest(
        bytes32 jobId,
        address callbackAddress,
        uint256 nonce,
        string memory data
    ) external payable returns (bool);
```

### Receive Verification Data

You need to incorporate the `oracleResponse` function defined by the OpenDD Oracle contract into your application contract. This way, the OpenDID Oracle contract can send back the verification results corresponding to `requestId` to your application contract.

```solidity
pragma solidity ^0.8.0;

function oracleResponse(bytes32 requestId, string memory data) external;
```

### Cancel Verification Data

Your application contract can cancel the data verification by calling the `cancelOracleRequest` function of the OpenDID Oracle contract. After cancellation, we will immediately refund the paid service fees to `refundAddress`.

```solidity
pragma solidity ^0.8.0;

function cancelOracleRequest(
        bytes32 requestId,
        address refundAddress
    ) external returns (bool);
```    


:mage: **Don't know how to develop an application contract?** Please refer to the following example:

```solidity
pragma solidity ^0.8.0;

import "../interfaces/DIDOracleRequestInterface.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract ATestConsumer is Ownable {
    DIDOracleRequestInterface public oracle;
    uint256 public requestCount = 1;
    mapping(bytes32 => string) public results;

    event OracleRequested(bytes32 indexed requestId);
    event RequestOracleDataFulfilled(bytes32 indexed requestId);

    constructor(address didOracleAddress) Ownable(msg.sender) {
        oracle = DIDOracleRequestInterface(didOracleAddress);
    }

    // @notice gets the Job fee.
    // @param jobId The Job Specification ID
    // @return gasAmount The amount of gas used for job messaging fee
    function quote(string memory jobId) public view returns (uint256) {
        return oracle.quote(stringToBytes32(jobId));
    }

    // @notice Creates the oracle request with the specified job
    // @param jobId The Job Specification ID
    // @param data The request parameters associated with jobid
    function requestOracleData(
        string memory jobId,
        string memory data
    ) public payable {
        uint256 nonce = requestCount;
        requestCount = nonce + 1;
        emit OracleRequested(buildRequestId(nonce));
        bool flag = oracle.oracleRequest{value: msg.value}(
            stringToBytes32(jobId),
            address(this),
            nonce,
            data
        );
        require(flag, "failed to call oracle");
    }

    // @notice Cancels the oracle request.
    // @param requestId The fulfillment request ID
    // @param refundAddress The address to receive the refund
    // @return Status if the call was successful
    function cancelOracleRequest(
        bytes32 requestId,
        address refundAddress
    ) public returns (bool) {
        bool flag = oracle.cancelOracleRequest(requestId, refundAddress);
        require(flag, "failed to call oracle");
        return flag;
    }

    // @notice Called by the oracle contract to fulfill requests.
    // @param requestId The fulfillment request ID
    // @param data The response data
    function oracleResponse(
        bytes32 requestId,
        string calldata data
    ) public onlyFromOracle {
        results[requestId] = data;
        emit RequestOracleDataFulfilled(requestId);
    }

    // @notice Generates a jobid for the specified nonce.
    function buildRequestId(uint256 nonce) public view returns (bytes32) {
        return keccak256(abi.encodePacked(this, nonce));
    }

    // @notice Converts a string to type bytes32.
    function stringToBytes32(
        string memory source
    ) private pure returns (bytes32 result) {
        bytes memory tempEmptyString = bytes(source);
        if (tempEmptyString.length == 0) {
            return 0x0;
        }
        assembly {
            result := mload(add(source, 32))
        }
    }

    modifier onlyFromOracle() {
        require(
            address(oracle) == msg.sender,
            "Only can call via the oracle contract"
        );
        _;
    }
}
```  

## Aptos

### Register UA
UA refers to any `struct` within an application contract. However, each application contract can only register one UA. You can register a UA into the application contract through the following method. The application contract will save the returned `UaCapability<UA>` and utilize it in subsequent calls.

```move
public fun register_ua<UA>(account: &signer): UaCapability<UA>;
```    

> [!WARNING]
> You must register the UA in the initialization function of your application contract. This way, we can verify that you are using your own contract's UA.


### Query Fees
Your application contract can call the `quote` function of the OpenDID Oracle contract to obtain the service fee that needs to be paid. You will pay this service fee when sending the verification data.

```move
public fun quote(job_id: vector<u8>): u64

```

> [!WARNING]
> You can get the parameter of `job_id` in [OpenDID Oracle Job's Data Field Instructions](https://github.com/OpenDID-Labs/developer-docs/blob/main/OpenDID%20Oracle%20Job's%20Data%20Field%20Instructions.md). The service fees corresponding to different job_ids are also different.


### Send Verification Data

Your application contract can call the `oracle_request<UA>` function of the OpenDID Oracle contract to send the data to be verified.

```move
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

> [!WARNING]
> You need to to fill in the application contract address `callback_address` and module name `callback_madule` for receiving verification results, and specify the amount of `fee` not less than the amount of `queried service fee` * `GasPrice`.


### Receive Verification Results

Your application contract needs to implement a function for receiving the response of OpenDID Oracle verification results.

```move
public entry fun set_oracle_response(request_id: vector<u8>)

```

After receiving the verification results, your application contract needs to call the `receive_response<UA>` function of the OpenDID Oracle contract for query.

```move
public fun receive_response<UA>(request_id: vector<u8>, _cap: &UaCapability<UA>): String 

```

### Cancel Verification Data

Your application contract can cancel the data verification by calling the `cancel_Oracle_Request` function of the OpenDID Oracle contract with `request_id`. If successful, the AptosCoin you have paid for this request will be refunded to your application contract. The calling method is as follows:

```move
public fun cancel_oracle_request<UA>(request_id: vector<u8>,  _cap: &UaCapability<UA>):coin::Coin<AptosCoin>

```


:mage: **Don't know how to develop an application contract?** Please refer to the following example:

```move
module app_example::app {
    use std::option;
    use std::option::Option;
    use std::signer;
    use did_oracle::oracle::{Self,UaCapability};
    use std::string::{Self, String};
    use aptos_std::simple_map;
    use aptos_framework::aptos_coin::AptosCoin;
    use aptos_framework::coin;
    use aptos_framework::transaction_context;

    struct OracleUA {}

    #[resource_group_member(group = aptos_framework::object::ObjectGroup)]
    struct AppMetaData has key {
        nonce: u64,
        request_ids: simple_map::SimpleMap<u64, vector<u8>>,
        oracle_call: simple_map::SimpleMap<vector<u8>, OracleCallData>,
        cap:UaCapability<OracleUA>,
    }

    struct OracleCallData has store {
        call_back: bool,
        call_fee:u64,
        request_data: String,
        response_data: option::Option<String>
    }

    fun init_module(signer: &signer) {
        let cap = oracle::register_ua<OracleUA>(signer);

        move_to(
            signer,
            AppMetaData {
                nonce: 0,
                request_ids: simple_map::create(),
                oracle_call: simple_map::create(),
                cap,
            }
        );
    }

    #[view]
    public fun get_oracle_call(nonce: u64): (vector<u8>, bool, String, Option<String>) acquires AppMetaData {
        let metadata = borrow_global_mut<AppMetaData>(@app_example);

        assert!(simple_map::contains_key(&metadata.request_ids, &nonce), 404);
        let request_id = simple_map::borrow(&metadata.request_ids, &nonce);

        let oracle_call = simple_map::borrow(&metadata.oracle_call, request_id);

        (*request_id, oracle_call.call_back, oracle_call.request_data, oracle_call.response_data)

    }

    public entry fun call_oracle(account:&signer,job_id:vector<u8>,data: String) acquires AppMetaData {
        let module_id = string::utf8(b"app");

        let job_fee_amount = oracle::quote(job_id);

        job_fee_amount = job_fee_amount + (job_fee_amount/10)*2;

        job_fee_amount = job_fee_amount*transaction_context::gas_unit_price();

        let job_fee = coin::withdraw<AptosCoin>(account,job_fee_amount);


        let metadata = borrow_global_mut<AppMetaData>(@app_example);

        let request_id = oracle::oracle_request<OracleUA>(job_id, @app_example, module_id, metadata.nonce, data,job_fee,&metadata.cap);

        simple_map::add(&mut metadata.request_ids, metadata.nonce, request_id);

        simple_map::add(
            &mut metadata.oracle_call,
            request_id,
            OracleCallData { call_back: false, call_fee:job_fee_amount,request_data: data, response_data: option::none() }
        );

        metadata.nonce = metadata.nonce + 1;
    }

    public entry fun oracle_response(request_id: vector<u8>) acquires AppMetaData {
        let metadata = borrow_global_mut<AppMetaData>(@app_example);

        let data = oracle::receive_response<OracleUA>(request_id,&metadata.cap);

        if (simple_map::contains_key(&metadata.oracle_call, &request_id)) {
            let call_data = simple_map::borrow_mut(&mut metadata.oracle_call, &request_id);
            call_data.call_back = true;
            call_data.response_data = option::some(data);
        }
    }

    public entry fun cancel_oracle_request(account:&signer,request_id: vector<u8>) acquires AppMetaData {
        let metadata = borrow_global<AppMetaData>(@app_example);
        let refund_fee = oracle::cancel_oracle_request<OracleUA>(request_id,&metadata.cap);
        coin::deposit(signer::address_of(account),refund_fee);
    }

    #[test_only]
    public fun init_for_test(signer: &signer) {
        init_module(signer);
    }
}
```

## Legal

Your use of this Project is governed by the terms found at www.opendid.io (the "Terms").

## Community
Contact us: contact@opendid.io

Website: www.opendid.io
