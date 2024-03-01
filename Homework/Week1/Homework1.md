# Homework Week 1


## 1. Why is client diversity important for Ethereum ?
Having many independently developed and maintained clients is vital for the health of a decentralized network. Let's explore the reasons why.

**Bugs**
A bug in an individual client is less of a risk to the network when representing a minority of Ethereum nodes. With a roughly even distribution of nodes across many clients, the likelihood of most clients suffering from a shared issue is small, and as a result, the network is more robust.

**Resilience to attacks**
Client diversity also offers resilience to attacks. For example, an attack that tricks a particular client(opens in a new tab) onto a particular branch of the chain is unlikely to be successful because other clients are unlikely to be exploitable in the same way and the canonical chain remains uncorrupted. Low client diversity increases the risk associated with a hack on the dominant client. Client diversity has already proven to be an important defense against malicious attacks on the network, for example the Shanghai denial-of-service attack in 2016 was possible because attackers were able to trick the dominant client (Geth) into executing a slow disk i/o operation tens of thousands of times per block. Because alternative clients were also online which did not share the vulnerability, Ethereum was able to resist the attack and continue to operate while the vulnerability in Geth was fixed.

**Proof-of-stake finality**
A bug in a consensus client with over 33% of the Ethereum nodes could prevent the consensus layer from finalizing, meaning users could not trust that transactions would not be reverted or changed at some point. This would be very problematic for many of the apps built on top of Ethereum, particularly DeFi.

🚨 Worse still, a critical bug in a client with a two-thirds majority could cause the chain to incorrectly split and finalize(opens in a new tab), leading to a large set of validators getting stuck on an invalid chain. If they want to rejoin the correct chain, these validators face slashing or a slow and expensive voluntary withdrawal and reactivation. The magnitude of a slashing scales with the number of culpable nodes with a two-thirds majority slashed maximally (32 ETH).
Although these are unlikely scenarios, the Ethereum eco-system can mitigate their risk by evening out the distribution of clients across the active nodes. Ideally, no consensus client would ever reach a 33% share of the total nodes.

**Shared responsibility**
There is also a human cost to having majority clients. It puts excess strain and responsibility on a small development team. The lesser the client diversity, the greater the burden of responsibility for the developers maintaining the majority client. Spreading this responsibility across multiple teams is good for both the health of Ethereum's network of nodes and its network of people.

## 2. Where is the full Ethereum state held ?
Conceptually, there are 2 important components of an account-based blockchain:

transactions represent state transition functions
the result of these functions can be stored
A "full/archival" node might store all transactions and resulting state transitions for all block heights in a local data store. This would include all historical states, even those no longer valid. This allows clients to query the state of the blockchain at any time in the past without having to re-calculate everything from the beginning. This will likely require very large amounts of disk storage and because it is not strictly necessary, conceptually blockchain data can be separated:

chain data (the list of blocks forming the chain)
state data (the result of each transaction's state transition)
While all chain data will be needed to ensure the cryptographic chain-of-custody and that nothing has been tampered with, old state data can be discarded (known as "pruning"). This is because state data is implicit data. That is, its value is known only from calculation, not from the actual information communicated. By contrast, chain data is explicit and stored as the block chain itself.

So currently, while both chain and state data are stored locally on the node's disk, only the chain data is strictly necessary. State data is can be ephemeral.

## 3. What is a replay attack ? , which 2 pieces of information can prevent it ?
At a high level, a replay attack in a blockchain context consists of taking advantage of a signed transaction at a blockchain level or smart contract level. This type of vulnerability was originally introduced at a blockchain level where malicious actors would take valid signed transactions on one chain and try to resubmit them on another blockchain for illicit intent. This type of attack occurs when blockchain clients (e.g., geth, nethermind) or smart contracts do not have proper security measures in place, which results in a valid transaction being used again for malicious purposes.

Nonce and ChainId can prevent it -->
Nonce
A nonce is a sequentially numbered value included when generating a signed transaction that keeps track of the number of transactions an address has sent. This counter of transactions (i.e., nonce) is recorded in the world state of Ethereum, which nodes hold state for.

Given that each transaction (i.e., confirmed transaction) has a unique nonce, this also results in a unique signature. This means a malicious actor can't take your valid signed transaction and try to broadcast it again (replaying) since it'll need to have a different nonce. 

Chain ID
A chain ID is used to specify which blockchain the transaction is meant for. This value isn't unique to each transaction but is unique to each blockchain (e.g., Ethereum, Optimism, Polygon). For example, the Ethereum mainnet uses 1, the Optimism mainnet uses 10, Polygon PoS uses 137, and so on. If a user tries to submit a transaction on a network the chain ID was not meant for, the node will not accept it. This is also a preventative measure where malicious actors cannot take signed transactions (already executed) and try to resubmit them on another chain.

This specific safeguard was introduced because of the Ethereum Classic hard fork, as EIP-155, which looked to prevent these replay attacks between Ethereum and the forked version, Ethereum Classic. There is also a network ID field, which isn't used with transaction verification but instead is used between node communication on a network level.

## 4. In a contract, how do we know who called a view function ?

See below for an example ,
If the caller is an EOA then msg.sender , if called inside another contract then the contract address is the caller.


```
pragma solidity 0.8.24;

contract ViewFuncTest {

    function getViewFuncCaller() public view returns(address) {
        return msg.sender;
    }

}

contract ViewFuncTestCaller {

ViewFuncTest public MyViewFuncTest;

    constructor(address ViewFuncTestAddress) {

        MyViewFuncTest = ViewFuncTest(ViewFuncTestAddress);

    }

    function getViewFuncCallerFromExtContract() public view {

        MyViewFuncTest.getViewFuncCaller();

    }

} 
```