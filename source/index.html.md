---
title: API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

# includes:
#   - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Nucal API
---

# Introduction

Welcome to the Nucal API! You can use our API to use Ethereum rpc's.


# Authentication

> To authorize, use this code:


> Make sure to replace `<your_wallet_address>` with your wallet address.

```shell
curl -request GET "https://api.web3.nucal.com/auth/connect/wallet/<your_wallet_address>/nonce/"
```

Response will be like following:

```shell
{
    "nonce": 6156349486544681,
    "text": "Please sign this message, to connect. (6156349486544681)"
}
```

Sign the text in response with your wallet.


> Make sure to replace `<your_wallet_address>` with your wallet address.
> Make sure to replace `<signature>` with the one you created with your wallet.

```shell
curl --request POST 'https://api.web3.nucal.com/auth/connect/wallet/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "walletAddress": "<your_wallet_address>",
  "signature": "<signature>"
}'
```

Response:

```shell
{
  "accessToken": "<your_access_token>",
  "accessTokenExpiresAt": <access_token_expiration_time>,
  "refreshToken": "<your_refresh_token>",
  "refreshTokenExpiresAt": <refresh_token_expiration_time>
}
```

Nucal uses token to allow access to the API. 

Nucal expects for the token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer <token>`

<aside class="notice">
You must replace <code>token</code> with your access token.
</aside>


# Ethereum

## eth_accounts

Returns a list of addresses owned by client.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": []
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`Array of DATA - addresses owned by the client`

## eth_blockNumber

<!-- Returns the current "latest" block number. -->

Returns the number of most recent block.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "10307C9"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter | Description                                                           |
| --------- | --------------------------------------------------------------------- |
| DATA      | address to check for balance                                          |
| TAG       | integer block number, or the string "latest", "earliest" or "pending" |

### Returns

`QUANTITY - integer of the current block number the client is on.`

## eth_getBalance

Returns the balance of the account of given address.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getBalance",
          "params":[
            "0x9d9EAC372D333E1c391a96DEc802ed4faa946D91",
            "latest"
          ],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x0234c8a3397aab58"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`QUANTITY - integer of the current balance, measured in wei.`

## eth_call

Executes a new message call immediately without creating a transaction on the block chain.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_call",
          "params":[
              {
                "from": null,
                "to": "0x9d9EAC372D333E1c391a96DEc802ed4faa946D91",
                "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
              },
              "latest"
          ]
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

1. `Object` - The transaction call object.

| Parameter | Type     | Description                                                           |
| --------- | -------- | --------------------------------------------------------------------- |
| from      | DATA     | (optional) The address the transaction is sent from.                  |
| to        | DATA     | The address the transaction is directed to.                           |
| gas       | QUANTITY | (optional) The integer of gas provided for the transaction execution. |
| gasPrice  | QUANTITY | Integer of the gasPrice used for each paid gas(optional)              |
| value     | QUANTITY | (optional)                                                            |
| data      | DATA     | Hash of the method signature and encoded parameters                   |

2. `QUANTITY` - integer block number, or the string "latest", "earliest", "pending" or "finalized".

<aside class="notice">
eth_call consumes `zero gas`, but this parameter may be needed by some executions.
</aside>

### Returns

`DATA - the return value of executed contract method.`

## eth_protocolVersion

Returns the current Ethereum protocol version.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":1}'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 35,
  "result": "54"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`String - The current Ethereum protocol version`

## eth_chainId

Returns the currently configured chain id, a value used in replay-protected transaction signing as introduced by EIP-155.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":1}'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x1"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`QUANTITY - hexadecimal of the current chain id.`

## eth_estimateGas

Generates and returns an estimate of how much gas is necessary to allow the transaction to complete. The transaction will not be added to the blockchain. Note that the estimate may be significantly more than the amount of gas actually used by the transaction, for a variety of reasons including EVM mechanics and node performance.

<aside class="notice">
Method Limitations -- 
To prevent abuse of the API, the gas parameter to eth_estimateGas and eth_call are capped at 10x (1000%) the current block gas limit. You can recreate this behavior in your local test environment (ganache, besu, geth, or other client) via the rpc.gascap command-line option.
</aside>

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_estimateGas",
          "params":
              [{
                "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155,
                "to": "0x9d9EAC372D333E1c391a96DEc802ed4faa946D91",
                "gas": "0x76c0",
                "gasPrice": "0x9184e72a000",
                "value": "0x9184e72a"
                "data": "0x70a082310000000000000000000000006E0d01A76C3Cf4288372a29124A26D4353EE51BE"
              }]
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x5cec"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`Object` - The transaction call object.

| Parameter            | Type     | Description                                                                                           |
| -------------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| from                 | DATA     | (optional) The address the transaction is sent from.                                                  |
| to                   | DATA     | (optional) The address the transaction is directed to.                                                |
| gas                  | QUANTITY | (optional) The integer of gas provided for the transaction execution.                                 |
| gasPrice             | QUANTITY | (optional) Integer of the gasPrice used for each paid gas(optional)                                   |
| maxPriorityFeePerGas | QUANTITY | (optional) Maximum fee, in Wei, the sender is willing to pay per gas above the base fee.              |
| maxFeePerGas         | QUANTITY | (optional) Maximum total fee (base fee + priority fee), in Wei, the sender is willing to pay per gas. |
| value                | QUANTITY | (optional) hexadecimal value of the value sent with this transaction.                                 |
| data                 | DATA     | (optional) Hash of the method signature and encoded parameters.                                       |

<aside class="notice">
eth_estimateGas consumes `zero gas`, but this parameter may be needed by some executions.
</aside>

### Returns

`GAS USED - the amount of gas used.`

## eth_feeHistory

Returns historical gas information, allowing you to track trends over time.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_feeHistory",
          "params":[4, "latest", [25, 75]],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "result": {
    "baseFeePerGas": [
      "0x3da8e7618",
      "0x3e1ba3b1b",
      "0x3dfd72b90",
      "0x3d64eee76",
      "0x3d4da2da0",
      "0x3ccbcac6b"
    ],
    "gasUsedRatio": [
      0.5290747666666666, 0.49240453333333334, 0.4615576, 0.49407083333333335,
      0.4669053
    ],
    "oldestBlock": "0xfab8ac",
    "reward": [
      ["0x59682f00", "0x59682f00"],
      ["0x59682f00", "0x59682f00"],
      ["0x3b9aca00", "0x59682f00"],
      ["0x510b0870", "0x59682f00"],
      ["0x3b9aca00", "0x59682f00"]
    ]
  },
  "id": 0
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`Object` - The transaction call object.

| Parameter         | Type              | Description                                                                                                                                                           |
| ----------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| blockCount        | integer           | Number of blocks in the requested range. Between 1 and 1024 blocks can be requested in a single query.                                                                |
| newestBlock       | string            | (optional) Integer representing the highest number block of the requested range, or one of the string tags latest, earliest, or pending.                              |
| rewardPercentiles | array of integers | (optional) A monotonically increasing list of percentile values to sample from each block's effective priority fees per gas in ascending order, weighted by gas used. |

<aside class="notice">
blockCount -> If blocks in the specified block range are not available, then only the fee history for available blocks is returned.      
</aside>

### Returns

`oldestBlock - Lowest number block of the returned range expressed as a hexidecimal number.`

`baseFeePerGas - An array of block base fees per gas. This includes the next block after the newest of the returned range, because this value can be derived from the newest block. Zeroes are returned for pre-EIP-1559 blocks`

`gasUsedRatio - An array of block gas used ratios. These are calculated as the ratio of gasUsed and gasLimit.`

`reward - An array of effective priority fee per gas data points from a single block. All zeroes are returned if the block is empty.`

## eth_gasPrice

Returns the current gas price in wei.(the current price per gas in wei)

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":1}'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x1dfd13ffe"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`QUANTITY - integer of the current gas price in wei.`

## eth_getBlockByHash

Returns information about a block by hash.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getBlockByHash",
          "params": ["0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",false],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "difficulty": "0xbfabcdbd93dda",
    "extraData": "0x737061726b706f6f6c2d636e2d6e6f64652d3132",
    "gasLimit": "0x79f39e",
    "gasUsed": "0x79ccd3",
    "hash": "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "logsBloom": "0x4848112002a2020aaa08121...",
    "miner": "0x5a0b54d5dc17e0aadc383d2db43b0a0d3e029c4c",
    "mixHash": "0x3d1fdd16f15aeab72e7db1013b9f034ee33641d92f71c0736beab4e67d34c7a7",
    "nonce": "0x4db7a1c01d8a8072",
    "number": "0x5bad55",
    "parentHash": "0x61a8ad530a8a43e3583f8ec163f773ad370329b2375d66433eb82f005e1d6202",
    "receiptsRoot": "0x5eced534b3d84d3d732ddbc714f5fd51d98a941b28182b6efe6df3a0fe90004b",
    "sha3Uncles": "0x8a562e7634774d3e3a36698ac4915e37fc84a2cd0044cb84fa5d80263d2af4f6",
    "size": "0x41c7",
    "stateRoot": "0xf5208fffa2ba5a3f3a2f64ebd5ca3d098978bedd75f335f56b705d8715ee2305",
    "timestamp": "0x5b541449",
    "totalDifficulty": "0x12ac11391a2f3872fcd",
    "transactions": [
      "0x8784d99762bccd03b2086eabccee0d77f14d05463281e121a62abfebcf0d2d5f",
      ...
    ],
    "transactionsRoot": "0xf98631e290e88f58a46b7032f025969039aa9b5696498efc76baf436fa69b262",
    "uncles": [
      "0x824cce7c7c2ec6874b9fa9a9a898eb5f27cbaf3991dfa81084c3af60d1db618c"
    ]
  }
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`Object` - The transaction call object.

| Parameter                 | Type    | Description                                                                                               |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------------------- |
| BlockHash                 | string  | [required] a string representing the hash (32 bytes) of a block                                           |
| ShowTransactionDetailFlag | boolean | [required] If true it returns the full transaction objects, if false only the hashes of the transactions. |

### Returns

### BLOCK - A block object, or null when no block was found

`number: QUANTITY - the block number. null when its pending block.`

`hash: DATA, 32 Bytes - hash of the block. null when its pending block.`

`parentHash: DATA, 32 Bytes - hash of the parent block.`

`nonce: DATA, 8 Bytes - hash of the generated proof-of-work. null when its pending block.`

`sha3Uncles: DATA, 32 Bytes - SHA3 of the uncles data in the block.`

`logsBloom: DATA, 256 Bytes - the bloom filter for the logs of the block. null when its pending block.`

`transactionsRoot: DATA, 32 Bytes - the root of the transaction trie of the block.`

`stateRoot: DATA, 32 Bytes - the root of the final state trie of the block.`

`receiptsRoot: DATA, 32 Bytes - the root of the receipts trie of the block.`

`miner: DATA, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.`

`difficulty: QUANTITY - integer of the difficulty for this block.`

`totalDifficulty: QUANTITY - integer of the total difficulty of the chain until this block.`

`extraData: DATA - the "extra data" field of this block.`

`size: QUANTITY - integer the size of this block in bytes.`

`gasLimit: QUANTITY - the maximum gas allowed in this block.`

`gasUsed: QUANTITY - the total used gas by all transactions in this block.`

`timestamp: QUANTITY - the unix timestamp for when the block was collated.`

`transactions: Array - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.`

`uncles: Array - Array of uncle hashes.`

## eth_getBlockByNumber

Returns information about a block by block number.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getBlockByNumber",
          "params": ["0x1b4", true],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "difficulty": "0x4ea3f27bc",
    "extraData": "0x476574682f4c5649562f76312e302e302f6c696e75782f676f312e342e32",
    "gasLimit": "0x1388",
    "gasUsed": "0x0",
    "hash": "0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae",
    "logsBloom": "0x00000000000000000...",
    "miner": "0xbb7b8287f3f0a933474a79eae42cbca977791171",
    "mixHash": "0x4fffe9ae21f1c9e15207b1f472d5bbdd68c9595d461666602f2be20daf5e7843",
    "nonce": "0x689056015818adbe",
    "number": "0x1b4",
    "parentHash": "0xe99e022112df268087ea7eafaf4790497fd21dbeeb6bd7a1721df161a6657a54",
    "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "size": "0x220",
    "stateRoot": "0xddc8b0234c2e0cad087c8b389aa7ef01f7d79b2570bccb77ce48648aa61c904d",
    "timestamp": "0x55ba467c",
    "totalDifficulty": "0x78ed983323d",
    "transactions": [],
    "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "uncles": []
  }
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`Object` - The transaction call object.

| Parameter       | Description                                                                                    |
| --------------- | ---------------------------------------------------------------------------------------------- |
| BLOCK PARAMETER | hexadecimal block number, or the string "latest", "earliest" or "pending"                      |
| Boolean         | If true it returns the full transaction objects, if false only the hashes of the transactions. |

### Returns

### BLOCK - A block object, or null when no block was found

`number: QUANTITY - the block number. null when its pending block.`

`hash: DATA, 32 Bytes - hash of the block. null when its pending block.`

`parentHash: DATA, 32 Bytes - hash of the parent block.`

`nonce: DATA, 8 Bytes - hash of the generated proof-of-work. null when its pending block.`

`sha3Uncles: DATA, 32 Bytes - SHA3 of the uncles data in the block.`

`logsBloom: DATA, 256 Bytes - the bloom filter for the logs of the block. null when its pending block.`

`transactionsRoot: DATA, 32 Bytes - the root of the transaction trie of the block.`

`stateRoot: DATA, 32 Bytes - the root of the final state trie of the block.`

`receiptsRoot: DATA, 32 Bytes - the root of the receipts trie of the block.`

`miner: DATA, 20 Bytes - the address of the beneficiary to whom the mining rewards were given.`

`difficulty: QUANTITY - integer of the difficulty for this block.`

`totalDifficulty: QUANTITY - integer of the total difficulty of the chain until this block.`

`extraData: DATA - the "extra data" field of this block.`

`size: QUANTITY - integer the size of this block in bytes.`

`gasLimit: QUANTITY - the maximum gas allowed in this block.`

`gasUsed: QUANTITY - the total used gas by all transactions in this block.`

`timestamp: QUANTITY - the unix timestamp for when the block was collated.`

`transactions: Array - Array of transaction objects, or 32 Bytes transaction hashes depending on the last given parameter.`

`uncles: Array - Array of uncle hashes.`

## eth_getBlockTransactionCountByHash

Returns the number of transactions in a block from a block matching the given block hash.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getBlockTransactionCountByHash",
          "params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xb" // 11
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter         | Description                           |
| ----------------- | ------------------------------------- |
| BLOCK HASH - DATA | [required] hash (32 Bytes) of a block |

### Returns

`QUANTITY - integer of the number of transactions in this block.`

## eth_getBlockTransactionCountByNumber

Returns the number of transactions in a block matching the given block number.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getBlockTransactionCountByHash",
          "params":["0xe8"],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0xa" // 10
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter       | Description                                                               |
| --------------- | ------------------------------------------------------------------------- |
| BLOCK PARAMETER | hexadecimal block number, or the string "latest", "earliest" or "pending" |

### Returns

`QUANTITY - integer of the number of transactions in this block.`

## eth_getCode

Returns the compiled smart contract code, if any, at a given address.

```shell

curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getCode",
          "params":["0x06012c8cf97bead5deae237070f9587f8e7a266d", "0x65a8db"],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x6001600080..."
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter       | Description                                                                          |
| --------------- | ------------------------------------------------------------------------------------ |
| ADDRESS         | [required] a string representing the address (20 bytes) of the code                  |
| BLOCK PARAMETER | [required] hexadecimal block number, or the string "latest", "earliest" or "pending" |

### Returns

`CODE - a hex of the code at the given address`

## eth_getLogs

Returns an array of all logs matching a given filter object.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getLogs",
          "params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}]
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`Object` - The filter options.

| Parameter | Type           | Description                                                                                                                                                                                                                                                                                                                                                        |
| --------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| fromBlock | QUANTITY       | [optional] Integer block number, or "latest" for the last mined block or "pending", "earliest" for not yet mined transactions.                                                                                                                                                                                                                                     |
| toBlock   | QUANTITY       | [optional] Integer block number, or "latest" for the last mined block or "pending", "earliest" for not yet mined transactions.                                                                                                                                                                                                                                     |
| address   | DATA           | [optional] Contract address or a list of addresses from which logs should originate.                                                                                                                                                                                                                                                                               |
| topics    | Array of DATA, | [optional] Topics are order-dependent. Each topic can also be an array of DATA with "or" options.                                                                                                                                                                                                                                                                  |
| blockhash | QUANTITY       | [optional] With the addition of EIP-234, blockHash will be a new filter option which restricts the logs returned to the single block with the 32-byte hash blockHash. Using blockHash is equivalent to fromBlock = toBlock = the block number with hash blockHash. If blockHash is present in the filter criteria, then neither fromBlock nor toBlock are allowed. |

### Returns

LOG OBJECTS - An array of log objects, or an empty array if nothing has changed since last poll.

- logs are objects with following params:

  `REMOVED -  true when the log was removed, due to a chain reorganization. false if it's a valid log.`

  `LOG INDEX - hexadecimal of the log index position in the block. null when its pending log.`

  `TRANSACTION INDEX - hexadecimal of the transactions index position log was created from. null when its pending log.`

  `TRANSACTION HASH - 32 Bytes - hash of the transactions this log was created from. null when its pending log.`

  `BLOCK HASH - 32 Bytes - hash of the block where this log was in. null when its pending. null when its pending log.`

  `blockNumber: the block number where this log was in. null when its pending. null when its pending log.`

  `address: 20 Bytes - address from which this log originated.`

  `data: contains one or more 32 Bytes non-indexed arguments of the log.`

  `topics: Array of 0 to 4 32 Bytes of indexed log arguments. (In solidity: The first topic is the hash of the signature of the event (e.g. Deposit(address,bytes32,uint256)), except you declared the event with the anonymous specifier.)`

## eth_getProof

Returns the account and storage values of the specified account, including the Merkle proof.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getProof",
          "params":["0x7F0d15C7FAae65896648C8273B6d7E43f58Fa842", ["0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"], "latest"]
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "accountProof": [
      "0xf90211a...0701bc80",
      "0xf90211a...0d832380",
      "0xf90211a...5fb20c80",
      "0xf90211a...0675b80",
      "0xf90151a0...ca08080"
    ],
    "balance": "0x0",
    "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
    "nonce": "0x0",
    "storageHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "storageProof": [
      {
        "key": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "proof": ["0xf90211a...0701bc80", "0xf90211a...0d832380"],
        "value": "0x1"
      }
    ]
  }
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter       | Description                                                                          |
| --------------- | ------------------------------------------------------------------------------------ |
| ADDRESS         | a string representing the address (20 bytes) to check for balance                    |
| STORAGE KEYS    | Array of 32-Byte storage keys to proofed and include                                 |
| BLOCK PARAMETER | [required] hexadecimal block number, or the string "latest", "earliest" or "pending" |

### Returns

`ACCOUNT PROOF - array of rlp-serialized MerkleTree-Nodes, starting with the stateRoot-Node, following the path of the SHA3 (address) as key.`

`STORAGE PROOF - array of storage-entries as requested. Each entry is an object with these properties:`

- KEY -> the requested storage key.
- VALUE -> the storage value.
- PROOF -> array of rlp-serialized MerkleTree-Nodes, starting with the storageHash-Node, following the path of the SHA3 (key) as path..

`STORAGE HASH - SHA3 of the StorageRoot. All storage will deliver a Merkle proof starting with this rootHash.`

`NONCE - nonce of the account.`

`CODE HASH - 32-Byte hash of the code of the account.`

`BALANCE - hexadecimal of the current balance in wei.`

## eth_getTransactionByBlockHashAndIndex

Returns information about a transaction by block hash and transaction index position.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getTransactionByBlockHashAndIndex",
          "params": ["0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35","0x0"]
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "blockNumber": "0x5bad55",
    "from": "0xfbb1b73c4f0bda4f67dca266ce6ef42f520fbb98",
    "gas": "0x249f0",
    "gasPrice": "0x174876e800",
    "hash": "0x8784d99762bccd03b2086eabccee0d77f14d05463281e121a62abfebcf0d2d5f",
    "input": "0x6ea056a9000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000bd8d7fa6f8cc00",
    "nonce": "0x5e4724",
    "r": "0xd1556332df97e3bd911068651cfad6f975a30381f4ff3a55df7ab3512c78b9ec",
    "s": "0x66b51cbb10cd1b2a09aaff137d9f6d4255bf73cb7702b666ebd5af502ffa4410",
    "to": "0x4b9c25ca0224aef6a7522cabdbc3b2e125b7ca50",
    "transactionIndex": "0x0",
    "type": "0x0",
    "v": "0x25",
    "value": "0x0"
  }
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter                  | Description                                                            |
| -------------------------- | ---------------------------------------------------------------------- |
| BLOCK HASH                 | [required] a string representing the hash (32 bytes) of a block        |
| TRANSACTION INDEX POSITION | [required] a hex of the integer representing the position in the block |

### Returns

`Object - A transaction object, or null when no transaction was found:`

- blockHash: DATA, 32 Bytes - hash of the block where this transaction was in. null when its pending.

- blockNumber: QUANTITY - block number where this transaction was in. null when its pending.

- from: DATA, 20 Bytes - address of the sender.

- gas: QUANTITY - gas provided by the sender.

- gasPrice: QUANTITY - gas price provided by the sender in Wei.

- hash: DATA, 32 Bytes - hash of the transaction.

- input: DATA - the data send along with the transaction.

- nonce: QUANTITY - the number of transactions made by the sender prior to this one.

- to: DATA, 20 Bytes - address of the receiver. null when its a contract creation transaction.

- transactionIndex: QUANTITY - integer of the transactions index position in the block. null when its pending.

- value: QUANTITY - value transferred in Wei.

- v: QUANTITY - ECDSA recovery id

- r: QUANTITY - ECDSA signature r

- s: QUANTITY - ECDSA signature s

## eth_getTransactionByBlockNumberAndIndex

Returns information about a transaction by block number and transaction index position.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getTransactionByBlockNumberAndIndex",
          params: [ "0x29c", "0x0" ],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "blockNumber": "0x5daf3b", // 6139707
    "from": "0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
    "gas": "0xc350", // 50000
    "gasPrice": "0x4a817c800", // 20000000000
    "hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "input": "0x68656c6c6f21",
    "nonce": "0x15", // 21
    "to": "0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
    "transactionIndex": "0x41", // 65
    "value": "0xf3dbb76162000", // 4290000000000000
    "v": "0x25", // 37
    "r": "0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
    "s": "0x4ba69724e8f69de52f0125ad8b3c5c2cef33019bac3249e2c0a2192766d1721c"
  }
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter                  | Description                                                            |
| -------------------------- | ---------------------------------------------------------------------- |
| BLOCK HASH                 | [required] a string representing the hash (32 bytes) of a block        |
| TRANSACTION INDEX POSITION | [required] a hex of the integer representing the position in the block |

### Returns

**See** [eth_getTransactionByBlockHashAndIndex](#eth_getTransactionByBlockHashAndIndex)

## eth_getTransactionByHash

Returns the information about a transaction requested by transaction hash.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getTransactionByHash",
          "params": ["0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "blockHash":"0x1d59ff54b1eb26b013ce3cb5fc9dab3705b415a67127a003c3e61eb445bb8df2",
    "blockNumber":"0x5daf3b", // 6139707
    "from":"0xa7d9ddbe1f17865597fbd27ec712455208b6b76d",
    "gas":"0xc350", // 50000
    "gasPrice":"0x4a817c800", // 20000000000
    "hash":"0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    "input":"0x68656c6c6f21",
    "nonce":"0x15", // 21
    "to":"0xf02c1c8e6114b1dbe8937a39260b5b0a374432bb",
    "transactionIndex":"0x41", // 65
    "value":"0xf3dbb76162000", // 4290000000000000
    "v":"0x25", // 37
    "r":"0x1b5e176d927f8e9ab405058b2d2457392da3e20f328b16ddabcebc33eaac5fea",
    "s":"0x4ba69724e8f69de52f0125ad8b3c5c2cef33019bac3249e2c0a2192766d1721c"
  }
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter | Description           |
| --------- | --------------------- |
| DATA      | hash of a transaction |

### Returns

`Object - A transaction object, or null when no transaction was found:`

- blockHash: DATA, 32 Bytes - hash of the block where this transaction was in. null when its pending.

- blockNumber: QUANTITY - block number where this transaction was in. null when its pending.

- from: DATA, 20 Bytes - address of the sender.

- gas: QUANTITY - gas provided by the sender.

- gasPrice: QUANTITY - gas price provided by the sender in Wei.

- hash: DATA, 32 Bytes - hash of the transaction.

- input: DATA - the data send along with the transaction.

- nonce: QUANTITY - the number of transactions made by the sender prior to this one.

- to: DATA, 20 Bytes - address of the receiver. null when its a contract creation transaction.

- transactionIndex: QUANTITY - integer of the transactions index position in the block. null when its pending.

- value: QUANTITY - value transferred in Wei.

- v: QUANTITY - ECDSA recovery id

- r: QUANTITY - ECDSA signature r

- s: QUANTITY - ECDSA signature s

## eth_getTransactionCount

Returns the number of transactions sent from an address.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getTransactionCount",
          "params":["0xc94770007dda54cF92009BFF0dE90c06F603a09f","0x5bad55"]
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x1a"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter       | Description                                                                          |
| --------------- | ------------------------------------------------------------------------------------ |
| ADDRESS         | [required] a string representing the address (20 bytes) of the code                  |
| BLOCK PARAMETER | [required] hexadecimal block number, or the string "latest", "earliest" or "pending" |

### Returns

`TRANSACTION COUNT - a hex code of the integer representing the number of transactions sent from this address.`

## eth_getTransactionReceipt

Returns the receipt of a transaction by transaction hash.

<aside class="notice">
Note that the receipt is not available for pending transactions.
</aside>

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getTransactionReceipt",
          "params": ["0xbb3a336e3f823ec18197f1e13ee875700f08f03e2cab75f0d0b118dabb44cba0"],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
    "blockNumber": "0x5bad55",
    "contractAddress": null,
    "cumulativeGasUsed": "0xb90b0",
    "effectiveGasPrice": "0x746a528800",
    "from": "0x398137383b3d25c92898c656696e41950e47316b",
    "gasUsed": "0x1383f",
    "logs": [
      {
        "address": "0x06012c8cf97bead5deae237070f9587f8e7a266d",
        "blockHash": "0xb3b20624f8f0f86eb50dd04688409e5cea4bd02d700bf6e79e9384d47d6a5a35",
        "blockNumber": "0x5bad55",
        "data": "0x000000000000000...",
        "logIndex": "0x6",
        "removed": false,
        "topics": [
          "0x241ea03ca20251805084d27d4440371c34a0b85ff108f6bb5611248f73818b80"
        ],
        "transactionHash": "0xbb3a336e3f823ec18197f1e13ee875700f08f03e2cab75f0d0b118dabb44cba0",
        "transactionIndex": "0x11"
      }
    ],
    "logsBloom": "0x0000000000000...",
    "status": "0x1",
    "to": "0x06012c8cf97bead5deae237070f9587f8e7a266d",
    "transactionHash": "0xbb3a336e3f823ec18197f1e13ee875700f08f03e2cab75f0d0b118dabb44cba0",
    "transactionIndex": "0x11",
    "type": "0x0"
  }
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter        | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| TRANSACTION HASH | [required]- a string representing the hash (32 bytes) of a transaction |

### Returns

`TRANSACTION RECEIPT - A transaction receipt object, or null when no receipt was found:`

- transactionHash : DATA, 32 Bytes - hash of the transaction.

- transactionIndex: QUANTITY - integer of the transactions index position in the block.

- blockHash: DATA, 32 Bytes - hash of the block including this transaction.

- blockNumber: QUANTITY - block number including this transaction.

- contractAddress: DATA, 20 Bytes - the contract address created, if the transaction was a contract creation, otherwise null.

- cumulativeGasUsed: the total amount of gas used when this transaction was executed in the block.

- effectiveGasPrice: QUANTITY - The sum of the base fee and tip paid per unit of gas.

- from: DATA, 20 Bytes - address of the sender.

- to: DATA, 20 Bytes - address of the receiver. null when its a contract creation transaction.

- gasUsed: QUANTITY - The amount of gas used by this specific transaction alone.

- logs: Array - Array of log objects, which this transaction generated.

- logsBloom: 256 Bytes - Bloom filter for light clients to quickly retrieve related logs.

- type: DATA - integer of the transaction type, 0x00 for legacy transactions, 0x01 for access list types, 0x02 for dynamic fees. It also returns either :

- root : DATA 32 bytes of post-transaction stateroot (pre Byzantium)

- status: QUANTITY either 1 (success) or 0 (failure)

## eth_mining

Returns true if client is actively mining new blocks.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{"jsonrpc":"2.0","method":"eth_mining","params":[],"id":1}'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": false
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`IS MINING FLAG - a boolean indicating if the client is mining`

## eth_hashrate

Returns the number of hashes per second that the node is mining with. Only applicable when the node is mining.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{"jsonrpc":"2.0","method":"eth_hashrate","params":[],"id":1}'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x0"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`HASHRATE - a hex code of an integer representing the number of hashes per second.`

## eth_sendRawTransaction

Creates new message call transaction or a contract creation for signed transactions.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_sendRawTransaction",
          "params": ["0xf869018203e882520894f17f52151ebef6c7334fad080c5704d77216b732881bc16d674ec80000801ba02da1c48b670996dcb1f447ef9ef00b33033c48a4fe938f420bec3e56bfd24071a062e0aa78a81bf0290afbc3a9d8e9a068e6d74caa66c5e0fa8a46deaae96b0833"],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x0"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter        | Description                             |
| ---------------- | --------------------------------------- |
| TRANSACTION DATA | [required] The signed transaction data. |

### Returns

`TRANSACTION HASH - 32 Bytes - the transaction hash, or the zero hash if the transaction is not yet available`

<aside class="notice">
Use eth_getTransactionReceipt to get the contract address, after the transaction was mined, when you created a contract.
</aside>

## eth_sign

The sign method calculates an Ethereum specific signature with: sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message))).

By adding a prefix to the message makes the calculated signature recognizable as an Ethereum specific signature. This prevents misuse where a malicious dapp can sign arbitrary data (e.g. transaction) and use the signature to impersonate the victim.

<aside class="notice">
the address to sign with must be unlocked.
</aside>

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_sign",
          "params": ["0xf869018203e882520894f17f52151ebef6c7334fad080c5704d77216b732881bc16d674ec80000801ba02da1c48b670996dcb1f447ef9ef00b33033c48a4fe938f420bec3e56bfd24071a062e0aa78a81bf0290afbc3a9d8e9a068e6d74caa66c5e0fa8a46deaae96b0833"],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x0"
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

| Parameter | Description               |
| --------- | ------------------------- |
| DATA      | 20 Bytes - address        |
| DATA      | N Bytes - message to sign |

### Returns

`DATA: Signature`

## eth_sendTransaction

<aside class="notice">
The eth_sendTransaction JSON-RPC method is not supported because Infura doesn't store the user's private key required to sign the transaction. Use eth_sendRawTransaction instead.
</aside>

**See** [eth_sendRawTransaction](#eth_sendRawTransaction)

## eth_syncing

Returns an object with data about the sync status or false.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_syncing",
          "params":[],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": false
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`None`

### Returns

`SYNC STATUS - a boolean as false only when not syncing`

`SYNC BLOCKS i. startingBlock - a hexcode of the integer indicating the block at which the import started (will only be reset, after the sync reached his head) ii. currentBlock - a hexcode of the integer indicating the current block, same as eth_blockNumber iii. highestBlock - a hexcode of the integer indicating the highest block`

## eth_submitWork

Used for submitting a proof-of-work solution.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_syncing",
          "params": [
            "0x0000000000000001",
            "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
            "0xD1FE5700000000000000000000000000D1FE5700000000000000000000000000",
          ],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": true
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`WORK ARRAY`

| Parameter      | Description                      |
| -------------- | -------------------------------- |
| DATA, 8 Bytes  | The nonce found (64 bits)        |
| DATA, 32 Bytes | The header's pow-hash (256 bits) |
| DATA, 32 Bytes | The mix digest (256 bits)        |

### Returns

`Boolean - returns true if the provided solution is valid, otherwise false.`

## eth_getFilterChanges

Polling method for a filter, which returns an array of logs which occurred since last poll.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_syncing",
          "params": [
            "0x16", // 22
          ],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`WORK ARRAY`

| Parameter | Description    |
| --------- | -------------- |
| QUANTITY  | the filter id. |

### Returns

`Array - Array of log objects, or an empty array if nothing has changed since last poll.`

- For filters created with eth_newBlockFilter the return are block hashes (DATA, 32 Bytes), e.g. ["0x3454645634534..."].

- For filters created with eth_newPendingTransactionFilter the return are transaction hashes (DATA, 32 Bytes), e.g. ["0x6345343454645..."].

- For filters created with eth_newFilter logs are objects with following params:

  - removed: TAG - true when the log was removed, due to a chain reorganization. false if its a valid log.
  - logIndex: QUANTITY - integer of the log index position in the block. null when its pending log.
  - transactionIndex: QUANTITY - integer of the transactions index position log was created from. null when its pending log.
  - transactionHash: DATA, 32 Bytes - hash of the transactions this log was created from. null when its pending log.
  - blockHash: DATA, 32 Bytes - hash of the block where this log was in. null when its pending. null when its pending log.
  - blockNumber: QUANTITY - the block number where this log was in. null when its pending. null when its pending log.
  - address: DATA, 20 Bytes - address from which this log originated.
  - data: DATA - contains one or more 32 Bytes non-indexed arguments of the log.
  - topics: Array of DATA - Array of 0 to 4 32 Bytes DATA of indexed log arguments. (In solidity: The first topic is the hash of the signature of the event (e.g. Deposit(address,bytes32,uint256)), except you declared the event with the anonymous specifier.)

  ## eth_getFilterLogs

Returns an array of all logs matching filter with given id.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getFilterLogs",
          "params": [
            "0x16", // 22
          ],
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`WORK ARRAY`

| Parameter | Description    |
| --------- | -------------- |
| QUANTITY  | the filter id. |

### Returns

**See** [eth_getFilterChanges](#eth_getFilterChanges)

## eth_getLogs

Returns an array of all logs matching a given filter object.

```shell


curl https://api.web3.nucal.com/ethereum \
    -X POST \
    -H "Content-Type: application/json" \
    -H "Authorization: Bearer <token> \
    -d '{
          "jsonrpc":"2.0",
          "method":"eth_getFilterLogs",
          params: [{
            topics: [
              "0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b",
            ],
          }]
          "id":1
        }'
```

> The above command returns JSON structured like this:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": [{
    "logIndex": "0x1", // 1
    "blockNumber":"0x1b4", // 436
    "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
    "transactionIndex": "0x0", // 0
    "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
    "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
    "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
    },{
      ...
    }]
}
```

### Request

`POST https://api.web3.nucal.com/ethereum`

### Headers

`Content-Type: application/json`

`Authorization: Bearer <token>`

### Parameters

`WORK ARRAY`

| Parameter | Description                                                                                                                                                                                                                                                                                                                                                                                 |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fromBlock | QUANTITY (optional, default: "latest") Integer block number, or "latest" for the last mined block or "pending", "earliest" for not yet mined transactions.                                                                                                                                                                                                                                  |
| toBlock   | (optional, default: "latest") Integer block number, or "latest" for the last mined block or "pending", "earliest" for not yet mined transactions.                                                                                                                                                                                                                                           |
| topics    | Array of DATA, - (optional) Array of 32 Bytes DATA topics. Topics are order-dependent. Each topic can also be an array of DATA with "or" options.                                                                                                                                                                                                                                           |
| address   | QUANTITY (optional, default: "latest") Integer block number, or "latest" for the last mined block or "pending", "earliest" for not yet mined transactions.                                                                                                                                                                                                                                  |
| blockhash | DATA, 32 Bytes - (optional, future) With the addition of EIP-234, blockHash will be a new filter option which restricts the logs returned to the single block with the 32-byte hash blockHash. Using blockHash is equivalent to fromBlock = toBlock = the block number with hash blockHash. If blockHash is present in the filter criteria, then neither fromBlock nor toBlock are allowed. |

### Returns

**See** [eth_getFilterChanges](#eth_getFilterChanges)
