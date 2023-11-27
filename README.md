# TronPunks
TronPunks are byte perfect punks on the TRON blockchain using the hex data inscriptions format. Just like TRC20 [see here](https://github.com/TRC20org/TRC20)

## What punks are available?
Look at `mints.json` for the ones already taken, the ids that are not in this file can still be minted. But be careful because it can still be a duplicate when people mint the same one at the same time.

## Indexer
In progress.

## Number of TronPunks
10,000

## Method
 - mint: `data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAkElEQVR42mNgGAWjAAT+f+v7jw3TzGCqWUKMBWRbQsjQdd2JlFmCyyCYGAxTxQJkw5D5ZFuAy7XYMEUWwAwJqDEGY2SDYWJUsQAkFNvtjGIBTGxwBxEpFpBkCT4LQNIUWYArUxHjA7J9AbMkXpYTjinKZOh24XI9SI4qpTW2sKeW4Sg+gBpKVcMZaOlyqlgAAF9AOnOqpdBpAAAAAElFTkSuQmCC`
 - transfer: TBA

## Mint TronPunk with nodejs
1. Install Node.js
2. Create a directory, such as `TronPunks`
3. Open `TronPunks`, execute command:`npm init`
4. Execute command:`npm install tronweb`
5. Create an index.js file, copy the code below
6. Pick a `base64_data` from `punk_data.json`
7. Run index.js: `node index.js` 

```
const TronWeb = require('tronweb');
const HttpProvider = TronWeb.providers.HttpProvider;
const fullNode = new HttpProvider("https://api.trongrid.io");
const solidityNode = new HttpProvider("https://api.trongrid.io");
const eventServer = new HttpProvider("https://api.trongrid.io");
const privateKey = "your privateKey"; //
const tronWeb = new TronWeb(fullNode, solidityNode, eventServer, privateKey);

const blackHole = "T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb";  //black hole address

const memo = 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAkElEQVR42mNgGAWjAAT+f+v7jw3TzGCqWUKMBWRbQsjQdd2JlFmCyyCYGAxTxQJkw5D5ZFuAy7XYMEUWwAwJqDEGY2SDYWJUsQAkFNvtjGIBTGxwBxEpFpBkCT4LQNIUWYArUxHjA7J9AbMkXpYTjinKZOh24XI9SI4qpTW2sKeW4Sg+gBpKVcMZaOlyqlgAAF9AOnOqpdBpAAAAAElFTkSuQmCC';  

async function main() {

    const unSignedTxn = await tronWeb.transactionBuilder.sendTrx(blackHole, 1); //0.000001 TRX is the minimum transfer amount.
    const unSignedTxnWithNote = await tronWeb.transactionBuilder.addUpdateData(unSignedTxn, memo, 'utf8');
    const signedTxn = await tronWeb.trx.sign(unSignedTxnWithNote);
    console.log("signed =>", signedTxn);
    const ret = await tronWeb.trx.sendRawTransaction(signedTxn);
    console.log("broadcast =>", ret);
}

main().then(() => {

    })
    .catch((err) => {
        console.log("error:", err);
    });
```

## Mint TronPunks with TokenPocket Wallet

 - Pick a base64_data from punk_data.json, copy the base64 that starts with "data:"
 - Receiver address:T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb.
 - Transfer amount 0.000001 TRX
 - Click on Advanced Settings and fill in the base64_data chosen, for example punk 0000: `data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAkElEQVR42mNgGAWjAAT+f+v7jw3TzGCqWUKMBWRbQsjQdd2JlFmCyyCYGAxTxQJkw5D5ZFuAy7XYMEUWwAwJqDEGY2SDYWJUsQAkFNvtjGIBTGxwBxEpFpBkCT4LQNIUWYArUxHjA7J9AbMkXpYTjinKZOh24XI9SI4qpTW2sKeW4Sg+gBpKVcMZaOlyqlgAAF9AOnOqpdBpAAAAAElFTkSuQmCC`



## Idea for developing indexer
1. Get all transactions (of black hole address) and see which unique mints are first.
2. Use the FULL NODE HTTP API to get the `from`,`to`,`data` field of each txid, match all mint inscriptions. For more details, please refer to the following link: https://developers.tron.network/reference/wallet-gettransactionbyid.
3. The obtained addresses need to undergo encoding conversion to Tron wallet addresses. For more details, please refer to the following link: https://www.btcschools.net/tron/tron_tool_base58check_hex.php.

## Indexer(TBA)


## FAQ
### Why you need transfer to `T9yD14Nj9j7xAB4dbGeiX9h8unkKHxuWwb` account?
Because TRON blockchain cannot transfer to yourself address.We decided transfer to the black hole address of the TRON blockchain. Refer to the black hole address given in the official [documentation](https://developers.tron.network/docs/faq#3-what-is-the-destruction-address-of-tron) of the TRON blockchain. 

### Why you need transfer to 0.000001 TRX to black hole address?
Because TRON blockchain cannot transfer zero amount, 0.000001 TRX is the minimum transfer amount.




