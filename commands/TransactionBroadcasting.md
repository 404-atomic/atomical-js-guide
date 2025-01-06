# Atomicals-js 交易广播命令

## 功能概述
交易广播命令用于处理比特币交易的广播、解码和查询。这些命令是与比特币网络交互的核心组件。

## 命令列表
- **[broadcast-command.ts](#broadcast-commandts)**  
  - 广播原始交易到比特币网络。
- **[decode-tx-command.ts](#decode-tx-commandts)**  
  - 解码原始交易数据，转换为可读格式。
- **[tx-command.ts](#tx-commandts)**  
  - 查询特定交易的详细信息。

# broadcast-command.ts
广播原始交易到比特币网络。

## 功能描述
- 接收原始交易数据（rawtx）
- 通过Electrum API广播交易
- 返回广播结果

## 代码实现
```typescript
import { ElectrumApiInterface } from "../api/electrum-api.interface";
import { CommandInterface } from "./command.interface";
 
export class BroadcastCommand implements CommandInterface {
  constructor( 
    private electrumApi: ElectrumApiInterface,
    private rawtx: string,
  ) {
  }
  async run(): Promise<any> {
    const result = await this.electrumApi.broadcast(this.rawtx);
    return {
      success: true,
      data: result,
    };
  }
}
```

# decode-tx-command.ts
解码原始交易数据，转换为可读格式。

## 功能描述
- 接收原始交易数据
- 使用bitcoinjs-lib解析交易内容
- 返回解析后的交易对象

## 代码实现
```typescript
import { CommandInterface } from "./command.interface";
import { Transaction } from 'bitcoinjs-lib/src/transaction';
 
export class DecodeTxCommand implements CommandInterface {
  constructor(
    private rawtx: string
  ) {
     
  }
  async run(): Promise<any> {
    const tx = Transaction.fromHex(this.rawtx);
    return {
      success: true,
      data: {
        tx: tx,
      }
    };
  }
}
```

# tx-command.ts
查询特定交易的详细信息。

## 功能描述
- 接收交易ID（txid）
- 通过Electrum API查询交易详情
- 返回交易信息

## 代码实现
```typescript
import { ElectrumApiInterface } from "../api/electrum-api.interface";
import { CommandInterface } from "./command.interface";

export class TxCommand implements CommandInterface {
  constructor(
    private electrumApi: ElectrumApiInterface,
    private txid: string,
  ) {
  }

  async run(): Promise<any> {
    return this.electrumApi.getTx(this.txid);
  }
}
```

## 使用示例

### 广播交易
```typescript
const broadcastCmd = new BroadcastCommand(electrumApi, rawTransactionHex);
const result = await broadcastCmd.run();
console.log('交易广播结果:', result);
```

### 解码交易
```typescript
const decodeCmd = new DecodeTxCommand(rawTransactionHex);
const decodedTx = await decodeCmd.run();
console.log('解码后的交易:', decodedTx);
```

### 查询交易
```typescript
const txCmd = new TxCommand(electrumApi, transactionId);
const txInfo = await txCmd.run();
console.log('交易信息:', txInfo);
```

### 依赖关系
- bitcoinjs-lib: 用于交易解码和处理
- electrum-api: 用于与Electrum服务器通信

