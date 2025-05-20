---
title: Blockchain-1
date: 2025-01-07 10:05:37
tags:
---
### 概述
本章内容为Web3.0技术栈。

### **Web3.0 分层说明**

#### **1. 基础设施层（Infrastructure Layer）**
- **区块链网络**  
  - 公链（以太坊、Solana、Polkadot）、联盟链（Hyperledger）  
  - 共识机制（PoW/PoS/DPoS）、分片技术  
- **分布式存储**  
  - IPFS、Filecoin、Arweave（文件存储）  
  - Ceramic（动态数据库）  
- **分布式计算**  
  - Golem、iExec（算力共享）  
  - 边缘计算节点（结合 IoT）  
- **节点网络**  
  - P2P 通信协议（Libp2p、Devp2p）  
  - 节点服务商（Infura、Alchemy）  


#### **2. 协议层（Protocol Layer）**
- **智能合约平台**  
  - 以太坊虚拟机（EVM）、Solana VM、Move VM（Aptos/Sui）  
- **跨链协议**  
  - Cosmos IBC、Polkadot XCMP、LayerZero（跨链桥）  
- **身份与隐私协议**  
  - DID（去中心化身份）、零知识证明（zk-SNARKs）、安全多方计算（MPC）  
- **中间件服务**  
  - Chainlink（预言机）、The Graph（数据索引）、ENS（域名解析）  


#### **3. 扩展与工具层（Scaling & Tooling Layer）**
- **Layer2 扩展方案**  
  - Rollups（Optimistic/ZK）、侧链（Polygon）、状态通道  
- **开发工具**  
  - 智能合约框架（Hardhat、Foundry）、SDK（Web3.js、Ethers.js）  
  - 合约库（OpenZeppelin）  
- **钱包与认证**  
  - MetaMask、WalletConnect、账户抽象（ERC-4337）  


#### **4. 应用层（Application Layer）**
- **DeFi（去中心化金融）**  
  - DEX（Uniswap）、借贷（Aave）、稳定币（DAI）  
- **NFT 与数字资产**  
  - 艺术品（Art Blocks）、虚拟地产（Decentraland）  
- **GameFi 与元宇宙**  
  - Axie Infinity、The Sandbox  
- **SocialFi 与 DAO**  
  - Lens Protocol、Aragon（DAO 治理）  
- **去中心化科学（DeSci）**  
  - 科研数据共享、分布式协作  


#### **5. 用户交互层（User Interaction Layer）**
- **前端界面**  
  - DApp 网页端、移动端（Status、Phantom 钱包）  
- **用户体验优化**  
  - 账户抽象（无 Gas 交易）、社交恢复钱包  
- **链上-链下交互**  
  - 法币入口（MoonPay）、NFT 市场（OpenSea）  


### **架构图示例（文字版）**
```
┌───────────────────────────────┐  
│        用户交互层             │  
│  DApp前端 | 钱包 | NFT市场    │  
└──────────────┬────────────────┘  
               ↓  
┌───────────────────────────────┐  
│          应用层               │  
│ DeFi | NFT | GameFi | DAO     │  
└──────────────┬────────────────┘  
               ↓  
┌───────────────────────────────┐  
│      扩展与工具层              │  
│ Layer2 | 开发工具 | 钱包SDK    │  
└──────────────┬────────────────┘  
               ↓  
┌───────────────────────────────┐  
│          协议层               │  
│ 智能合约 | 跨链 | 身份协议      │  
└──────────────┬────────────────┘  
               ↓  
┌───────────────────────────────┐  
│        基础设施层              │  
│ 区块链 | 存储 | 节点网络        │  
└───────────────────────────────┘  
```

### **关键交互关系**
1. **用户层**通过前端界面与 **应用层**（如 DeFi、NFT）交互。  
2. **应用层**依赖 **协议层**（智能合约、跨链协议）实现逻辑。  
3. **扩展层**优化性能（Layer2）并提供开发工具。  
4. **基础设施层**提供底层存储、计算和网络支持。  
