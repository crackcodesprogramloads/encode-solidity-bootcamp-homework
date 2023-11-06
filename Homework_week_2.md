# Weekend 2 Homework 
Team 1's exercise to Connect to Ballot.sol over public blockchain and perform interactions

## Tasks 
This is a group activity for at least 3 students:
Develop and run scripts for “Ballot.sol” within your group to give voting rights, casting votes, delegating votes and querying results
Write a report with each function execution and the transaction hash, if successful, or the revert reason, if failed
Submit your code in a github repository in the form

### @Monk's transactions:
#### Deploy the contract
Transaction hash: https://sepolia.etherscan.io/tx/0x94921437371a17cd33dcaebf70efc3d0bb827ea9f99fb8d345680f15f2ad9a88
Contract hash:0xec5bfec0298b373a6b57cb4f0b4403fcb5dae33a

#### Provide voting rights to Mike
https://sepolia.etherscan.io/tx/0x76d999cb5efe2f9c6f27aa3aaa76f69163b867ccce8e95e5fd760e217883fc66

#### Receive vote from Mike
https://sepolia.etherscan.io/tx/0x1ce6084038279d75a8d5075e1b18d6ebc614c41d68d65a2f4de1755516b02730

#### Cast my vote::
https://sepolia.etherscan.io/tx/0x8e62959fcee2101b8967cc57da4577c383e5f7eb505984be1fc48e0eba71efbd

### Output of get the winning proposal:
C:\Users\HP\Desktop\temp\project2>npx ts-node ./scripts/WinningProposal.ts 0xeC5
bfeC0298B373a6B57cB4F0B4403FcB5DAe33A
Last block number: 4626658
Last block timestamp: 1699077216 (11/4/2023 1:53:36 PM)
Using address 0xF122871816918a04108E051Caa31c6438718b61e
Wallet balance 0.46192266584245767 ETH
1n


#### Again cast vote:
Again case vote:
Error:
Last block number: 4626614
Last block timestamp: 1699076652 (11/4/2023 1:44:12 PM)
Using address 0xF122871816918a04108E051Caa31c6438718b61e
Wallet balance 0.46192266584245767 ETH
Error: execution reverted: "Already voted." (action="estimateGas", data="0x08c37
9a000000000000000000000000000000000000000000000000000000000000000200000000000000
00000000000000000000000000000000000000000000000000e416c726561647920766f7465642e0
00000000000000000000000000000000000", reason="Already voted.", transaction={ "da
ta": "0x0121b93f0000000000000000000000000000000000000000000000000000000000000001


### @Monk's Additional functions:
#### GiveRightToVote.ts
import { ethers } from "hardhat";
import * as dotenv from 'dotenv';
import { Ballot, Ballot__factory } from "../typechain-types";
dotenv.config();
//const PROPOSALS = ["Proposal 1", "Proposal 2", "Proposal 3"];

async function main() {
    const parameters = process.argv.slice(2);
    if (!parameters || parameters.length < 2)
        throw new Error("Parameters not provided");
    const contractAddress = parameters[0];
    const addressOfVoter = parameters[1];

    const provider = new ethers.JsonRpcProvider(process.env.RPC_ENDPOINT_URL ?? "");
    // const provider = ethers.getDefaultProvider("sepolia");
    const lastBlock = await provider.getBlock('latest');
    console.log(`Last block number: ${lastBlock?.number}`);
    const lastBlockTimestamp = lastBlock?.timestamp ?? 0;
    const lastBlockDate = new Date(lastBlockTimestamp * 1000);
    console.log(`Last block timestamp: ${lastBlockTimestamp} (${lastBlockDate.toLocaleDateString()} ${lastBlockDate.toLocaleTimeString()})`);

    const wallet = new ethers.Wallet(process.env.PRIVATE_KEY ?? "", provider)
    console.log(`Using address ${wallet.address}`);
    const balanceBN = await provider.getBalance(wallet.address);
    const balance = Number(ethers.formatUnits(balanceBN));
    console.log(`Wallet balance ${balance} ETH`);
    if (balance < 0.01) {
        throw new Error("Not enough ether");
    }

    const ballotFactory = new Ballot__factory(wallet);
    const ballotContract = ballotFactory.attach(contractAddress) as Ballot;
    const tx = await ballotContract.giveRightToVote(addressOfVoter);
    const receipt = await tx.wait();
    console.log(`Transaction completed ${receipt?.hash}`)
}

main().catch((error) => {
    console.error(error);
    process.exitCode = 1;
});

#### @Monk's Additional functions:WinningProposal.ts
import { ethers } from "hardhat";
import * as dotenv from 'dotenv';
import { Ballot, Ballot__factory } from "../typechain-types";
dotenv.config();
//const PROPOSALS = ["Proposal 1", "Proposal 2", "Proposal 3"];

async function main() {
    const parameters = process.argv.slice(2);
    if (!parameters || parameters.length < 1)
        throw new Error("Parameters not provided");
    const contractAddress = parameters[0];
    //const addressOfVoter = parameters[1];

    const provider = new ethers.JsonRpcProvider(process.env.RPC_ENDPOINT_URL ?? "");
    // const provider = ethers.getDefaultProvider("sepolia");
    const lastBlock = await provider.getBlock('latest');
    console.log(`Last block number: ${lastBlock?.number}`);
    const lastBlockTimestamp = lastBlock?.timestamp ?? 0;
    const lastBlockDate = new Date(lastBlockTimestamp * 1000);
    console.log(`Last block timestamp: ${lastBlockTimestamp} (${lastBlockDate.toLocaleDateString()} ${lastBlockDate.toLocaleTimeString()})`);

    const wallet = new ethers.Wallet(process.env.PRIVATE_KEY ?? "", provider)
    console.log(`Using address ${wallet.address}`);
    const balanceBN = await provider.getBalance(wallet.address);
    const balance = Number(ethers.formatUnits(balanceBN));
    console.log(`Wallet balance ${balance} ETH`);
    if (balance < 0.01) {
        throw new Error("Not enough ether");
    }

    const ballotFactory = new Ballot__factory(wallet);
    const ballotContract = ballotFactory.attach(contractAddress) as Ballot;
    const winningProposalIndex = await ballotContract.winningProposal();
    //const receipt = await tx.wait();
    console.log(winningProposalIndex)
}

main().catch((error) => {
    console.error(error);
    process.exitCode = 1;
});

### Mike's transactions
#### Casting a vote to proposal 1
https://sepolia.etherscan.io/tx/0x1292257b22bdf4cd9159a0a8d0c6d850f77a9e7b73d4d5d86ba75c35b3e07c3c

--------------------------------------------------------------------------------------------------------

### t0m0's transactions
#### Deploying a new contract
https://sepolia.etherscan.io/address/0xef50e690f7f446565db61b6aa3403c53601a7432
#### Voting proposal 1
https://sepolia.etherscan.io/tx/0x8dfcc661b40efd2cff3a8a24b23014c5725ee058a07b6528547c22e0d48614b6
#### Gave Mike the right to vote
https://sepolia.etherscan.io/tx/0x6b0ed45be0aaa79eca2ba1a9eb783f4ef1c2b800f43f34f7c4ad81946c79d627
#### Gave Loren the right to vote
https://sepolia.etherscan.io/tx/0xbf38d8b49fbd515866c40a780f00b71fd66a93c83fab1df90ee2335f27786cf5

### Mike's transactions
#### Delegating a vote to Loren
https://sepolia.etherscan.io/tx/0x2860a1455623b12e4f0e069ccbf7b6d21ac1536f2c70219f0aad1223f5e76715

### Loren's transactions
#### Vote for proposal 1
https://sepolia.etherscan.io/tx/0x659585c1d6e9e2fa1977737c2669c321d625f4f7e9d5143e7c28603614e408d3

------------------------------------------------------------------------------------------------------

### Cerise transactions
#### Deploying new contract
https://sepolia.etherscan.io/tx/0xb6b1b1d7652b0c12b098e88dbd032010c4102399fa04d8fa7067577585ceff57
#### Giving Mike right to vote
https://sepolia.etherscan.io/tx/0xef6fe7918c93bd1ae37c7a208a3bbc2d1eaf92e4d533c595fbd39ab1ba81a4ee
#### Giving Kent's first address right to vote
https://sepolia.etherscan.io/tx/0xed43047bc8315528953f26f95c97bbb1b1f5fe2d8da591979e603dc6d45d8fd7
#### Giving Kent's second address right to vote
https://sepolia.etherscan.io/tx/0xc3b96241b68e6e8f612c36b41c153dbfc9462922eeeff5322e13115605411710
#### Voting proposal 4
https://sepolia.etherscan.io/tx/0x303ba8fc5fa8bb2d7fd6a7e8384bc089f6fc61ec6fb6aac7ebae8ca78921cf34

### Mike's transactions
#### Delegating a vote to Cerise
https://sepolia.etherscan.io/tx/0x1292257b22bdf4cd9159a0a8d0c6d850f77a9e7b73d4d5d86ba75c35b3e07c3c

### Kent's txns
#### Vote for proposal 1
https://sepolia.etherscan.io/tx/0xeb2b7c6218693d3559d286523226de0befd63fa337303e4e7ba492d1a2299796

#### Delegate
https://sepolia.etherscan.io/tx/0x6d6bfa651dc27d0190e67d8cfd62ae224f0e2f3ca7e4be7b11991db92af013dc

#### Delegated voter cannot use delegated vote if that voter has already voted
https://sepolia.etherscan.io/tx/0x8f88c329c604db6ac52bb3008adac581a45bc07ecf910d6fa3141ffd06a78fcd
