# Atomicals-js 实用工具命令

## 功能概述
实用工具命令提供了一系列辅助功能，用于下载、预览、解析和查询 Atomicals 相关信息。这些命令主要用于开发调试和系统维护。

## 命令列表

- **[download-command.ts](#download-commandts)**
  - 下载和解析 Atomicals 交易数据
- **[render-previews-command.ts](#render-previews-commandts)**
  - 预览 Atomicals 文件内容
- **[resolve-command.ts](#resolve-commandts)**
  - 解析 Atomicals 标识符
- **[server-version-command.ts](#server-version-commandts)**
  - 获取服务器版本信息
- **[get-global-command.ts](#get-global-commandts)**
  - 获取全局状态信息
- **[list-command.ts](#list-commandts)**
  - 列出 Atomicals 信息

## 命令详解

# download-command.ts

## 工作原理
该命令用于下载和解析 Atomicals 交易数据，将其保存为可读的文件格式。

### 核心流程

1. **交易获取与解析**
```typescript
const txResult = await this.electrumApi.getTx(txid);
const filemap = buildAtomicalsFileMapFromRawTx(tx, false, false);
```
- 通过 txid 获取交易数据
- 解析交易内容为文件映射

2. **文件写入处理**
```typescript
const writeFiles = async (inputIndexToFilesMap: any, txDir: string): Promise<FileMap> => {
  // 遍历文件映射
  for (const inputIndex in inputIndexToFilesMap) {
    // 创建目录结构
    // 写入原始数据
    // 解析并保存文件内容
  }
}
```
- 创建目录结构
- 保存原始交易数据
- 解析并保存各个文件

3. **文件类型处理**
```typescript
if (fileEntry['$ct'] && fileEntry['$b']) {
  // 处理带内容类型的文件
} else if (fileEntry['$b']) {
  // 处理 JSON 格式文件
} else {
  // 处理其他类型文件
}
```
- 支持多种文件类型
- 自动检测文件扩展名
- 保持数据完整性

### 输出结构
```json
{
  "inputIndex": {
    "directory": "存储目录",
    "files": {
      "filename": {
        "filename": "文件名",
        "fileNameWithExtension": "带扩展名的文件名",
        "detectedExtension": "检测到的扩展名",
        "fullPath": "完整路径",
        "contentType": "内容类型",
        "contentLength": "内容长度",
        "body": "文件内容（十六进制）"
      }
    }
  }
}
```

# render-previews-command.ts

## 工作原理
用于预览 Atomicals 文件内容，支持图片和文本文件的终端显示。

### 核心功能

1. **文件类型检测**
```typescript
export function isImage(contentType: string): boolean {
  return /^image\/(jpe?g|png|gif|bmp|webp|svg)$/.test(contentType);
}

export function isText(contentType: string): boolean {
  return /utf\-?8|application\/json|text\/plain|markdown|xml|html/.test(contentType);
}
```
- 支持常见图片格式
- 支持文本和 JSON 格式
- 自动检测文件类型

2. **内容渲染**
```typescript
if (isImage(contentType)) {
  // 使用 terminal-image 显示图片
} else if (isText(contentType)) {
  // 解码并显示文本内容
} else {
  // 提示手动查看文件
}
```
- 图片终端预览
- 文本内容解码
- 友好的提示信息

# resolve-command.ts

## 工作原理
解析各种类型的 Atomicals 标识符，支持多种查询方式。

### 标识符类型

1. **支持的标识符类型**
```typescript
enum AtomicalIdentifierType {
  ATOMICAL_ID,      // Atomical ID
  ATOMICAL_NUMBER,  // Atomical 编号
  REALM_NAME,       // 领域名称
  CONTAINER_NAME,   // 容器名称
  TICKER_NAME       // 代币名称
}
```

2. **解析流程**
```typescript
const atomicalType = getAtomicalIdentifierType(this.atomicalAliasOrId);
// 根据类型选择相应的命令
if (atomicalType.type === AtomicalIdentifierType.ATOMICAL_ID) {
  // 使用 GetCommand
} else if (atomicalType.type === AtomicalIdentifierType.REALM_NAME) {
  // 使用 GetByRealmCommand
} // ... 其他类型
```
- 自动识别标识符类型
- 选择合适的查询命令
- 统一的返回格式

# server-version-command.ts

## 工作原理
获取 Electrum 服务器的版本信息。

### 实现细节
```typescript
async run(): Promise<any> {
  const result = await this.electrumApi.serverVersion();
  return result;
}
```
- 简单直接的实现
- 用于服务器兼容性检查
- 版本信息验证

# get-global-command.ts

## 工作原理
获取 Atomicals 的全局状态信息。

### 核心功能
```typescript
async run(): Promise<any> {
  let response = await this.electrumApi.atomicalsGetGlobal(this.hashes);
  // 装饰返回结果
  return {
    success: true,
    data: updatedRes
  }
}
```
- 获取全局状态
- 结果格式化
- 数据装饰处理

# list-command.ts

## 工作原理
列出 Atomicals 信息，支持分页和排序。

### 实现细节
```typescript
async run(): Promise<any> {
  const response = await this.electrumApi.atomicalsList(
    this.limit,   // 限制数量
    this.offset,  // 偏移量
    this.asc      // 升序/降序
  );
  // 处理返回结果
}
```
- 支持分页查询
- 灵活的排序选项
- 结果装饰处理


