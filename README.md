# Atomicals-js 文档中心
![systemArchitecture.png](_resources/systemArchitecture.png)

### 核心文档
- [核心设施](./atomical-js/CoreInfrastructure/核心设施.md):
包含底层工具和基础组件，如交易构建、脚本处理、签名验证等核心功能的实现。
- [钱包管理命令](./atomical-js/WalletCommands/钱包管理命令.md): 
创建和管理钱包，包括密钥生成、地址管理、余额查询和UTXO管理等功能。
- [铸造命令](./atomical-js/MintingCommands/铸造命令.md): 
支持多种数字资产的铸造，包括NFT、FT、Container、Realm、Subrealm等，每种资产类型都有其独特的铸造流程和规则。
- [转账命令](./atomical-js/TransferCommands/转账命令.md): 
处理各类数字资产的转移操作，支持单笔和批量转账，包含UTXO选择、交易构建和签名等完整流程。
- [查询命令](./atomical-js/QueryCommands/查询命令.md): 
提供丰富的查询接口，支持按地址、交易ID、资产ID等多维度查询，包括历史记录、状态和元数据等信息。
- [数据管理命令](./atomical-js/DataManagementCommands/数据管理命令.md): 
提供数据的创建、读取、更新和删除功能，支持多种数据格式，包括JSON、文本和二进制数据的处理。
- [领域管理命令](./atomical-js/RealmManagement/领域管理命令.md):
处理领域（Realm）相关的操作，包括领域创建、子领域管理、规则设置等功能。
- [交易广播命令](./atomical-js/TransactionBroadcasting/交易广播命令.md):
负责交易的广播、确认和状态查询，支持交易解码和验证功能。
- [交互操作命令](./atomical-js/InteractiveOperations/交互操作命令.md):
提供一系列交互式操作，如颜色定制、删除、事件发送、封存等高级功能。
- [实用工具命令](./atomical-js/UtilityCommands/实用工具命令.md):
包含下载、预览渲染、解析、服务器状态查询等辅助工具功能.

## 项目概述

Atomicals-js 是一个用于在比特币网络上创建和管理数字资产的 JavaScript/TypeScript 工具库。它提供了完整的命令行界面（CLI）和程序化 API，让开发者能够方便地操作 Atomicals 协议中的各类数字资产。

### 主要特性
- 支持多种数字资产类型管理
- 完整的钱包功能
- 安全的密钥管理
- 灵活的交易处理
- 丰富的查询接口

## 核心组件

### 1. 基础架构
项目采用模块化设计，主要包含以下核心目录：

```html
/lib
  ├── api/          // API 接口实现
  ├── commands/     // CLI 命令实现
  ├── utils/        // 工具函数
  ├── interfaces/   // TypeScript 接口定义
  ├── types/        // 类型定义
  ├── browser/      // 浏览器相关实现
  ├── cli.ts        // CLI 入口
  └── index.ts      // 库入口
```

```html2
/lib/commands/
├── Core Infrastructure                                     // 核心基础设施
│   ├── command.interface.ts                                // 命令接口定义
│   ├── command-result.interface.ts                         // 命令结果接口
│   ├── command-helpers.ts                                  // 命令辅助函数
│   └── witness_stack_to_script_witness.ts                  // 见证栈转换工具
│
├── Minting Operations                                     // 铸造操作
│   ├── DFT Minting                                        // DFT铸造
│   │   ├── init-interactive-dft-command.ts                // 初始化交互式DFT
│   │   ├── init-interactive-fixed-dft-command.ts          // 初始化固定供应DFT
│   │   ├── init-interactive-infinite-dft-command.ts       // 初始化无限供应DFT    
│   │   └── mint-interactive-dft-command.ts                // DFT铸造命令
│   ├── NFT & Container Minting                            // NFT和容器铸造
│   │   ├── mint-interactive-nft-command.ts                // NFT铸造
│   │   ├── mint-interactive-container-command.ts          // 容器铸造
│   │   ├── mint-interactive-dat-command.ts                // DAT铸造
│   │   └── mint-interactive-ditem-command.ts              // DItem铸造
│   ├── Realm Minting                                      // 域名铸造
│   │   ├── mint-interactive-realm-command.ts              // 域名铸造
│   │   ├── mint-interactive-subrealm-command.ts           // 子域名铸造
│   │   ├── mint-interactive-subrealm-direct-command.ts    // 直接子域名铸造
│   │   └── mint-interactive-subrealm-with-rules-command.ts   // 规则子域名铸造
│   └── FT Minting                                         // FT铸造
│       └── mint-interactive-ft-command.ts                 // FT铸造命令
│
├── Transfer Operations                                     // 转账操作
│   ├── transfer-interactive-builder-command.ts             // 转账构建器
│   ├── transfer-interactive-ft-command.ts                  // FT转账
│   ├── transfer-interactive-nft-command.ts                 // NFT转账
│   └── transfer-interactive-utxos-command.ts               // UTXO转账
│
├── Wallet & UTXO Management                                 // 钱包和UTXO管理
│   ├── Wallet Commands                                      // 钱包命令
│   │   ├── wallet-create-command.ts                         // 创建钱包
│   │   ├── wallet-import-command.ts                         // 导入钱包
│   │   ├── wallet-info-command.ts                           // 钱包信息
│   │   ├── wallet-init-command.ts                           // 初始化钱包
│   │   └── wallet-phrase-decode-command.ts                  // 助记词解码
│   └── UTXO Operations                                      // UTXO操作
│       ├── await-utxo-command.ts                            // 等待UTXO
│       ├── get-utxos.ts                                     // 获取UTXO
│       └── merge-interactive-utxos.ts                       // 合并UTXO
│
├── Query & Information                                      // 查询和信息
│   ├── Address Information                                  // 地址信息
│   │   ├── address-history-command.ts                       // 地址历史
│   │   ├── address-info-command.ts                          // 地址信息
│   │   └── get-atomicals-address-command.ts                 // 获取Atomicals地址
│   ├── Container Queries                                    // 容器查询
│   │   ├── get-container-item.ts                            // 获取容器项
│   │   ├── get-container-items-command.ts                   // 获取容器项列表
│   │   ├── get-container-item-validated-command.ts          // 验证容器项
│   │   └── get-container-item-validated-by-manifest-command.ts // 通过清单验证
│   ├── Search Commands                                      // 搜索命令
│   │   ├── search-containers-command.ts                     // 搜索容器
│   │   ├── search-realms-command.ts                         // 搜索域名
│   │   └── search-tickers-command.ts                        // 搜索代币符号
│   └── Summary Commands                                     // 摘要命令
│       ├── summary-containers-command.ts                    // 容器摘要
│       ├── summary-realms-command.ts                        // 域名摘要
│       ├── summary-subrealms-command.ts                     // 子域名摘要
│       └── summary-tickers-command.ts                       // 代币符号摘要
│
├── Transaction & Broadcasting                               // 交易和广播
│   ├── broadcast-command.ts                                 // 广播交易
│   ├── decode-tx-command.ts                                 // 解码交易
│   └── tx-command.ts                                        // 交易命令
│
├── Container Management                                     // 容器管理
│   ├── set-container-data-interactive-command.ts            // 设置容器数据
│   ├── set-container-dmint-interactive-command.ts           // 设置容器DMint
│   └── get-by-container-command.ts                          // 获取容器信息
│
├── Realm Management                                         // 域名管理
│   ├── enable-subrealm-rules-command.ts                     // 启用子域名规则
│   ├── disable-subrealm-rules-command.ts                    // 禁用子域名规则
│   ├── get-subrealm-info-command.ts                         // 获取子域名信息
│   ├── pending-subrealms-command.ts                         // 待处理子域名
│   └── get-by-realm-command.ts                              // 获取域名信息
│
├── DMint Operations                                         // DMint操作
│   ├── create-dmint-command.ts                              // 创建DMint
│   └── create-dmint-manifest-command.ts                     // 创建DMint清单
│
├── Interactive Operations                                   // 交互式操作
│   ├── custom-color-interactive-command.ts                  // 自定义颜色
│   ├── delete-interactive-command.ts                        // 删除操作
│   ├── emit-interactive-command.ts                          // 发射操作
│   ├── seal-interactive-command.ts                          // 封装操作
│   ├── set-interactive-command.ts                           // 设置操作
│   ├── set-relation-interactive-command.ts                  // 设置关系
│   ├── splat-interactive-command.ts                         // 分散操作
│   └── split-interactive-command.ts                         // 分割操作
│
└── Utility Commands                                         // 工具命令
    ├── download-command.ts                                  // 下载命令
    ├── render-previews-command.ts                           // 渲染预览
    ├── resolve-command.ts                                   // 解析命令
    ├── server-version-command.ts                            // 服务器版本
    ├── get-global-command.ts                                // 获取全局信息
    └── list-command.ts                                      // 列表命令
```

### 2. 关键依赖
核心依赖包含：

```json
{
  "dependencies": {
    "bitcoinjs-lib": "^6.1.0",    // 比特币操作库
    "bip32": "^3.1.0",            // 密钥派生
    "bip39": "^3.1.0",            // 助记词管理
    "electrum-client": "^1.1.0",   // ElectrumX 客户端
    "commander": "^9.4.1"          // CLI 框架
  }
}
```

## 开发环境配置

### 1. 环境准备
首先需要配置开发环境：

```bash
# 安装依赖
yarn install

# 配置环境变量
cp .env.example .env

# 编译项目
yarn build
```

### 2. 钱包配置
创建并配置钱包：

```typescript
// 初始化钱包
const initWallet = async () => {
  const mnemonic = generateMnemonic();  // 生成助记词
  const wallet = await Wallet.fromMnemonic(mnemonic);
  
  console.log("钱包地址:", wallet.address);
  console.log("请妥善保管助记词:", mnemonic);
};
```

## 常用命令指南

### 1. 钱包管理命令 (Wallet Management)
钱包是您与比特币网络交互的基础。就像您的银行账户一样，它存储了您的数字资产。

```bash
# 创建新钱包（就像开设新的银行账户）
yarn cli wallet-create

# 初始化钱包配置文件
yarn cli wallet-init

# 导入已有钱包（类似于在新设备上登录您的账户）
yarn cli wallet-import <私钥> <别名>

# 查看钱包余额
yarn cli balances
```

### 2. 地址操作命令 (Address Operations)
地址就像您的银行账号，用于接收和发送数字资产。

```bash
# 查询地址信息（就像查看账户详情）
yarn cli address <地址>

# 查看地址的所有UTXO（未花费的交易输出，类似于账户中的零钱）
yarn cli address-utxos <地址>

# 查看地址历史（类似于查看账单记录）
yarn cli address-history <地址>
```

### 3. 资产管理命令 (Asset Management)

#### NFT 操作
非同质化代币(NFT)就像独一无二的艺术品或收藏品。

```bash
# 创建新的NFT（类似于创作一件艺术品）
yarn cli mint-nft <配置文件>

# 转移NFT到新地址（类似于将艺术品送给他人）
yarn cli transfer-nft <NFT编号> <接收地址>
```

#### FT（同质化代币）操作
同质化代币类似于货币，每个单位都是相同的。

```bash
# 创建新代币（类似于发行新的货币）
yarn cli mint-ft <代币符号> <供应量> <配置文件>

# 转移代币（类似于转账）
yarn cli transfer-ft <代币编号>
```

### 4. 域名系统命令 (Realm System)
域名系统类似于互联网的域名，提供易记的名称。

```bash
# 注册顶级域名（类似于注册.com域名）
yarn cli mint-realm <域名>

# 注册子域名（类似于注册子域名，如 blog.example.com）
yarn cli mint-subrealm <子域名>
```

### 5. 容器操作命令 (Container Operations)
容器就像一个数字化的收藏夹，可以存储多个相关的数字资产。

```bash
# 创建新容器（类似于创建新的收藏夹）
yarn cli mint-container <容器名称>

# 查看容器内容（类似于浏览收藏夹）
yarn cli get-container <容器名称>
```

### 实用技巧

1. **命令帮助**
```bash
# 查看任何命令的详细帮助
yarn cli help <命令名称>
```

2. **常见操作流程**

初学者建议按以下顺序学习：

a) 钱包管理
```bash
# 第一步：创建钱包
yarn cli wallet-create

# 第二步：查看余额
yarn cli balances
```

b) 创建第一个NFT
```bash
# 准备NFT配置文件 config.json
{
  "name": "我的第一个NFT",
  "description": "这是一个测试NFT",
  "image": "ipfs://..."
}

# 铸造NFT
yarn cli mint-nft config.json
```