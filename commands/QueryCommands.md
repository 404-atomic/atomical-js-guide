# Atomicals-js 查询命令

## 查询命令概述
查询命令用于获取各种信息，包括地址信息、资产详情、全局状态等。本文档详细介绍了所有查询相关的命令操作。

## 命令列表
- **[get-by-ticker-command.ts](#get-by-ticker-commandts)**  
  - 用于通过代币的Ticker符号进行查询。
- **[get-by-realm-command.ts](#get-by-realm-commandts)**  
  - 用于通过领域（Realm）进行查询。
- **[get-by-container-command.ts](#get-by-container-commandts)**  
  - 用于通过容器（Container）进行查询。
- **[get-atomicals-address-command.ts](#get-atomicals-address-commandts)**  
  - 用于查询特定地址的原子化信息。

# get-by-ticker-command.ts
## 功能概述
GetByTicker命令用于通过代币的Ticker符号（例如 $atom）查找对应的Atomical信息。它首先查找Atomical ID，然后获取完整的Atomical数据。

![get-by-ticker-command](/images/get-by-ticker-command.png)

## 工作流程
1. 初始查询
```typescript
// 通过Ticker查询Atomical ID
const responseResult = await this.electrumApi.atomicalsGetByTicker(this.ticker);
if (!responseResult.result || !responseResult.result.atomical_id) {
  return {
    success: false,
    data: responseResult.result
  }
}
```
2. 获取详细信息
```typescript
// 使用GetCommand获取完整信息
const getDefaultCommand = new GetCommand(
  this.electrumApi,
  responseResult.result.atomical_id,
  this.fetchType
);
const getDefaultCommandResponse = await getDefaultCommand.run();
```
3. 数据处理与返回
```typescript
// 装饰并合并结果
const updatedRes = Object.assign({},
  getDefaultCommandResponse.data,
  {
    result: decorateAtomical(getDefaultCommandResponse.data.result)
  }
);

return {
  success: true,
  data: updatedRes
}
```

## 使用示例
```typescript
// 创建命令实例
const getByTicker = new GetByTickerCommand(
  electrumApi,                         // Electrum API实例
  "ATOM",                             // 要查询的Ticker
  AtomicalsGetFetchType.GET           // 查询类型（可选）
);

// 执行查询
try {
  const result = await getByTicker.run();
  if (result.success) {
    console.log('查询成功:', result.data);
  } else {
    console.log('未找到Ticker对应的Atomical');
  }
} catch (error) {
  console.error('查询失败:', error);
}
```

# get-by-realm-command.ts
## 功能概述
GetByRealm命令帮助你通过Realm名称查找Atomical信息。想象它像是一个域名查找工具，输入域名（Realm），得到完整的信息。

![get-by-realm-command](/images/get-by-realm-command.png)

## 工作流程
1. Realm查询
```typescript
// 获取Realm信息
const responseResult = await this.electrumApi
  .atomicalsGetRealmInfo(this.realm);

// 检查结果
if (!responseResult.result?.atomical_id) {
  return {
    success: false,
    data: responseResult.result
  }
}
```
2. 获取详细信息
```typescript
// 使用GetCommand获取完整数据
const getDefaultCommand = new GetCommand(
  this.electrumApi,
  responseResult.result.atomical_id,
  this.fetchType
);
const response = await getDefaultCommand.run();
```
3. 数据处理
```typescript
// 整理并装饰数据
const updatedRes = Object.assign({},
  response.data,
  {
    result: decorateAtomical(response.data.result)
  }
);
```

## 使用示例
```typescript
// 最简单的用法 - 只查询Realm信息
const getByRealm = new GetByRealmCommand(
  electrumApi,
  "gaming"  // Realm名称，例如 "gaming"
);

const result = await getByRealm.run();
console.log('Realm信息:', result.data);
```

# get-by-container-command.ts
## 功能概述
Container查询让你能通过Container名称（例如 #gaming）找到相关的Atomical信息。它就像一个文件夹查找系统，你给它一个名称，它返回所有相关信息。

![get-by-container-command](/images/get-by-container-command.png) 

## 工作流程
1. 名称处理
```typescript
// 移除#前缀（如果有的话）
const trimmedContainer = this.container.startsWith('#') 
  ? this.container.substring(1) 
  : this.container;
```
2. 查询过程
```typescript
// 获取Container信息
const responseResult = await this.electrumApi
  .atomicalsGetByContainer(trimmedContainer);

// 验证结果
if (!responseResult.result?.atomical_id) {
  return {
    success: false,
    data: responseResult.result
  }
}
```
3. 获取详细信息
```typescript
// 使用GetCommand获取完整数据
const getDefaultCommand = new GetCommand(
  this.electrumApi,
  responseResult.result.atomical_id,
  this.fetchType
);

// 获取并处理结果
const response = await getDefaultCommand.run();
const updatedRes = Object.assign({},
  response.data,
  {
    result: decorateAtomical(response.data.result)
  }
);
```

## 使用示例
```typescript
async function queryContainer(containerName: string) {
  const command = new GetByContainerCommand(
    electrumApi,
    containerName
  );

  try {
    const result = await command.run();
    if (result.success) {
      console.log('找到Container:', result.data.result);
    } else {
      console.log('Container不存在');
    }
  } catch (error) {
    console.error('查询出错:', error);
  }
}

// 使用方法
await queryContainer('#gaming');
```

# get-atomicals-address-command.ts
## 功能概述
GetAtomicalsAddress命令用于查询特定地址的原子化信息，包括地址余额，原子化资产，以及地址历史等信息。

![get-atomicals-address-command](/images/get-atomicals-address-command.png)

## 工作流程
1. 地址转换：把比特币地址转换为scripthash
2. 资产查询：使用scripthash查询相关的Atomicals
```typescript
async run(): Promise<any> {
  // 第1步：转换地址为scripthash
  const { scripthash } = detectAddressTypeToScripthash(this.address);
  // 第2步：查询Atomicals
  const result = await this.electrumApi.atomicalsByScripthash(scripthash);
  return result;
}
```

## 使用示例
```typescript
async function checkAddressAtomicals(address: string) {
  const command = new GetAtomicalsAddressCommand(
    electrumApi,
    address
  );

  try {
    const result = await command.run();
    console.log('找到的Atomicals:', result);
  } catch (error) {
    console.error('查询出错:', error);
  }
}                                                                                                                                                              