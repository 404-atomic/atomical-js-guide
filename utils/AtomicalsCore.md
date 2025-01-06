# Atomicals-js Atomicals核心工具

## 概述
本模块包含Atomicals协议的核心工具函数，处理格式化、操作构建和验证等基础功能。

## 说明列表
- [atomical-format-helpers.ts](#atomical-format-helpersts)
  - Atomicals数据格式化工具
  - 提供标准化的数据处理接口

- [atomical-operation-builder.ts](#atomical-operation-builderts)
  - 构建Atomicals操作的核心逻辑
  - 实现各类操作的构建流程

### atomical-format-helpers.ts
#### 基础概念
这是一个用于处理 Atomicals 协议数据的工具库，主要包含以下功能：
1. 标识符处理
2. CBOR 数据编解码
3. Bitwork 工作量证明验证
4. 数据扩展和装饰
5. 验证规则处理

#### 标识符处理
1. 处理不同类型的标识符：
```markdown
Atomical ID (如: txid+index)
Realm 名称 (以 + 开头)
Container 名称 (以 # 开头)
Ticker 名称 (以 $ 开头)
```
2. CBOR 数据处理
```typescript
export function decodePayloadCBOR(payload: any, hexify = true, addUtf8 = false) {
  if (hexify) {
    return hexifyObjectWithUtf8(cbor.decode(payload), addUtf8);
  }
  return cbor.decode(payload);
}
```
3. Bitwork 验证
```typescript
export const isValidBitworkString = (fullstring, safety = true): BitworkInfo => {
  // 验证工作量证明格式和难度
  const splitted = fullstring.split('.');
  // ... 验证逻辑
}
```
#### 关键工具函数
1. Atomical ID 处理
```typescript
export function isAtomicalId(atomicalId) {
  if (typeof atomicalId !== 'string' || 
      !atomicalId?.length || 
      atomicalId?.indexOf('i') !== 64) {
    return false;
  }
  // ... 解析逻辑
}
```
2. 名称验证
```typescript
export const isValidRealmName = (name: string) => {
  // 验证领域名称格式
  if (!/^[a-z][a-z0-9\-]{0,63}$/.test(name)) {
    throw new Error('Invalid realm name');
  }
  return true;
}
```
3. 数据转换
```typescript
export function hexifyObjectWithUtf8(obj: any, utf8 = true): any {
  // 将 Buffer 转换为十六进制字符串
  function isBuffer(obj) {
    return Buffer.isBuffer(obj);
  }
  // ... 转换逻辑
}
```

#### 验证规则处理
1. 子域名规则验证
```typescript
export function validateSubrealmRulesObject(topobject) {
  if (!topobject.subrealms) {
    throw new Error(`Missing 'subrealms' object`);
  }
  // ... 验证规则
}
```
2. 价格规则验证
```typescript
// 验证价格规则
if (priceRuleValue < 0) {
  throw new Error('Price cannot be negative');
}
if (priceRuleValue > 100000000000) {
  throw new Error('Price exceeds maximum');
}
```

### atomical-operation-builder.ts
#### 基础概念
AtomicalOperationBuilder 是一个用于构建 Atomical 操作的工具类，主要功能包括：
1. 构建和发送 Commit 事务
2. 构建和发送 Reveal 事务
3. 支持多种操作类型（NFT/FT/DFT）
4. 工作量证明（Bitwork）处理
5. 手续费计算和管理

#### 主要类和接口
1. 基础配置接口
```typescript
export interface AtomicalOperationBuilderOptions {
    electrumApi: ElectrumApiInterface;
    rbf?: boolean;                    // 是否支持手续费替换
    satsbyte?: number;                // 每字节的聪数
    address: string;                  // 地址
    opType: "nft" | "ft" | "dft" | "dmt"; // 操作类型
    // ... 其他配置选项
}
```
2.  费用计算接口
```typescript
export interface FeeCalculations {
    commitAndRevealFeePlusOutputs: number;  // 总费用（包括输出）
    commitAndRevealFee: number;             // 总费用
    revealFeePlusOutputs: number;           // Reveal费用（包括输出）
    commitFeeOnly: number;                  // 仅Commit费用
    revealFeeOnly: number;                  // 仅Reveal费用
}
```
#### 关键流程解析
1. 初始化过程
```typescript
constructor(private options: AtomicalOperationBuilderOptions) {
    if (!this.options) {
        throw new Error("Options required");
    }
    // 验证和设置基本参数
    if (!this.options.satsbyte) {
        this.options.satsbyte = DEFAULT_SATS_BYTE;
    }
    // ... 其他初始化逻辑
}
```
2. Commit 事务构建
```typescript
// 构建Commit事务
const baseTxCommit = prepareCommitRevealConfig(
    this.options.opType,
    fundingKeypair,
    atomPayload
);

// 计算费用
const fees = this.calculateFeesRequiredForAccumulatedCommitAndReveal(
    hashLockP2TROutputLen,
    performBitworkForRevealTx
);
```
3. Reveal 事务构建
```typescript
// 构建Reveal事务
psbt.addInput({
    hash: utxoOfCommitAddress.txid,
    index: utxoOfCommitAddress.vout,
    witnessUtxo: {
        value: utxoOfCommitAddress.value,
        script: hashLockP2TR.output!,
    },
    tapLeafScript: [tapLeafScript]
});
```
#### 特殊功能实现
1. Bitwork 挖矿实现
```typescript
if (performBitworkForCommitTx) {
    printBitworkLog(this.bitworkInfoCommit as any, true);
    // 使用工作线程进行挖矿
    const workerCount = os.cpus().length - 1;
    // ... 多线程挖矿逻辑
}
```
2. 费用计算
```typescript
calculateFeesRequiredForCommit(): number {
    return Math.ceil(
        (this.options.satsbyte as any) *
        (BASE_BYTES + INPUT_BYTES_BASE + OUTPUT_BYTES_BASE)
    );
}
```
#### 使用示例
```typescript
// 创建 NFT
const builder = new AtomicalOperationBuilder({
    address: "your-address",
    opType: "nft",
    nftOptions: {
        satsoutput: 1000
    }
});

// 设置 Container 名称
builder.setRequestContainer("my-container");

// 开始构建和广播
const result = await builder.start(fundingWIF);
```
