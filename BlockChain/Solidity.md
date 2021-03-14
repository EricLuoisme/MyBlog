# Solidity



## Smart Contract

Look likes a "CLASS", but it's public accessable. 

The variable inside the Smart Contract, is more like a "Gobal Variable", which will be **recorded** in the BlockChain.



## Owner

```js
pragma solidity 0.5.1;

contract MyContract {
    uint256 public peopleCount = 0;
    mapping(uint => Person) public people;
    
    address owner;
    
    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
    
    constructor() public {
        owner = msg.sender;
    }
    
    struct Person {
        uint _id;
        string _firstName;
        string _lastName;
    }
    
    function addPerson(string memory _firstName, string memory _lastName) public onlyOwner {
        incrementCount();
        people[peopleCount] = Person(peopleCount, _firstName, _lastName);
    }
    
    function incrementCount() internal {
        peopleCount += 1;
    }
}
```

通过设置public后的关键字，可以限制只允许特定人群才能调用该function



## 延迟Sell

```js

```

