
## 1. Write a function that will delete items (one at atime) from a dynamic array without leaving
## gaps in the array. You should assume that the
## items to be deleted are chosen at random, and
## try to do this in a gas efficient manner.
## For example imagine your array has 12 items
## and you need to delete the items at indexes 8,
## 2 and 7.
## The final array will then have items
## {0,1,3,4,5,6,9,10,11}

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.24;

contract Homework2 {
    uint256[] public arrayDelItems = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];

    function deleteItem(uint256 _index) public {
        require(arrayDelItems.length > 0, "Array is Empty");
        arrayDelItems[_index] = arrayDelItems[arrayDelItems.length - 1];
        arrayDelItems.pop();
    }
}
```