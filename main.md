// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract c_funding {
    
    mapping (address => uint) investor;

    address owner = msg.sender;

    uint minimum_amount = 1 ether;

    uint goal = 100 ether;

    uint time = block.timestamp + 5 minutes;

    function invest() public payable {
     
     require(time >= block.timestamp,"time over");

     require(msg.value >= minimum_amount,"minimum 1 ether require");

     uint balance = address(this).balance;

     balance += msg.value;

     investor[msg.sender] = msg.value;
     
    }

   function Refund() public payable {
     uint balance = address(this).balance;
     require(time < block.timestamp,"wait for time end");
     require(balance != goal,"goal is rich");
     require(investor[msg.sender] > 0,"you never participate");
     
     balance -= investor[msg.sender];

     payable (msg.sender).transfer(investor[msg.sender]);

     investor[msg.sender] = 0;
   }
}
