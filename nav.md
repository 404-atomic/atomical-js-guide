# 基础架构
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

- [commands](#commands)
- [utils](#utils)

## commands


```html2
/lib/commands/
├── Core Infrastructure #核心基础设施
│   ├── command.interface.ts                                // 命令接口定义
│   ├── command-result.interface.ts                         // 命令结果接口
│   ├── command-helpers.ts                                  // 命令辅助函数
│   └── witness_stack_to_script_witness.ts                  // 见证栈转换工具
│
├── Minting Operations #铸造操作
│   ├── DFT Minting #DFT铸造
│   │   ├── init-interactive-dft-command.ts                // 初始化交互式DFT
│   │   ├── init-interactive-fixed-dft-command.ts          // 初始化固定供应DFT
│   │   ├── init-interactive-infinite-dft-command.ts       // 初始化无限供应DFT    
│   │   └── mint-interactive-dft-command.ts                // DFT铸造命令
│   ├── NFT & Container Minting #NFT和容器铸造
│   │   ├── mint-interactive-nft-command.ts                // NFT铸造
│   │   ├── mint-interactive-container-command.ts          // 容器铸造
│   │   ├── mint-interactive-dat-command.ts                // DAT铸造
│   │   └── mint-interactive-ditem-command.ts              // DItem铸造
│   ├── Realm Minting #域名铸造
│   │   ├── mint-interactive-realm-command.ts              // 域名铸造
│   │   ├── mint-interactive-subrealm-command.ts           // 子域名铸造
│   │   ├── mint-interactive-subrealm-direct-command.ts    // 直接子域名铸造
│   │   └── mint-interactive-subrealm-with-rules-command.ts   // 规则子域名铸造
│   └── FT Minting #FT铸造
│       └── mint-interactive-ft-command.ts                 // FT铸造命令
│
├── Transfer Operations #转账操作
│   ├── transfer-interactive-builder-command.ts             // 转账构建器
│   ├── transfer-interactive-ft-command.ts                  // FT转账
│   ├── transfer-interactive-nft-command.ts                 // NFT转账
│   └── transfer-interactive-utxos-command.ts               // UTXO转账
│
├── Wallet & UTXO Management #理钱包和UTXO管
│   ├── Wallet Commands #钱包命令
│   │   ├── wallet-create-command.ts                         // 创建钱包
│   │   ├── wallet-import-command.ts                         // 导入钱包
│   │   ├── wallet-info-command.ts                           // 钱包信息
│   │   ├── wallet-init-command.ts                           // 初始化钱包
│   │   └── wallet-phrase-decode-command.ts                  // 助记词解码
│   └── UTXO Operations #UTXO操作
│       ├── await-utxo-command.ts                            // 等待UTXO
│       ├── get-utxos.ts                                     // 获取UTXO
│       └── merge-interactive-utxos.ts                       // 合并UTXO
│
├── Query & Information #查询和信息
│   ├── Address Information #地址信息
│   │   ├── address-history-command.ts                       // 地址历史
│   │   ├── address-info-command.ts                          // 地址信息
│   │   └── get-atomicals-address-command.ts                 // 获取Atomicals地址
│   ├── Container Queries #容器查询
│   │   ├── get-container-item.ts                            // 获取容器项
│   │   ├── get-container-items-command.ts                   // 获取容器项列表
│   │   ├── get-container-item-validated-command.ts          // 验证容器项
│   │   └── get-container-item-validated-by-manifest-command.ts // 通过清单验证
│   ├── Search Commands #搜索命令
│   │   ├── search-containers-command.ts                     // 搜索容器
│   │   ├── search-realms-command.ts                         // 搜索域名
│   │   └── search-tickers-command.ts                        // 搜索代币符号
│   └── Summary Commands #摘要命令
│       ├── summary-containers-command.ts                    // 容器摘要
│       ├── summary-realms-command.ts                        // 域名摘要
│       ├── summary-subrealms-command.ts                     // 子域名摘要
│       └── summary-tickers-command.ts                       // 代币符号摘要
│
├── Transaction & Broadcasting #交易和广播
│   ├── broadcast-command.ts                                 // 广播交易
│   ├── decode-tx-command.ts                                 // 解码交易
│   └── tx-command.ts                                        // 交易命令
│
├── Container Management #容器管理
│   ├── set-container-data-interactive-command.ts            // 设置容器数据
│   ├── set-container-dmint-interactive-command.ts           // 设置容器DMint
│   └── get-by-container-command.ts                          // 获取容器信息
│
├── Realm Management #域名管理
│   ├── enable-subrealm-rules-command.ts                     // 启用子域名规则
│   ├── disable-subrealm-rules-command.ts                    // 禁用子域名规则
│   ├── get-subrealm-info-command.ts                         // 获取子域名信息
│   ├── pending-subrealms-command.ts                         // 待处理子域名
│   └── get-by-realm-command.ts                              // 获取域名信息
│
├── DMint Operations #DMint操作
│   ├── create-dmint-command.ts                              // 创建DMint
│   └── create-dmint-manifest-command.ts                     // 创建DMint清单
│
├── Interactive Operations #交互式操作
│   ├── custom-color-interactive-command.ts                  // 自定义颜色
│   ├── delete-interactive-command.ts                        // 删除操作
│   ├── emit-interactive-command.ts                          // 发射操作
│   ├── seal-interactive-command.ts                          // 封装操作
│   ├── set-interactive-command.ts                           // 设置操作
│   ├── set-relation-interactive-command.ts                  // 设置关系
│   ├── splat-interactive-command.ts                         // 分散操作
│   └── split-interactive-command.ts                         // 分割操作
│
└── Utility Commands #工具命令
    ├── download-command.ts                                  // 下载命令
    ├── render-previews-command.ts                           // 渲染预览
    ├── resolve-command.ts                                   // 解析命令
    ├── server-version-command.ts                            // 服务器版本
    ├── get-global-command.ts                                // 获取全局信息
    └── list-command.ts                                      // 列表命令
```

- [核心设施](./commands/CoreInfrastructure/核心设施.md):
包含底层工具和基础组件，如交易构建、脚本处理、签名验证等核心功能的实现。
- [钱包管理命令](./commands/WalletCommands/钱包管理命令.md): 
创建和管理钱包，包括密钥生成、地址管理、余额查询和UTXO管理等功能。
- [铸造命令](./commands/MintingCommands/铸造命令.md): 
支持多种数字资产的铸造，包括NFT、FT、Container、Realm、Subrealm等，每种资产类型都有其独特的铸造流程和规则。
- [转账命令](./commands/TransferCommands/转账命令.md): 
处理各类数字资产的转移操作，支持单笔和批量转账，包含UTXO选择、交易构建和签名等完整流程。
- [查询命令](./commands/QueryCommands/查询命令.md): 
提供丰富的查询接口，支持按地址、交易ID、资产ID等多维度查询，包括历史记录、状态和元数据等信息。
- [数据管理命令](./commands/DataManagementCommands/数据管理命令.md): 
提供数据的创建、读取、更新和删除功能，支持多种数据格式，包括JSON、文本和二进制数据的处理。
- [领域管理命令](./commands/RealmManagement/领域管理命令.md):
处理领域（Realm）相关的操作，包括领域创建、子领域管理、规则设置等功能。
- [交易广播命令](./commands/TransactionBroadcasting/交易广播命令.md):
负责交易的广播、确认和状态查询，支持交易解码和验证功能。
- [交互操作命令](./commands/InteractiveOperations/交互操作命令.md):
提供一系列交互式操作，如颜色定制、删除、事件发送、封存等高级功能。
- [实用工具命令](./commands/UtilityCommands/实用工具命令.md):
包含下载、预览渲染、解析、服务器状态查询等辅助工具功能。

## utils

```html
/lib/utils/
├── Address & Key Management #地址和密钥管理
│   ├── address-helpers.ts              // 地址相关辅助函数
│   ├── address-keypair-path.ts         // 密钥对路径管理
│   ├── create-key-pair.ts             // 密钥对创建工具
│   ├── create-mnemonic-phrase.ts      // 助记词生成
│   └── decode-mnemonic-phrase.ts      // 助记词解码
│
├── Atomicals Core #Atomicals核心
│   ├── atomical-format-helpers.ts     // Atomicals格式化工具
│   ├── atomical-operation-builder.ts  // 核心操作构建逻辑
│   └── container-validator.ts         // 容器验证工具
│
├── Wallet Management #钱包管理
│   ├── validate-wallet-storage.ts     // 钱包存储验证
│   ├── wallet-path-resolver.ts        // 钱包路径解析
│   └── select-funding-utxo.ts         // UTXO选择逻辑
│
├── Mining #挖矿
│   └── miner-worker.ts               // 挖矿工作器实现
│
├── File & Configuration #文件和配置
│   ├── file-utils.ts                 // 文件处理工具
│   └── hydrate-config.ts             // 配置注入
│
├── Input/Output #输入输出
│   ├── prompt-helpers.ts             // CLI提示工具
│   └── validate-cli-inputs.ts        // CLI输入验证
│
└── General Utilities #通用工具
    └── utils.ts                      // 通用工具函数
```

- [地址和密钥管理工具](./utils/AddressAndKeyManagement/地址和密钥管理.md):
提供处理地址、密钥对等相关的辅助函数，包括路径管理、密钥生成和助记词处理等功能。
- [Atomicals核心工具](./utils/AtomicalsCore/Atomicals核心工具.md):
提供Atomicals协议的核心工具，包括格式化、操作构建和验证等基础功能。
- [钱包管理工具](./utils/Wallet Management/钱包管理工具.md):
处理钱包存储、路径解析和UTXO选择等功能，优化交易输入选择策略。
- [挖矿工具](./utils/Mining/挖矿工具.md):
实现了Atomicals协议的挖矿功能，包括工作量证明算法和挖矿策略。
- [文件和配置工具](./utils/FileAndConfiguration/文件和配置工具.md):
处理文件操作、配置注入和CLI输入验证等功能。
- [输入输出工具](./utils/InputOutput/输入输出工具.md):
提供CLI提示工具和输入验证功能，优化用户交互体验。
- [通用工具](./utils/GeneralUtilities/通用工具.md):
提供通用的辅助函数，包括文件处理、路径管理和CLI输入验证等功能。