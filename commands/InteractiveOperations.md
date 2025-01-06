# Atomicals-js 交互操作命令

## 功能概述
交互操作命令用于处理 Atomicals 的各种交互式操作，包括颜色自定义、删除、事件发送、封存、设置和拆分等功能。这些命令都需要用户交互来完成操作。

## 命令列表

- **[custom-color-interactive-command.ts](#custom-color-interactive-commandts)**
  - 自定义 FT 的颜色属性
- **[delete-interactive-command.ts](#delete-interactive-commandts)**
  - 删除 Atomical 的属性
- **[emit-interactive-command.ts](#emit-interactive-commandts)**
  - 发送事件
- **[seal-interactive-command.ts](#seal-interactive-commandts)**
  - 封存 Atomical
- **[set-interactive-command.ts](#set-interactive-commandts)**
  - 设置 Atomical 属性
- **[set-relation-interactive-command.ts](#set-relation-interactive-commandts)**
  - 设置关系属性
- **[splat-interactive-command.ts](#splat-interactive-commandts)**
  - NFT 拆分操作
- **[split-interactive-command.ts](#split-interactive-commandts)**
  - FT 拆分操作

## 命令详解
![交互操作命令](/images/交互操作命令.png)

# custom-color-interactive-command.ts

## 工作原理
此命令用于自定义 FT（同质化通证）的颜色属性，允许用户将一个 FT 拆分成多个不同数量的部分。

### 核心流程

1. **位置验证和 Atomical 检查**
```typescript
const command = new GetAtomicalsAtLocationCommand(this.electrumApi, this.locationId);
const response = await command.run();
const atomicals = response.data.atomicals;
```
- 验证指定位置的 Atomicals
- 检查是否包含 NFT
- 准备 UTXO 信息

2. **交互式数量分配**
```typescript
async promptCustomColored(index, availableBalance): Promise<{}> {
  let remainingBalance = availableBalance;
  const atomicalColored = {};
  // 用户交互式输入每个部分的数量
  while (remainingBalance > 0) {
    // 获取用户输入的分配数量
    // 验证输入的有效性
    // 更新剩余余额
  }
  return atomicalColored;
}
```
- 允许用户指定每个部分的数量
- 确保总和等于可用余额
- 验证每个部分的最小值（546 聪）

3. **交易构建**
```typescript
const atomicalBuilder = new AtomicalOperationBuilder({
  opType: 'z',  // 自定义颜色操作
  // ...配置项
});
```
- 创建颜色定制操作
- 添加输入和输出
- 设置操作数据

### 使用场景
1. 将大额 FT 拆分成多个小额部分
2. 为不同用途的 FT 创建不同的标识
3. 优化 FT 的分配方案

# delete-interactive-command.ts

## 工作原理
用于删除 Atomical 中的特定属性。

### 核心流程

1. **属性验证**
```typescript
let filesData = await readJsonFileAsCompleteDataObjectEncodeAtomicalIds(this.filesWithDeleteKeys);
```
- 读取要删除的属性列表
- 验证属性格式

2. **删除操作构建**
```typescript
const atomicalBuilder = new AtomicalOperationBuilder({
  opType: 'mod',  // 修改操作
  // ...配置项
});
```
- 设置删除标记
- 构建修改交易

### 安全考虑
- 验证操作权限
- 保护关键属性
- 确保数据一致性

# emit-interactive-command.ts

## 工作原理
用于发送事件信息，通常用于 NFT 的状态更新或通知。

### 核心流程

1. **事件数据准备**
```typescript
let filesData = await prepareFilesDataAsObject(this.files);
```
- 读取事件数据
- 格式化事件内容

2. **事件发送**
```typescript
const atomicalBuilder = new AtomicalOperationBuilder({
  opType: 'evt',  // 事件操作
  // ...配置项
});
```
- 构建事件交易
- 添加事件数据
- 广播交易

### 应用场景
1. NFT 状态更新
2. 链上通知
3. 触发器事件

# seal-interactive-command.ts

## 工作原理
用于封存 Atomical，使其不可再修改。

### 核心流程

1. **封存操作**
```typescript
const atomicalBuilder = new AtomicalOperationBuilder({
  opType: 'sl',  // 封存操作
  // ...配置项
});
```
- 验证封存条件
- 创建封存交易
- 永久锁定状态

### 重要提示
- 封存操作不可逆
- 确保数据完整性
- 谨慎使用该功能

# set-interactive-command.ts

## 工作原理
用于设置 Atomical 的属性值。

### 核心流程

1. **属性设置**
```typescript
let filesData = await readJsonFileAsCompleteDataObjectEncodeAtomicalIds(this.filename);
```
- 读取新属性值
- 验证数据格式

2. **更新操作**
```typescript
const atomicalBuilder = new AtomicalOperationBuilder({
  opType: 'mod',  // 修改操作
  // ...配置项
});
```
- 构建更新交易
- 设置新属性值

# set-relation-interactive-command.ts

## 工作原理
用于设置 Atomical 之间的关系。

### 核心流程

1. **关系定义**
```typescript
await atomicalBuilder.setData({
  $path: '/relns',
  [this.relationName]: this.values
});
```
- 设置关系路径
- 定义关系值

### 应用场景
1. NFT 集合关联
2. 跨 Atomical 引用
3. 关系网络构建

# splat-interactive-command.ts

## 工作原理
用于将多个 NFT 从同一位置拆分到不同位置。

### 核心流程

1. **NFT 验证**
```typescript
const atomicalNfts = atomicals.filter((item) => {
  return item.type === 'NFT';
});
```
- 检查 NFT 数量
- 验证类型

2. **拆分操作**
```typescript
const atomicalBuilder = new AtomicalOperationBuilder({
  opType: 'x',  // 拆分操作
  // ...配置项
});
```
- 创建输出
- 分配 NFT

### 使用限制
- 仅适用于 NFT
- 需要多个 NFT 在同一位置
- 不能包含 FT

# split-interactive-command.ts

## 工作原理
用于将多个 FT 从同一位置拆分到不同位置。

### 核心流程

1. **FT 验证**
```typescript
const atomicalFts = atomicals.filter((item) => {
  return item.type === 'FT';
});
```
- 检查 FT 数量
- 验证类型

2. **拆分操作**
```typescript
const atomicalBuilder = new AtomicalOperationBuilder({
  opType: 'y',  // FT拆分操作
  // ...配置项
});
```
- 计算输出金额
- 构建拆分交易
