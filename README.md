# How to run a local Geth node

A no-nonsense guide to running a local [Geth](https://geth.ethereum.org/) node for smart contract development and testing.

## Step-By-Step

* Install [Geth](https://geth.ethereum.org/)
* Create an empty folder where the node data will be
* Execute ```shellgeth account new``` 
  * Backup the password - locally I used ‘12345’
  * Note the address: 0xa024670E7E3ef957b781bF5e09C2b8b8157bfD87 in my case
  * Note the location of the keystore: C:\Users\tj\AppData\Local\Ethereum\keystore\ in my case
* Create a'genesis.json' file:
```json
{
  "config": {
    "chainId": 9,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "clique": {
      "period": 1,
      "epoch": 1
    }
  },
  "difficulty": "1",
  "gasLimit": "8000000",
  "extradata": "0x0000000000000000000000000000000000000000000000000000000000000000a024670E7E3ef957b781bF5e09C2b8b8157bfD870000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "alloc": {
    "7df9a875a174b3bc565e6424a0050ebc1b2d1d82": { "balance": "300000" },
    "f41c74c9ae680c1aa78f42e5647a62f353b7bdde": { "balance": "400000" },
    "a024670E7E3ef957b781bF5e09C2b8b8157bfD87": { "balance": "100000000000000000000000000000" }
  }
}
```
* Note the PoA consensus (clique), 'difficulty' and initial balance for the address ending in D87
* Note the chain id - I put '9'.
* Initialise the genesis block via ```geth.exe --datadir=C:\Users\tj\TOOLS\local-geth init genesis.json```
* I have copied the keystore file from the default location into the local keystore folder
* Run the command ```"C:\Users\tj\TOOLS\Geth1.10.15\geth.exe" --http --http.api personal,eth,net,web3 --ipcpath tj.ipc --rpc.txfeecap 0 --rpc.gascap 0 --syncmode "full" --verbosity 3 --nodiscover --datadir=C:\Users\tj\TOOLS\local-geth --unlock 0xa024670e7e3ef957b781bf5e09c2b8b8157bfd87 --mine --allow-insecure-unlock --password C:\Users\tj\TOOLS\local-geth\pwd```
  * Note this is unsecure - this is a local node for dev - no discovery allowed
  * The password '12345' is stored in a file called 'pwd' (with a line return at the end)
  * The unlock parameters are used for mining and signing
* Funding happens via the console using the pip data which can be found in the nodes logs
  * Funding using ipc (url=\\.\pipe\tj.ipc): ```geth.exe attach \\.\pipe\tj.ipc```
  * ```eth.sendTransaction({from: "0xa024670e7e3ef957b781bf5e09c2b8b8157bfd87",to: "0xD1eCf18C4703F524B787B9A44d7EcF58365EA74F", value: "740000000"})```
  * And check balance using ```eth.getBalance("0xD1eCf18C4703F524B787B9A44d7EcF58365EA74F")```

