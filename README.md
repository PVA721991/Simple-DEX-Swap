# Simple-DEX-Swap
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract SimpleDEX {
    uint256 public reserveTokenA;
    uint256 public reserveTokenB;

    uint256 public constant FEE = 30; // 0.3%

    event Swap(address indexed user, uint256 amountIn, uint256 amountOut);

    function addLiquidity(uint256 amountA, uint256 amountB) public payable {
        reserveTokenA += amountA;
        reserveTokenB += amountB;
    }

    function swapAforB(uint256 amountAIn) public payable returns (uint256) {
        uint256 amountInWithFee = amountAIn * (1000 - FEE) / 1000;
        uint256 amountBOut = (amountInWithFee * reserveTokenB) / (reserveTokenA + amountAIn);

        reserveTokenA += amountAIn;
        reserveTokenB -= amountBOut;

        payable(msg.sender).transfer(amountBOut);
        emit Swap(msg.sender, amountAIn, amountBOut);
        return amountBOut;
    }
}
