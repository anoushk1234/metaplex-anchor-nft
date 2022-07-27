# Mint NFTs on Solana using Metaplex and Anchor

Follow along with the tutorial blog post on [medium](https://anoushk.medium.com/how-to-mint-nfts-on-solana-using-rust-and-metaplex-f66bac717cb8) or [mirror.xyz](https://mirror.xyz/anoushk.eth/HLL3JE47SJE5AnvzMVrfz7Rec7FDupXK5948aYNsDUU)


[Devnet Test Tutorial]

- Required Tools & Libraries
	- Solana CLI Tools — The official Solana CLI toolset
	- Anchor Framework — A high-level framework for developing Solana programs. This is a must unless you’re a god-level dev, in which case you're not reading this blog. Lol.
	- Solana/web3.js — A Solana version of web3.js
	- Solana/spl-token — A package to work with spl tokens
	- Mocha — A JS testing tool
	- Phantom Wallet

	

1. Set devnet config of solana and generate solana key fair
	- `$ solana config set --url devnet`
	- `$ solana-keygen new`
	- If you execute the command, the solana private key is generated in the default root (default: /Users/<user>/.config/solana/id.json).
	- In anchor-based projects (most of that), need the solana key fair in the local environment. 


2. Find the public key
	- `$ vim /Users/<user>/.config/solana/id.json`
	- Copy the private key ( [23, 123, 32, ....] )
	- private key to base58 converting [code](https://gist.github.com/Xavier59/b0b216f003b8e54db53c39397e98cd70)
	- Add a private key to the phantom wallet and check the public key (address)

	
```
// exporting from a bs58 private key to an Uint8Array
// == from phantom private key to solana cli id.json key file
// npm install bs58 @solana/web3.js

const web3 = require("@solana/web3.js");
const bs58 = require('bs58');
let secretKey = bs58.decode("[base58 private key here]");
console.log(`[${web3.Keypair.fromSecretKey(secretKey).secretKey}]`);

// exporting back from Uint8Array to bs58 private key
// == from solana cli id.json key file to phantom private key

const bs58 = require('bs58');
privkey = new Uint8Array([111, 43, 24, ...]); // content of id.json here
console.log(bs58.encode(privkey));
```


3. airdrop 1 sol to the user address

	- `$ solana airdrop 1 <ADDRESS>`
	- In Solana system need at least 1 sol in the account to make a transaction. So, we need airdrop 1 sol this account for minting (deploy or pay fee, etc.)
	- This project needs about 1.5 sol for deploying the contract. So, you should execute the airdrop at least 4-5 times (for deploying and testing). 

4. Setting Custom Path, Value
	- Some values need to be changed depending on each local path. 
	- Check the `Anchor.toml > [provider]`
		- cluster = "devnet"
		- wallet = "/Users/<user>/.config/solana/id.json"
		- Check the <user> path.
	
5. build, deploy, and test
	- Now, you can execute the test script.
	- `$ yarn install`
	- `$ anchor build`
	- `$ anchor deploy`
		- when deployment is completed, you can get the program_id (That is related metaplex nft)
		- Replace the program Id in the `Anchor.toml > [programs.devnet] > metaplex_anchor_nft`
	- `$ anchor test`


6. check the nft in your phantom wallet.
	- If the anchor test is completed successfully, you can obtain the 1 nft on your account. 
	- Check the phantom wallet -> Your Collectibles

















