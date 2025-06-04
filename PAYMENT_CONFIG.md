# 支付配置说明

本文档说明如何配置支付方式和网站设置。所有配置都在 `src/config/payment-config.ts` 文件中。

## 网站配置

您可以自定义网站的基本信息，包括标题、图标、页面标题和感谢文本。

### 网站配置结构

```typescript
export interface SiteConfig {
  title: string           // 网站标题（显示在浏览器标签页）
  description: string     // 网站描述
  favicon?: string        // 网站图标路径
  pageTitle: string       // 页面H1标题
  pageDescription: string // 页面描述
  thankYouText: string    // 感谢文本
}
```

### 如何自定义网站配置

编辑 `src/config/payment-config.ts` 文件中的 `siteConfig` 对象：

```typescript
export const siteConfig: SiteConfig = {
  title: "您的网站标题",              // 浏览器标签页显示的标题
  description: "您的网站描述",        // 网站描述（用于SEO）
  favicon: "/your-icon.svg",        // 网站图标路径
  pageTitle: "您的页面标题",          // 页面上显示的H1标题
  pageDescription: "您的页面描述",    // 页面描述文本
  thankYouText: "感谢您的支持！"      // 页面底部的感谢文本
}
```

### 自定义网站图标

1. 将您的图标文件放置在 `public` 目录下
2. 支持的格式：`.ico`、`.png`、`.svg` 等
3. 推荐使用 SVG 格式，文件更小且可缩放
4. 在配置中更新 `favicon` 路径

## 配置文件位置

配置文件位于：`src/config/payment-config.ts`

## 支付方式类型

目前支持以下几种支付方式类型：

1. **二维码支付 (qrcode)** - 用于支付宝、微信支付、USDT等需要展示二维码的支付方式
2. **地址支付 (address)** - 用于加密货币等需要展示钱包地址的支付方式
3. **银行卡支付 (bankcard)** - 用于银行卡转账支付方式

## 配置结构

每个支付方式包含以下字段：

```typescript
interface PaymentMethodConfig {
  id: string          // 唯一标识符
  name: string        // 显示名称
  description: string // 描述信息
  type: 'qrcode' | 'address' | 'bankcard'  // 支付类型
  qrCodeUrl?: string  // 二维码图片路径（仅当type为'qrcode'时使用）
  address?: string    // 钱包地址（仅当type为'address'时使用）
  bankCardNumber?: string  // 银行卡号（仅当type为'bankcard'时使用）
  bankName?: string   // 银行名称（仅当type为'bankcard'时使用）
  accountName?: string // 账户名（仅当type为'bankcard'时使用）
  enabled: boolean    // 是否启用此支付方式
}
```

## 配置示例

### 二维码支付方式（支付宝、微信等）

```typescript
// 支付宝二维码
{
  id: "alipay",
  name: "支付宝",
  description: "使用支付宝扫码支付",
  type: "qrcode",
  qrCodeUrl: "/alipay.svg", // 二维码图片路径
  enabled: true
}

// USDT二维码（同时支持二维码和地址）
{
  id: "usdt",
  name: "USDT",
  description: "USDT-TRC20",
  type: "qrcode",
  qrCodeUrl: "/usdt-qr.svg", // USDT收款码路径
  address: "T...", // USDT钱包地址（可选，用于备用显示）
  enabled: true
}
```

### 钱包地址支付方式（加密货币）

```typescript
{
  id: "usdt",
  name: "USDT",
  description: "USDT-TRC20",
  type: "address",
  address: "TYourActualUSDTAddressHere", // 您的实际USDT地址
  enabled: true
}
```

### 银行卡支付方式

```typescript
{
  id: "bankcard",
  name: "银行卡转账",
  description: "银行卡转账支付",
  type: "bankcard",
  bankCardNumber: "6222 **** **** 1234",
  bankName: "中国工商银行",
  accountName: "张三",
  enabled: true
}
```

## 如何添加您的收款码

### 步骤1：准备图片文件

1. 将您的收款码图片（PNG、JPG、SVG格式）放入 `public` 目录
2. 建议图片尺寸为 256x256 像素或更大
3. 文件命名示例：
   - `alipay-qr.png` - 支付宝收款码
   - `wechat-qr.png` - 微信收款码
   - `usdt-qr.svg` - USDT收款码
   - `custom-qr.svg` - 自定义收款码

**注意**：USDT等加密货币支付方式可以同时配置二维码和钱包地址，用户可以选择扫码或复制地址的方式进行支付。

### 步骤2：更新配置文件

编辑 `src/config/payment-config.ts`，更新对应的 `qrCodeUrl` 字段：

```typescript
{
  id: "alipay",
  name: "支付宝",
  description: "使用支付宝扫码支付",
  type: "qrcode",
  qrCodeUrl: "/alipay-qr.png", // 更新为您的图片路径
  enabled: true
}
```

## 如何添加银行卡信息

编辑 `src/config/payment-config.ts`，更新对应的银行卡字段：

```typescript
{
  id: "bankcard",
  name: "银行卡转账",
  description: "银行卡转账支付",
  type: "bankcard",
  bankCardNumber: "6222 0000 0000 1234", // 更新为您的真实银行卡号
  bankName: "中国工商银行", // 更新为您的银行名称
  accountName: "张三", // 更新为您的真实姓名
  enabled: true
}
```

## 如何添加钱包地址

编辑 `src/config/payment-config.ts`，更新对应的 `address` 字段：

```typescript
{
  id: "usdt",
  name: "USDT",
  description: "USDT-TRC20",
  type: "address",
  address: "TYourActualUSDTAddressHere", // 替换为您的真实地址
  enabled: true
}
```

## 启用/禁用支付方式

通过设置 `enabled` 字段来控制支付方式的显示：

```typescript
{
  id: "btc",
  name: "比特币",
  description: "BTC",
  type: "address",
  address: "bc1your-btc-address",
  enabled: false  // 设置为 false 将隐藏此支付方式
}
```

## 添加新的支付方式

您可以在 `paymentConfig` 数组中添加新的支付方式：

```typescript
export const paymentConfig: PaymentMethodConfig[] = [
  // 现有配置...
  {
    id: "paypal",
    name: "PayPal",
    description: "使用PayPal支付",
    type: "qrcode",
    qrCodeUrl: "/paypal-qr.png",
    enabled: true
  },
  {
    id: "doge",
    name: "狗狗币",
    description: "DOGE",
    type: "address",
    address: "DYourDogeAddressHere",
    enabled: true
  },
  {
    id: "bankcard2",
    name: "招商银行",
    description: "招商银行转账",
    type: "bankcard",
    bankCardNumber: "6225 **** **** 5678",
    bankName: "招商银行",
    accountName: "李四",
    enabled: true
  }
]
```

## 注意事项

1. **图片路径**：所有图片路径都是相对于 `public` 目录的
2. **文件格式**：支持 PNG、JPG、SVG 等常见图片格式
3. **地址安全**：请确保钱包地址的准确性，错误的地址可能导致资金丢失
4. **重启服务**：修改配置后需要重启开发服务器才能看到更改

## 配置验证

修改配置后，您可以通过以下方式验证：

1. 检查浏览器控制台是否有错误信息
2. 确认图片能正常显示
3. 测试地址复制功能是否正常工作
4. 验证启用/禁用的支付方式是否正确显示

## 示例完整配置

```typescript
export const paymentConfig: PaymentMethodConfig[] = [
  {
    id: "alipay",
    name: "支付宝",
    description: "使用支付宝扫码支付",
    type: "qrcode",
    qrCodeUrl: "/alipay-qr.png",
    enabled: true
  },
  {
    id: "wechat",
    name: "微信支付",
    description: "使用微信扫码支付",
    type: "qrcode",
    qrCodeUrl: "/wechat-qr.png",
    enabled: true
  },
  {
    id: "usdt",
    name: "USDT",
    description: "USDT-TRC20",
    type: "address",
    address: "TYourActualUSDTAddress",
    enabled: true
  }
]
```