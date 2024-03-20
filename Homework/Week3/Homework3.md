
## 1. What are the advantages and disadvantages of
## the 256 bit word length in the EVM?
Advantages : this facilitates Keccak256
hash scheme and elliptic-curve computations.
Disadvantages : 

## 2. What would happen if the implementation of a
## precompiled contract varied between Ethereum
## clients ?
It will produce a problem on the state that will be different and then consensus will not work.
The different clients would not come to consensus and you get end up with forks of the network.

## 3. Using Remix write a simple contract that uses a
## memory variable, then using the debugger step
## through the function and inspect the memory.


