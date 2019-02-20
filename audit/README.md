# Sparrowex SPO token Audit Report


Prepared by:

- Bruce Li, [bruce@blockchainlabs.nz](bruce@blockchainlabs.nz)

Report:

- February 22, 2019 – date of delivery

<br>

### Table of Contents

- [Preamble](#preamble)
- [Scope](#scope)
- [Tests](#tests)
- [Issues](#issues-found):
	- [Minor](#minor)
	- [Moderate](#moderate)
	- [Major](#major)
	- [Critical](#critical)
- [Observations](#observations)
- [Conclusion](#conclusion)



<div class="page-break"></div><!-- ******************************************************** -->

## Preamble

This audit report was undertaken by **BlockchainLabs.nz** for the purpose of providing feedback to **Sparrowex**. It has subsequently been shared publicly without any express or implied warranty.

We took part in carefully inspecting the source code and tests for the **Sparrowex SPO**. We set out to establish that _the code had no known security vulnerabilities_.



<br><!-- ******************************************************** -->

## Scope

The following sources were in scope for static analysis:

#### Smart contracts

  - [SPO.sol](https://github.com/BlockchainLabsNZ/Sparrowex-spo-audit/blob/master/contracts/SPO.sol)


<br><!-- ******************************************************** -->


## Tests

#### Dynamic test

 - Test script : [SPO.test.js](https://github.com/BlockchainLabsNZ/Sparrowex-spo-audit/blob/master/test/contracts/SPO.test.js)
 - [x] The dynamic test is passed.

#### Deploy test
- The deployment is performed on kovan testnet through [infura](https://infura.io/) using truffle migration
- The code has been verified on kovan. See here [SPO.sol](https://kovan.etherscan.io/address/0x53770abcb1d89805643da591d980b088bbb7a96d#code)

<br>

### Out of scope

- [openzeppelin-solidity library](https://github.com/OpenZeppelin/openzeppelin-solidity) – *standard library, previously extensevly audited*

<br><!-- ******************************************************** -->

## Focus Areas

Testing was focused on the following key areas - though this is not an exhaustive list.

- ### Correctness

	- No correctness defects uncovered during static analysis?
	- No implemented contract violations uncovered during execution?
	- No other generic incorrect behaviour detected during execution?
	- Adherence to adopted standards such as ERC20?

- ### Testability

	- Test coverage across all functions and events?
	- Test cases for both expected behaviour and failure modes?
	- Settings for easy testing of a range of parameters?
	- No reliance on nested callback functions or console logs?
	- Avoidance of test scenarios calling other test scenarios?

- ### Security

	- No presence of known security weaknesses?
	- No funds at risk of malicious attempts to withdraw/transfer?
	- No funds at risk of control fraud?
	- Prevention of Integer Overflow or Underflow?

- ### Best Practice

	- Explicit labelling for the visibility of functions and state variables?
	- Proper management of gas limits and nested execution?
	- Latest version of the Solidity compiler?

<br><!-- *********************************************** -->

## Issues

<h4>Severity Description:</h4>

<table>
  <tr>
    <td><strong>Minor</strong></td>
    <td>A defect that does not have a material impact on the contract execution and is likely to be subjective.</td>
  </tr>
  <tr>
    <td><strong>Moderate</strong></td>
    <td>A defect that could impact the desired outcome of the contract execution in a specific scenario.</td>
  </tr>
  <tr>
    <td><strong>Major</strong></td>
    <td> A defect that impacts the desired outcome of the contract execution or introduces a weakness that may be exploited.</td>
  </tr>
  <tr>
    <td><strong>Critical</strong></td>
    <td>A defect that presents a significant security vulnerability or failure of the contract across a range of scenarios.</td>
  </tr>
</table>


### Minor

- **Avoid magic number** – `Best practice` [Issue #1](https://github.com/BlockchainLabsNZ/Sparrowex-spo-audit/issues/1)<br>
```javascript
13    uint256 public constant TOTAL_SUPPLY = 1400000000 * (10 ** 18); // 1.4 billion SPO
......
20    _mint(0xCF44ccB8f27aEA1abb054d4Ae89AA00CFbda3603, 376000000 * (10 ** 18)); // ICO
21    _mint(0xEd6D7d1Ffb258bB82A6B5Cf2C2377DCAA6A0b533, 57000000 * (10 ** 18)); // Bonus
22    _mint(0x1fcE49D845036a54E348fe10Aa6f541DEb48547a, 70000000 * (10 ** 18)); // Advisors
23    _mint(0xEf235b3CaAA268C4Db6215879a436Af751A0F729, 140000000 * (10 ** 18)); // Founders
24    _mint(0x49035f3a38644d805C8AEdA5eB5fAA5DB54A5fA0, 70000000 * (10 ** 18)); // Employees
25    _mint(0xdaBF505B241695788780b12515066746fDD65592, 337000000 * (10 ** 18)); // Reserves Pool
26    _mint(0xEf13476b380C31CA6B1FB5C49d5CfCedcdB90b69, 140000000 * (10 ** 18)); // Marketing
27    _mint(0xe998a139D865883C8964d34048a9acb75Eb314Ba, 210000000 * (10 ** 18)); // Strategic Partnerships
```
We recommend avoiding the use of magic numbers, using a variable here would improve readability and make the code more maintainable for the future.
e.g1 add `uint256 public constant ONE_TOKEN = 10 ** 18;` and then replace all the `10 ** 18` with `ONE_TOKEN`
e.g2 add `uint256 public constant ONE_MILLION = 10 ** 24;` and then carefully replace those numbers with new numbers (for instance, `1400000000 * 10 ** 18` becomes `1400 * ONE_MILLION`)
<br><br>

### Moderate

- None found

### Major

- None found

### Critical

- None found

<br>

## Observations

### Favor auto deployment over manual
It is better to deploy the contract using script or truffle migration. That makes deployment easier and less prone to mistake.


<br><!-- *********************************************** -->

## Conclusion

The code of smart contract is concise and solid, not exhibitting any know security vulnerabilities. A well-known and well-audited solidity library is imported and used in the smart contract, which should increase confidence in the security of the contract.

<br><!-- *********************************************** -->
<hr>
<h4>Disclaimer</h4>

Our team uses our current understanding of the best practises for Solidity and Smart Contracts. Development in Solidity and for Blockchains is an emerging area of software engineering which still has a lot of room to grow, hence our current understanding of best practices may not find all of the issues in this code and design. We have not analysed any of the assembly code generated by the Solidity compiler. We have not verified the deployment process and configurations of the contracts. We have only analysed the code outlined in the scope. We have not verified any of the claims made by any of the organisations behind this code. Security audits do not warrant bug-free code. We encourage all users interacting with smart contract code to continue to analyse and inform themselves of any risks before interacting with any smart contracts.
