# dashsight.js

SDK for Dash's flavor of the Insight API

# TODO

- Do we want to normalize duffs?

# Install

```bash
npm install --save dashsight
```

# Usage

```js
"use strict";

require("dotenv").config({ path: ".env" });

let dashsightBaseUrl =
  process.env.INSIGHT_BASE_URL || "https://insight.dash.org";

let dashsight = require("dashsight").create({
  baseUrl: dashsightBaseUrl,
});

dashsight.getInstantBalance(address).then(function (info) {
  console.info(`Current balance is: Đ${info.balance}`);
});
```

# API

| `Dashsight.create({ baseUrl })`        |
| -------------------------------------- |
| `dashsight.getInstantBalance(addrStr)` |
| `dashsight.getTx(txIdHex)`             |
| `dashsight.getTxs(addrStr, maxPages)`  |
| `dashsight.getUtxos(addrStr)`          |
| `dashsight.instantSend(txHex)`         |

## `Dashsight.create({ baseUrl })`

Creates an instance of the insight sdk bound to the given baseUrl.

```js
let Dashsight = = require("dashsight");

let dashsight Dashsight.create({
  baseUrl: "https://insight.dash.org",
});
```

Note: There is no default `baseUrl` (this is supposed to be used in a
decentralized fashion, after all), but `https://insight.dash.org` might be one
you trust.

## `insight.getBalance(address)` (BUG)

Do not use. Does not give accurate balances. Provided for completeness /
compatibility only.

## `dashsight.getInstantBalance(addr)`

Takes a normal payment address, gives back the instantaneous balance (reflects
instant send TXs).

```js
// Base58Check-encoded Pay to Pubkey Hash (p2pkh)
let addr = `Xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`;

let info = await dashsight.getInstantBalance(addr);

console.log(info);
```

```json
{
  "addrStr": "Xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "balance": 10.01,
  "balanceSat": 1001000000
}
```

Note: This is not actually part of Dash's Insight API, but would be if it could
correctly calculate balances adjusted for Instant Send.

## `dashsight.getTx(txIdHex)`

## `dashsight.getTxs(addrStr)`

## `dashsight.getUtxos(addrStr)`

Gets all unspent transaction outputs (the usable "coins") for the given address.

```js
// Base58Check-encoded Pay to Pubkey Hash (p2pkh)
let addr = `Xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`;

let utxos = await dashsight.getUtxos(addr);

console.log(utxos);
```

```json
[
  {
    "address": "Xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "txid": "ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "vout": 0,
    "scriptPubKey": "00000000000000000000000000000000000000000000000000",
    "amount": 0.01,
    "satoshis": 1000000,
    "height": 1500000,
    "confirmations": 200000
  }
]
```

## `dashsight.instantSend(txHex)`
