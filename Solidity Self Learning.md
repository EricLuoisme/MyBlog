# Solidity



## Array

```js
pragma solidity 0.5.1;

contract MyContract {

    Person[] public people;
    
    uint256 public peopleCount;
    
    struct Person{
        string _firstName;
        string _lastName;
    }
    
    function addPerson(string memory _firstName, string memory _lastName) public {
        people.push(Person(_firstName, _lastName));
        peopleCount += 1;
    }
    
}

```

Array使用push进行插入。



## Mapping

```js
pragma solidity 0.5.1;

contract MyContract {

    mapping(uint256 => Person) public people;
    
    uint256 public peopleCount;
    
    struct Person{
        uint256 id;
        string _firstName;
        string _lastName;
    }
    
    function addPerson(string memory _firstName, string memory _lastName) public {
        peopleCount += 1;
        people[peopleCount] = Person(peopleCount, _firstName, _lastName);
    }
}
```

Mapping跟Array不同点在于，Array是从0开始的，而Mapping则由你的key开始。所以Array如果输入没有的index得到的是异常，而Mapping则返回的是一个**默认值**。