# Atomicals-js 地址和密钥管理

## 概述
本模块负责处理所有与比特币地址和密钥相关的功能，包括地址生成、密钥对管理、助记词操作等核心功能。

## 说明列表
- [address-helpers.ts](#address-helpersts)
  - 提供地址相关的辅助函数
  - 包含地址格式化、验证等功能

- [address-keypair-path.ts](#address-keypair-pathsts)
  - 管理密钥对的路径
  - 处理密钥存储位置和访问逻辑

- [create-key-pair.ts](#create-key-pairsts)
  - 创建新的密钥对
  - 实现安全的密钥生成算法

- [create-mnemonic-phrase.ts](#create-mnemonic-phrasests)
  - 生成标准BIP39助记词
  - 提供多语言支持

- [decode-mnemonic-phrase.ts](#decode-mnemonic-phrasests)
  - 解码助记词
  - 从助记词恢复密钥对

# address-helpers.ts
## 基础概念
比特币地址有多种格式，每种都有其特定用途和特点。这个工具包帮助你处理不同类型的地址转换和验证。

![address-helpers](/images/address-helpers.png)

## 主要功能
1. 地址类型检测
```typescript
// 检测地址类型
const addressType = getAddressType("bc1q....");

// 支持的地址类型
enum AddressTypeString {
  p2pkh,        // Legacy地址 (1...)
  p2tr,         // Taproot地址 (bc1p...)
  p2sh,         // 脚本哈希地址 (3...)
  p2wpkh,       // SegWit地址 (bc1q...)
  // ... 测试网络变体
}
```
2. 地址格式化
```typescript
export function detectAddressTypeToScripthash(address: string) {
  // 尝试检测是否为传统地址
  try {
    bitcoin.address.fromBase58Check(address, NETWORK);
    const p2pkh = addressToP2PKH(address);
    // ... 转换逻辑
  } catch (err) {
    // 如果不是传统地址，尝试其他类型
  }
}
```
3. UTXO处理
```typescript
export function utxoToInput(
  utxo: any,
  address: string,
  publicKey: string,
  option: {
    override: {
      vout?: number;
      script?: string | Buffer;
    };
  },
) {
  const addressType = getAddressType(address);
  // ... 根据地址类型处理 UTXO
}
```

## 实用示例
检查地址类型
```typescript
// 示例：检查一个地址的类型
const address = "bc1qar0srrr7xfkvy5l643lydnw9re59gtzzwf5mdq";
const addressType = getAddressType(address);
console.log(`地址类型是: ${addressType}`); // 输出：p2wpkh
```

转换地址格式
```typescript
// 示例：转换地址到脚本哈希
const scriptInfo = detectAddressTypeToScripthash(address);
console.log("输出脚本:", scriptInfo.output);
console.log("脚本哈希:", scriptInfo.scripthash);
```

# address-keypair-path.ts
## 功能概述
这个模块帮助你从助记词生成Taproot地址和密钥对。它实现了BIP39助记词、BIP32路径派生，最后生成Taproot（P2TR）地址。
![address-keypair-path](/images/address-keypair-path.png)

## 主要函数分析
1. getExtendTaprootAddressKeypairPath 函数
```typescript
export const getExtendTaprootAddressKeypairPath = async (
  phrase: string,      // 助记词
  path: string,        // 派生路径
  passphrase?: string, // 可选密码
) => {
  // 1. 从助记词生成种子
  const seed = await bip39.mnemonicToSeed(phrase, passphrase);
  
  // 2. 从种子生成根密钥
  const rootKey = bip32.fromSeed(seed);
  
  // 3. 使用路径派生子密钥
  const childNode = rootKey.derivePath(path);
  
  // 4. 获取 XOnly 公钥
  const childNodeXOnlyPubkey = childNode.publicKey.slice(1, 33);
  
  // 5. 生成 Taproot 地址
  const { address, output } = bitcoin.payments.p2tr({
    internalPubkey: childNodeXOnlyPubkey,
    network: NETWORK
  });
```
2. getKeypairInfo 函数
这是一个辅助函数，用于生成密钥对信息：
```typescript
export const getKeypairInfo = (childNode: any): KeyPairInfo => {
  // 1. 转换为 XOnly 公钥
  const childNodeXOnlyPubkey = toXOnly(childNode.publicKey);
  
  // 2. 生成 Taproot 地址和输出
  const { address, output } = bitcoin.payments.p2tr({
    internalPubkey: childNodeXOnlyPubkey,
    network: NETWORK
  });
}
```

# create-key-pair.ts
## 功能概述
这个模块提供了创建新的密钥对的功能，包括生成随机密钥对和从助记词恢复密钥对等。
![create-key-pair](/images/create-key-pair.png)

功能详解
1. 创建密钥对的主函数
```typescript
export const createKeyPair = async (
    phrase: string = '',              // 助记词（可选）
    path = defaultDerivedPath,        // 派生路径（默认值）
    passphrase: string = ''           // 密码（可选）
) : Promise<KeyPair>
```
2. 关键步骤解析
```typescript
// 如果没有提供助记词，就创建新的
if (!phrase || phrase === '') {
    const phraseResult = createMnemonicPhrase();
    phrase = phraseResult.phrase;
}
// 从助记词生成种子
const seed = await bip39.mnemonicToSeed(phrase, passphrase);
// 2. 从种子生成根密钥
const rootKey = bip32.fromSeed(seed);
// 3. 使用路径派生子密钥
const childNode = rootKey.derivePath(path);
// 4. 获取 XOnly 公钥
const childNodeXOnlyPubkey = toXOnly(childNode.publicKey);
// 5. 生成 Taproot 地址和输出
const p2trPrimary = bitcoin.payments.p2tr({
    internalPubkey: childNodeXOnlyPubkeyPrimary,
    network: NETWORK
});
```
## 实用示例
```typescript
export const createNKeyPairs = async (
    phrase: string | undefined,
    passphrase: string | undefined,
    n = 1  // 要创建的密钥对数量
) => {
    const keypairs: any = [];
    for (let i = 0; i < n; i++) {
        keypairs.push(await createKeyPair(
            phrase, 
            `${defaultDerivedPath.substring(0, 13)}/${i}`, 
            passphrase
        ));
    }
    return { phrase, keypairs };
}

//XOnly 公钥转换
export const toXOnly = (publicKey) => {
    return publicKey.slice(1, 33);  // 提取 32 字节的 X 坐标
}
```

# create-mnemonic-phrase.ts
## 功能概述
这个模块提供了生成标准BIP39助记词的功能，可以选择不同的语言和字典。

## 核心函数解析
```typescript
export function createMnemonicPhrase() : ({
    phrase: string
}) {
    // 步骤1：生成随机熵（16字节的随机数据）
    const entropy = randomBytes(16);
    
    // 步骤2：转换为十六进制字符串
    const entropyHex = entropy.toString('hex');
    
    // 步骤3：转换为助记词
    const mnemonic = bip39.entropyToMnemonic(entropyHex);
    
    // 步骤4：验证助记词
    if (!bip39.validateMnemonic(mnemonic)) {
        throw new Error("助记词无效！");
    }
    
    // 返回生成的助记词
    return {
        phrase: mnemonic
    }
}
```

# decode-mnemonic-phrase.ts
## 功能概述
这个模块提供了从助记词恢复密钥对的功能，可以选择不同的语言和字典。

## 核心函数解析
1. 输入验证
```typescript
// 验证助记词是否有效
if (!bip39.validateMnemonic(phrase)) {
    throw new Error("助记词无效！");
}
```
2. 生成种子
```typescript
const seed = await bip39.mnemonicToSeed(phrase, passphrase);
```
3. 从种子生成根密钥
```typescript
const rootKey = bip32.fromSeed(seed);
```
4. 使用路径派生子密钥
```typescript
const childNode = rootKey.derivePath(path);
```
5. Taproot 地址生成
```typescript
// 提取 XOnly 公钥
const childNodeXOnlyPubkey = toXOnly(childNode.publicKey);
// 生成 Taproot 地址
const p2tr = bitcoin.payments.p2tr({
    internalPubkey: childNodeXOnlyPubkey,
    network: NETWORK
});
```