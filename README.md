# THSAutoTrader

基于同花顺（THS）客户端的自动化交易工具，采用闪电下单模式，并提供可视化交互界面与 API 接口

## 功能特性

- 🚀 支持市价买入、市价卖出操作，获取当前持仓数据
- 🔌 提供 API 接口，便于外部系统调用，实现自动化交易
- ⚖️ 完全基于用户操作模拟，无修改或破解同花顺客户端，确保合法合规
- 🖥️ 提供友好的交互界面，支持多种编程语言调用
- ⌨️ 通过键盘模拟输入，实现自动化交易流程

## 打包后文件（可直接运行）

- dist/下单辅助程序.exe
- 注意：需要下载整个 dist 目录，然后运行下单辅助程序.exe

## 技术栈

- Python 3.11
- Poetry（依赖管理）
- Flask（Web 服务）
- PyWin32（Windows 窗口控制）

## 快速开始

### 环境准备

1. 安装 Python 3.11
2. 安装 Poetry
3. window 系统

### 安装依赖

```bash
poetry install
```

### 运行项目

#### 开发模式（热加载）

```bash
poetry run dev
```

### 项目打包

```bash
poetry run scripts:build
```

### 前端项目（Vue）

- 目录：/front
- node >= 18

```
// 依赖安装(进入front目录下执行命令)
npm install

// 运行
npm run dev

// 打包
npm run build
```

## 使用指南

### 前置准备

1. 打开同花顺客户端
2. 登录交易账户
3. 进入目标个股页面
4. 配置快捷下单：
   - 买入金额锁定
   - 设置为一键买入（买入时不需要确认）

### API 调用示例

#### 下单接口

```bash
# 买入操作
http://localhost:5000/xiadan?code=600000&status=1

# 卖出操作
http://localhost:5000/xiadan?code=600000&status=2
```

参数说明：

- `code`: 股票代码，如 600000（浦发银行）
- `status`: 交易类型，1 表示买入，2 表示卖出

#### 撤单接口（全撤）

```bash
http://localhost:5000/cancel_all_orders
```

#### 获取持仓接口

```bash
http://localhost:5000/position
```

注意:

- 需要保持同花顺交易的登录状态
- 交易界面以独立窗口运行（不要开精简模式，否则无法找到窗口）
- 不稳定（包括可能验证码识别错误），建议做错误重试

返回格式：

```json
{
  "data": [
    {
      "": "",
      "交易市场": "深圳Ａ股",
      "仓位占比(%)": "39.31",
      "冻结数量": "0",
      "可用余额": "3300",
      "市价": "16.230",
      "市值": "53559.000",
      "序号": "1",
      "当日买入": "0",
      "当日卖出": "0",
      "当日盈亏": "2178.00",
      "当日盈亏比(%)": "4.24",
      "成本价": "17.796",
      "盈亏": "-5166.790",
      "盈亏比例(%)": "-8.800",
      "股票余额": "3300",
      "证券代码": "002229",
      "证券名称": "鸿博股份"
    }
  ],
  "status": "success"
}
```

#### 模拟键盘按键接口

```bash
# 输入股票代码并回车
http://localhost:5000/send_key?key=600000+ENTER

# 闪电买入
http://localhost:5000/send_key?key=21+ENTER

# 闪电卖出
http://localhost:5000/send_key?key=F12
```

> 说明：
>
> - URL 中的 `+` 号会被自动转换为空格
> - 每多一个空格，代表操作间隔增加 500ms
> - `21` 为同花顺键盘精灵的闪电买入快捷键
> - `23` 为闪电卖出快捷键
> - 支持其他键盘精灵的输入操作
> - 常用快捷键：
>   - F1: 帮助
>   - F3: 上证指数
>   - F4: 深证成指
>   - F5: 分时/K 线切换
>   - F6: 自选股
>   - F12: 交易

#### 获取当前股票信息接口

```bash
http://localhost:5000/stock_info?code=600000
```

返回格式：

```json
{
  "code": 200,
  "data": {
    "stock_code": "600000",
    "stock_name": "浦发银行",
    "current_price": 7.5,
    "change": "+0.15",
    "change_percent": "+2.04%"
  }
}
```

#### 获取资金余额接口

```bash
http://localhost:5000/balance
```

返回格式：

```json
{
  "data": {
    "冻结金额": "0.00",
    "可取金额": "0.00",
    "可用金额": "59459.35",
    "当日盈亏": "-2508.00",
    "当日盈亏比": "-1.88%",
    "总资产": "130833.35",
    "持仓盈亏": "-6091.78",
    "股票市值": "71374.00",
    "资金余额": "59459.35"
  },
  "status": "success"
}
```

## 注意事项

1. 请确保同花顺客户端（统一版）已正确配置快捷下单
2. 交易前请仔细核对同花顺交易设置
3. 建议在模拟交易环境中充分测试后再进行实盘操作
4. 交易系统要单独一个窗口打开，不要精简模式

![交易系统窗口示例](https://github.com/user-attachments/assets/fe5ed4de-b895-459f-a927-55d49f1e17ec)


## 免责声明

⚠️ **重要提示：请仔细阅读以下声明**

1. **软件用途说明**：本软件仅供学习、研究和技术交流使用，不构成任何投资建议或操作指导。

2. **风险自担原则**：使用本软件进行的任何交易操作，其产生的盈亏结果完全由使用者自行承担，与本软件开发者无关。

3. **技术风险提醒**：本软件可能存在技术缺陷、网络延迟、系统故障等技术风险，可能导致交易失败或异常，使用者应当充分评估此类风险。

4. **免责条款**：本软件开发者不对使用本软件造成的任何直接或间接损失承担责任，包括但不限于经济损失、数据丢失、系统故障等。

5. **版权声明**：本软件仅供个人学习使用，禁止用于商业用途。如需商业使用，请联系开发者获得授权。

**使用本软件即表示您已阅读、理解并同意上述免责声明的所有条款。如不同意，请立即停止使用本软件。**
