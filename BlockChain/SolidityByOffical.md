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

<img src="E:\MyBlog\BlockChain\image-20210314202141024.png" alt="image-20210314202141024"  />







