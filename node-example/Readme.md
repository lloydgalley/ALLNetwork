# Run a ALL Validator

Greetings traveller, 

I am ***"ALLTRA"***, your ***A***dvanced ***L***ogistics and ***L***ife-support ***T***echnology ***R***esource ***A***ssistant. 

I see that you are interesrted in creating a Validator node on the ALLTRA Smartchain. (Please read the WARNING! at the bottom of the page before you decide to continue).


## Setting up a node
1. Git clone https://github.com/lloydgalley/ALLNetwork.git

2. Copy source form node-example to root folder
```
cp -r ALLNetwork/node-example/ALL  /root/
```
3. Create an Account

```
cd /root/ALL
chmod +x openethereum
./openethereum account new --config nodes/validator/node.toml
```
Returned address like that 0x00aa39d30f0d20ff03a22ccfc30b7efbfca597c2

Copy result address to mode.toml
Ex:
```
...
[account]
unlock = ["0x00aa39d30f0d20ff03a22ccfc30b7efbfca597c2"]
password = ["password"]

[mining]
force_sealing = true
engine_signer = "0x00aa39d30f0d20ff03a22ccfc30b7efbfca597c2"
reseal_on_txs = "none"
...
```
4. Run the authority nodes
```
./openethereum --config ./nodes/validator/node.toml

```
5. Stake

    Stake

    To stake ALL coin, all you should do is sending your ALL coin to the ALL Consensus contract address over the ALL network from the validator address.
    The ALL Consensus contract address: 0x2aC1a9CaE1E1edBE4Ffd9134342a25C96283B07e
    The easiest way to do so, is to import your private key or key-store file to your favourite wallet (for example Metamask), switch network to ALL and send the ALL coin to the Consensus contract address.

    You can find your key-store (containing your private key) and the password for the created account in:
    /ALL/nodes/validator/keys/ALL/UTC--xxxx
    /ALL/nodes/validator/node.pwd

6. Wait for 1 cycle (approximately 48 hours).


***NOTE: WARNING! - Please note that all Validators WILL need to be verified by the ALLTRA Head Office to complete the validation process. Any unauthorised validations that are attempted on the chain WILL be removed and barred from the chain.***

    Wait until the next cycle gets started.

   I am always at your service.
   May fortune favor you.

   ***THANK YOU AND HAPPY TRAVELS***

***ALLTRA***   


