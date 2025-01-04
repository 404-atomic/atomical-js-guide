# Atomicals-js 钱包管理工具

## 概述
本模块提供钱包管理相关的工具函数，包括存储验证、路径解析和UTXO选择等功能。

## 文件说明
- **validate-wallet-storage.ts**
  - 验证钱包存储的完整性
  - 确保钱包数据的安全性

- **wallet-path-resolver.ts**
  - 解析钱包文件路径
  - 管理钱包文件的存储位置

- **select-funding-utxo.ts**
  - UTXO选择逻辑实现
  - 优化交易输入选择策略
