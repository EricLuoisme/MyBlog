# Solidity Self-learning By Offical Documents



## Intro to smart contract



### Storage Example

```java
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

pragma类似C++中告知需要使用的编译器，上面的代码是一个简单的存储。



### Subcurrency Example

The following contract implements the simplest form of a cryptocurrency. The contract allows only its creator to create new coins (different issuance schemes are possible). Anyone can send coins to each other without a need for registering with a username and password, all you need is an Ethereum keypair.

<img src=".\image-20210314202141024.png" alt="image-20210314202141024"  />

As we can see above, the **event** keyword means an event that would **be published / broadcast** once we activate it with **emit** keyword.

> If you use this contract to send coins to an address, you will not see anything when you look at that address on a blockchain explorer, because the record that you sent coins and the changed balances are only stored in the data storage of this particular coin contract. By using events, you can create a “blockchain explorer” that tracks transactions and balances of your new coin, but you have to inspect the coin contract address and not the addresses of the coin owners.



## Blockchain Basics

These blocks form a linear sequence in time and that is where the word “blockchain” derives from. Blocks are added to the chain in rather regular intervals - for Ethereum this is roughly every **17 seconds**.



## The Ethereum Vitrual Machine

The Ethereum Virtual Machine or EVM is the **runtime environment for smart contracts in Ethereum**. It is not only sandboxed but actually completely isolated, which means that code running inside the EVM has no access to network, filesystem or other processes. Smart contracts even have limited access to other smart contracts.

### Account

There are two kinds of accounts in Ethereum, which **share the same address space**.

- **External accounts**: controlled by **public-private key pairs** accounts.
- **Contract accounts**: controlled by **the code** stored together with the account.

The address of External account is determined from the public key while **the address of a contract** is determined at the time the contract is created. (it is derived from creator's address & the number of transactions sent from that address, which so-called "nonce").

- Every account has a persistent key-value store mapping 256-bit words to 256-bit words called **storage**.

- Furthermore, every account has a **balance** in Ether (in “Wei” to be exact, `1 ether` is `10**18 wei`) which can be modified by sending transactions that include Ether.

### Transaction

A transaction is a message that is sent from one account to another account (which might be the same or empty), it can include binary data (which is called "payload") and Ether.

**If the target account contains code, that code is executed and the payload is provided as input data**. (如果TXN的Target账户是带有代码的，这个TXN将会调用这个代码来执行，相当于call this contract)

If the target account is not set (the transaction does not have a recipient, or recipient is set to NULL), the transaction creates a **new contract**. (如果TXN的Target账户为空，将会被认为是新建一个合约)

The payload of such a contract creation of transaction is taken to be EVM bytecode and executed. The output data of this execution is permanently stored as the code of the contract. This means that in order to create a contract, you do not send the actual code of the contract, but in fact code that returns that code when executed. （此类合同创建事务的payload被视为EVM字节码并执行。该执行的输出数据被永久存储为合同的代码。这意味着为了创建合同，您不发送合同的实际代码，而是发送在执行时返回该代码的字节码。）

While a contract is being created, its code is still empty. Because of that, you should not call back into the contract under construction until its constructor has finished executing.

### Gas

The **gas price** is a value set by the creator of the transaction, who has to pay **gas_price ${\times}$  gas** up front from the sending account. If some gas is left after the execution, it is refunded to the creator in the same way. 

### Storage, Memory and Stack

- **storage**: each account has this data area, which is persistence between function calls and transactions. It is comparatively costly to read, and even more to initialise and modify storage.
- **momory**: of which a contract a freshly cleared instance for each message call.
- **stack**: all computations are performed on this data area. 

### Message calls

Contracts can **call other contracts** or **send Ether to non-contract accounts** by the means of **message calls**. Which can be seen as a transaction. 



## Solidity by Example

### Voting









