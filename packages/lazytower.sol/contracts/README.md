<p align="center">
    <h1 align="center">
         LazyTower (Solidity)
    </h1>
    <p align="center">LazyTower Solidity libraries.</p>
</p>

<p align="center">
    <a href="https://github.com/privacy-scaling-explorations/zk-kit">
        <img src="https://img.shields.io/badge/project-zk--kit-blue.svg?style=flat-square">
    </a>
    <a href="https://github.com/privacy-scaling-explorations/zk-kit/blob/main/LICENSE">
        <img alt="Github license" src="https://img.shields.io/github/license/privacy-scaling-explorations/zk-kit.svg?style=flat-square">
    </a>
    <a href="https://www.npmjs.com/package/@zk-kit/lazytower.sol">
        <img alt="NPM version" src="https://img.shields.io/npm/v/@zk-kit/lazytower.sol?style=flat-square" />
    </a>
    <a href="https://npmjs.org/package/@zk-kit/lazytower.sol">
        <img alt="Downloads" src="https://img.shields.io/npm/dm/@zk-kit/lazytower.sol.svg?style=flat-square" />
    </a>
    <a href="https://eslint.org/">
        <img alt="Linter eslint" src="https://img.shields.io/badge/linter-eslint-8080f2?style=flat-square&logo=eslint" />
    </a>
    <a href="https://prettier.io/">
        <img alt="Code style prettier" src="https://img.shields.io/badge/code%20style-prettier-f8bc45?style=flat-square&logo=prettier" />
    </a>
</p>

<div align="center">
    <h4>
        <a href="https://appliedzkp.org/discord">
            🗣️ Chat &amp; Support
        </a>
    </h4>
</div>

## Libraries:

✔️ [LazyTower](https://github.com/privacy-scaling-explorations/zk-kit/blob/main/packages/lazytower.sol/contracts/LazyTower.sol) (Poseidon)

---

## 🛠 Install

### npm or yarn

Install the `@zk-kit/lazytower.sol` package with npm:

```bash
npm i @zk-kit/lazytower.sol --save
```

or yarn:

```bash
yarn add @zk-kit/lazytower.sol
```

## 📜 Usage

### Importing and using the library

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.4;

import "../LazyTower.sol";

contract LazyTowerTest {
    using LazyTower for LazyTowerData;

    // LazyTower may emit multiple events in a singal add() call
    event Add(uint8 indexed level, uint64 indexed lvFullIndex, uint256 value);

    // map for multiple test cases
    mapping(bytes32 => LazyTowerData) public towers;

    function add(bytes32 _towerId, uint256 _item) external {
        towers[_towerId].add(_item);
    }

    function getDataForProving(bytes32 _towerId)
        external
        view
        returns (
            uint256,
            uint256[] memory,
            uint256
        )
    {
        return towers[_towerId].getDataForProving();
    }
}

```

### Creating an Hardhat task to deploy the contract

```typescript
import { Contract } from "ethers"
import { task, types } from "hardhat/config"

task("deploy:ht-test", "Deploy a LazyTowerTest contract")
    .addOptionalParam<boolean>("logs", "Print the logs", true, types.boolean)
    .setAction(async ({ logs }, { ethers }): Promise<Contract> => {
        const PoseidonT3Factory = await ethers.getContractFactory("PoseidonT3")
        const PoseidonT3 = await PoseidonT3Factory.deploy()

        if (logs) {
            console.info(`PoseidonT3 library has been deployed to: ${PoseidonT3.address}`)
        }

        const LazyTowerLibFactory = await ethers.getContractFactory("LazyTower", {
            libraries: {
                PoseidonT3: PoseidonT3.address
            }
        })
        const lazyTowerLib = await LazyTowerLibFactory.deploy()

        await lazyTowerLib.deployed()

        if (logs) {
            console.info(`LazyTower library has been deployed to: ${lazyTowerLib.address}`)
        }

        const ContractFactory = await ethers.getContractFactory("LazyTowerTest", {
            libraries: {
                LazyTower: lazyTowerLib.address
            }
        })

        const contract = await ContractFactory.deploy()

        await contract.deployed()

        if (logs) {
            console.info(`Test contract has been deployed to: ${contract.address}`)
        }

        return contract
    })
```

## Contacts

### Developers

-   e-mail : lcamel@gmail.com
-   github : [@LCamel](https://github.com/LCamel)
-   website : https://twitter.com/LCamel