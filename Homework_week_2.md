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

#### Cast my vote:
https://sepolia.etherscan.io/tx/0x8e62959fcee2101b8967cc57da4577c383e5f7eb505984be1fc48e0eba71efbd

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


