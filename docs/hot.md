# Getting started
First of all you will need an Ethereum account, that will serve as a hot wallet.
You can generate one safely at: [MyEtherWallet](https://www.myetherwallet.com/#generate-wallet)

  * Type whatever password (don't need to remember it)
  * Click Generate
  * Copy and save `Private Key (unencrypted)` to a secure place. **Attention** anyone who knows this private key,
has access to your entire account
  * Copy and save `Your Address` somewhere. This will serve as your **Hot Wallet Address**
  * It is mandatory to have a separate Hot Wallet Address for every asset
  * Maintain some ETH on balance of your Hot Wallet Address.

Preconditions:

  * You know asset of interest contract address, e.g. [CryptoCarbon](http://explorer.ambisafe.co/#/asset/CC) `0xe4c94d45f7aef7018a5d66f44af780ec6023378e`
  * You know asset of interest ICAP currency code, e.g. **BCC** for CryptoCarbon
## Balance
This section will explain on how to get balance of your Hot Wallet Address.
### [Server-side Examples](https://github.com/Ambisafe/etoken-server-side-examples)
Preconditions:

  * You know asset of interest base unit(decimals) number, found at [Assets](http://explorer.ambisafe.co/#/assets), e.g. CryptoCarbon `6`
  * Set `config.js` parameters `address`(asset's contract address) and `baseUnit` accordingly.

```javascript
  'address': '0xe4c94d45f7aef7018a5d66f44af780ec6023378e', // CC
  'baseUnit': 6,
```
You can get balance with, replacing the `{address}` with your Hot Wallet Address:
```bash
node get-balance.js {address}
```
Check out example usages for **Ruby** and **PHP** in the repo, if neccessary.
### [ETokenD](http://etokend-docs.ambisafe.co), can be bought in [Ambisafe Store](https://www.ambisafe.co/product/list)
Preconditions:

  * Start ETokenD with the following parameters: `--assetaddress 0xe4c94d45f7aef7018a5d66f44af780ec6023378e --asset BCC --privatekey {PrivateKeyOfHotWallet}`

You can get balance with the following JSON RPC request:
```bash
curl localhost:18545 -d '{"jsonrpc":"2.0","method":"getbalance","params":[],"id":0}'
```
## Receive deposits
This section will explain on how to get paid.
### [Notificator](http://etoken-tutorial.readthedocs.io)
### [ETokenD](http://etokend-docs.ambisafe.co), can be bought in [Ambisafe Store](https://www.ambisafe.co/product/list)
Preconditions:

  * You know your company ICAP institution code, e.g. **MYCO**, request through contacting [support@ambisafe.co](mailto:support@ambisafe.co)
  * Asset of interest ICAP currency code with your company ICAP institution code is bound to your Hot Wallet Address, request through contacting [support@ambisafe.co](mailto:support@ambisafe.co)
  * Start ETokenD with the following parameters:
```
--assetaddress 0xe4c94d45f7aef7018a5d66f44af780ec6023378e
--asset BCC
--institution {ICAPInstitutionCode}
--privatekey {PrivateKeyOfHotWallet}
```

#### Deposits
Call `getnewaddress` once for each user account:
```bash
curl localhost:18545 -d '{"jsonrpc":"2.0","method":"getnewaddress","params":[],"id":0}'
```
Assign resulting deposit address to the user account, and ask user to do deposits to this same address every time.
#### Invoices
Call `getnewaddress` once for each new invoice:
```bash
curl localhost:18545 -d '{"jsonrpc":"2.0","method":"getnewaddress","params":[],"id":0}'
```
Assign resulting address to the invoice, and ask user to do invoice payment to this address.
**********
To know when payment arrived you can setup [Notificator](http://etoken-tutorial.readthedocs.io), deposit/invoice address can be accessed through `notification.eventData.icap`.
Another option is to poll ETokenD for incoming transactions:
```bash
curl localhost:18545 -d '{"jsonrpc":"2.0","method":"listtransactions","params":[],"id":0}'
```
Deposit/invoice address can be accessed through `transaction.icap`.
**Note**: payment's unique id is `transactionHash + logIndex`, make sure to save it in order to not double process the same payment.
## Send payouts
This section will explain how to do payouts.
### [Server-side Examples](https://github.com/Ambisafe/etoken-server-side-examples)
Preconditions:

  * You know asset of interest base unit(decimals) number, found at [Assets](http://explorer.ambisafe.co/#/assets), e.g. CryptoCarbon `6`
  * Set `config.js` parameters `address`(asset's contract address) and `baseUnit` accordingly.

```javascript
  'address': '0xe4c94d45f7aef7018a5d66f44af780ec6023378e', // CC
  'baseUnit': 6,
  'icapAssetCode': 'BCC',
  'privateKey': {PrivateKeyOfHotWallet}
```
You can send funds, replacing the `{destination}` with the recipient address or ICAP, and `{amount}` with desired amount(1.1 will result in 1100000 CC being sent):
```bash
node send-tx.js {destination} {amount}
```
Check out example usages for **Ruby** and **PHP** in the repo, if neccessary.
### [ETokenD](http://etokend-docs.ambisafe.co), can be bought in [Ambisafe Store](https://www.ambisafe.co/product/list)
Preconditions:

  * Start ETokenD with the following parameters: `--assetaddress 0xe4c94d45f7aef7018a5d66f44af780ec6023378e --asset BCC --privatekey {PrivateKeyOfHotWallet}`

Call `sendtoaddress` replacing the `{destination}` with the recipient address or ICAP, and `{amount}` with desired amount(1.1 will result in 1100000 CC being sent):
```bash
curl localhost:18545 -d '{"jsonrpc":"2.0","method":"sendtoaddress","params":["{destination}","{amount}"],"id":0}'
```
## ETH through EToken
This section will explain how to work with ETH.
Preconditions:

  * You know your company ICAP institution code, e.g. **MYCO**, request through contacting [support@ambisafe.co](mailto:support@ambisafe.co)
  * WE4 asset with your company ICAP institution code is bound to your Hot Wallet Address, request through contacting [support@ambisafe.co](mailto:support@ambisafe.co)
