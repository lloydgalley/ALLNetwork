# Run an ALLTRA validator on Nethermind 1.29.1

Greetings traveller,

I am ***"ALLTRA"***, your ***A***dvanced ***L***ogistics and ***L***ife-support ***T***echnology ***R***esource ***A***ssistant.

I see that you are interested in creating a Validator node on the ALLTRA Smartchain. (Please read the WARNING! at the bottom of the page before you decide to continue).

This folder contains everything needed to run a Nethermind node on the ALLTRA Smartchain.

## Required files

| File | Role |
|------|------|
| `nethermind-1.29.1.zip` | Download from https://alltra.global/static/nethermind-1.29.1.zip |
| `all.cfg` | ALL chain config |
| `balance-recovery.json` | AuRa native-balance remap (`Aura.BalanceRecoveryFilePath`) |
| `ALL/spec/spec.json` | ALL chainspec — copy to `chainspec/all.json` |

## 1. Download and unpack Nethermind

From this `node-example` directory:

```bash
mkdir -p /root/ALL
wget -O nethermind-1.29.1.zip https://alltra.global/static/nethermind-1.29.1.zip
unzip nethermind-1.29.1.zip -d /root/ALL
cd /root/ALL
chmod +x nethermind
```

The working directory must contain the `nethermind` binary plus `configs/`, `chainspec/`, and `keystore/`.

If the zip unpacks into a nested folder (for example `nethermind-linux-x64/`), `cd` into the folder that contains the `nethermind` binary.

## 2. Install config, chainspec, and recovery file

Still from `node-example`:

```bash
NETHERMIND_HOME=/root/ALL

cp all.cfg "$NETHERMIND_HOME/configs/all.cfg"
cp balance-recovery.json "$NETHERMIND_HOME/configs/balance-recovery.json"
mkdir -p "$NETHERMIND_HOME/chainspec" "$NETHERMIND_HOME/keystore"
cp ALL/spec/spec.json "$NETHERMIND_HOME/chainspec/all.json"
```

`all.cfg` already uses paths relative to the Nethermind working directory:

- `Init.ChainSpecPath` = `chainspec/all.json`
- `Aura.BalanceRecoveryFilePath` = `configs/balance-recovery.json`
- `KeyStore.KeyStoreDirectory` = `keystore`
- `KeyStore.PasswordFiles` = `keystore/pass-new.pwd`

## 3. Create an Account

Copy the example node (includes `openethereum`) into the Nethermind home if you have not already:

```bash
cp -a ALL/. /root/ALL/
```

Then create the account:

```bash
cd /root/ALL
chmod +x openethereum
./openethereum account new --config nodes/validator/node.toml
```

Returned address like that `0x00aa39d30f0d20ff03a22ccfc30b7efbfca597c2`

Copy the result address to `configs/all.cfg`:

```json
"KeyStore": {
  "PasswordFiles": ["keystore/pass-new.pwd"],
  "UnlockAccounts": ["0x00aa39d30f0d20ff03a22ccfc30b7efbfca597c2"],
  "KeyStoreDirectory": "keystore",
  "EnodeAccount": "0x00aa39d30f0d20ff03a22ccfc30b7efbfca597c2"
}
```

Copy the keystore and password so Nethermind can unlock the same account:

```bash
mkdir -p /root/ALL/keystore
cp /root/ALL/nodes/validator/keys/ALL/UTC--* /root/ALL/keystore/
cp /root/ALL/nodes/validator/node.pwd /root/ALL/keystore/pass-new.pwd
```

You can find your keystore (containing your private key) and the password for the created account in:

```
/ALL/nodes/validator/keys/ALL/UTC--xxxx
/ALL/nodes/validator/node.pwd
```

Set `EthStats.Name` to your node name (unique per validator).

## 4. Ports and firewall

Defaults in `all.cfg`:

| Service | Port |
|---------|------|
| P2P / Discovery | `30301` TCP + UDP |
| JSON-RPC | `8545` |
| WebSocket | `8546` |

Expose P2P. Keep RPC/WS on localhost or firewalled; `all.cfg` currently sets `JsonRpc.Host = 0.0.0.0`.

Default bootnodes:

```
enode://bea96dc68a7dedc99f5eb78d07e46885d5ec77a3ffbf8db58d263a530fae6bcd71d82c8578f0a9bed9e2fe6321901d5a65add4a78d2d919d54f3814def986c3a@103.42.59.57:30300
enode://84aeeaf6c32b23dc3fcc5daa26e55329a5627af48eff8e078c061fbc657155a87bcb0c63cebcb7f191dcbe22f8f10c5f69f6195ea4252d3930e0e97e91887c20@103.42.59.56:30300
enode://405544b9354f79168e4e663fb085d7dc40c6fab1d768525b13c5025b2f4f7cf8d7fbdc6f0c1c2b864fa4bba2514bfb52fac09e6449efb7fb2d5db10f44888d9a@103.42.59.55:30300
```

## 5. Run the node

```bash
cd /root/ALL
./nethermind -c all
```

Equivalent:

```bash
./nethermind --config configs/all.cfg
```

Run in the background:

```bash
mkdir -p logs
nohup ./nethermind -c all >> logs/all.console.log 2>&1 &
```

Logs: `logs/all.logs.txt` (`Init.LogFileName`). Database: `nethermind_db/all`.

Check RPC:

```bash
curl -s -X POST http://127.0.0.1:8545 \
  -H 'Content-Type: application/json' \
  --data '{"jsonrpc":"2.0","id":1,"method":"eth_blockNumber","params":[]}'
```

`Mining.Enabled = true` is required for the node to seal AuRa blocks.

## 6. Stake

Send ALL from **the validator address** to the Consensus contract on the ALL network:

```
0x2aC1a9CaE1E1edBE4Ffd9134342a25C96283B07e
```

The easiest way is to import the keystore file (`keystore/UTC--...`) and password (`keystore/pass-new.pwd`) into a wallet (for example MetaMask), switch to the ALL network, and send ALL to the contract.

Wait **1 cycle** (approximately 48 hours) for the validator to enter the sealer set.

## Directory layout after setup

```
/root/ALL/
  nethermind
  configs/all.cfg
  configs/balance-recovery.json
  chainspec/all.json
  keystore/pass-new.pwd
  keystore/UTC--...
  nethermind_db/all/          # created at runtime
  logs/all.logs.txt           # created at runtime
```

***WARNING! - Please note that all Validators WILL need to be verified by the ALLTRA Head Office to complete the validation process. Any unauthorised validations that are attempted on the chain WILL be removed and barred from the chain.***


   I am always at your service.
   May fortune favor you.

   ***THANK YOU AND HAPPY TRAVELS***

***ALLTRA**
