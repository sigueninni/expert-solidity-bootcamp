

# Assembly 1

## 1. Look at the example of init code in today's notes
## See gist --> https://gist.github.com/extropyCoder/4243c0f90e6a6e97006a31f5b9265b94
## When we do the CODECOPY operation, what are we overwriting ? (debugging this in Remix might help here)

We are overwriting init code.

## 2. Could the answer to Q1 allow an optimisation ?
Yes, not keeping init code is an optimisation.

## 3. Can you trigger a revert in the init code in Remix ?
Yes, if gas sent with Contract creation is not enough.

## 4. Write some Yul to
##    1. Add 0x07 to 0x08
##    2. store the result at the next free memory location.

```
assembly{
          let  freeMemoryPointer := mload(0x40)
          let  x := add(0x07, 0x08) 
          sstore(freeMemoryPointer, x )
        }
```

##    3. (optional) write this again in opcodes

```
PUSH1 0X07
PUSH1 0X08
ADD
PUSH1 0X40
SSTORE
```

##    5. Can you think of a situation where the opcode EXTCODECOPY is used ?


The EXTCODECOPY opcode in Ethereum has several potential use cases, particularly within smart contracts. Here are some of the primary usages:

Verification and Analysis: Smart contracts can use EXTCODECOPY to verify or analyze the bytecode of other contracts on the Ethereum blockchain. For instance, a contract might check the bytecode of another contract to ensure it conforms to certain standards or interfaces before interacting with it.

Upgradeable Contracts: In upgradeable contract patterns, such as proxy contracts, EXTCODECOPY can be used to copy the bytecode of a new version of the contract into the storage of the proxy contract. This allows for seamless upgrades while maintaining the proxy's logic intact.

Contract Initialization: During contract deployment or initialization processes, contracts might use EXTCODECOPY to read bytecode from a specified address and initialize their state based on that bytecode. This can be useful for contracts that need to interact with other contracts dynamically.

Contract Decompilation: Developers might use EXTCODECOPY for reverse engineering or decompilation purposes, where they extract the bytecode of contracts deployed on the Ethereum blockchain to study their functionality or security properties.

Dynamic Contract Loading: Smart contracts can use EXTCODECOPY to dynamically load and interact with other contracts at runtime based on certain conditions or parameters, allowing for more flexible and adaptable contract designs.


##    6. Complete the assembly exercises in this repo Exercises --> https://github.com/ExtropyIO/ExpertSolidityBootcamp


