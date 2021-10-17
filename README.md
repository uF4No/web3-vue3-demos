# web3-vue3-demos

## References

- **The Complete Guide to Full Stack Ethereum Development**

  - Dev.to: https://dev.to/dabit3/the-complete-guide-to-full-stack-ethereum-development-3j13
  - YT: https://www.youtube.com/watch?v=a0osIaAOFSE&t=25s

- Crypto Dev Hub: https://cryptodevhub.io/wiki/blockchain-development-tutorial

- Official tutorials: https://ethereum.org/en/developers/tutorials/
- Crypto Market Pool : https://cryptomarketpool.com/getting-started-with-solidity/

## Concepts/Libraries

### Hardhat

It's an Ethereum develpment environment that allows you to run Solidity locally. Looks like it's used to bootstrap a Solitidy project using the command `npx hardhat` (choose create sample project) which generates a basic Solidity smart contract and a script to deploy it in a `scripts` folder.

There is a config file `hardhat.config.js` in which can include settings like the output folder for the compiled contracts, networks/blockchains etc

We can use Hardhat to compile our .sol files to machine code using the command `npx hardhat compile`.

To generate a local blockchain node, we can run `npx hardhat node`. This will generate a local test network (blockchain) with around 20 accounts with a bunch of ETH each.

To deploy our compiled contract, run `npx hardhat run scripts/deploy.js --network localhost` (notice the path of the script points to the folder with the actual script files)

Other similar tools in the ecosystem are Ganache and Truffle.

### Ethers.js

A library that allows Javascript applications to interact with the Ethereum blockchain. Basically it allows you to read data and send new transactions.

Another similar option in the ecosystem is web3.js

### Metamak

It's used to manage Ethereum accounts and wallets from your browser. We can use

#### Connect Metamask to local Ethereum node

Make sure there is an Ethereum node actually running. You can start one with `npx hardhat node`, (this will whow a bunch of accounts in the console with their correspondent private key) then choose the blockchain you want Metamask to connect to from the dropdown.

Once Metamask is pointing to the local ethereum node, click on the user avatar and **Import Account** using a private key.

## Using Compiled contracts/artifacts in a Frontend application

We need to compile our contracts using Hardhat `npx hardhat compile` and then import the generated JSON file into our fronend app. We'll use the ABI, **Application Binary Interface**
