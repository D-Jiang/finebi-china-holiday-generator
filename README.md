# 中国非工作日查询工具

获取指定年份的中国非工作日列表（周末 + 节假日 - 补班）

## 功能说明

- 使用 [Timor API](https://timor.tech/api/holiday) 获取节假日数据
- 自动识别节假日（type.type = 1）和周末（type.type = 2）
- 自动排除调休工作日（type.type = 3）
- 返回格式化的日期数组

## 使用方法

### 方法一：浏览器环境（推荐）

建议用本地静态服务器打开，以避免某些环境下 `file://` 的 CORS 限制：

```bash
npx serve .
# 或
python -m http.server 8080
```

然后在浏览器访问 `http://localhost:PORT/finebi-china-holiday-generator.html`：

1. 打开 `finebi-china-holiday-generator.html`
2. 选择要查询的年份
3. 点击「查询」按钮
4. 结果区域会按 **`YYYY-MM-DD` 一行一个日期** 输出
5. 可使用「复制到剪贴板」或「下载为 TXT」按钮导出结果

### 方法二：Node.js 环境

1. 安装依赖：
```bash
npm install node-fetch
```

2. 运行脚本：
```bash
node getHolidays.js
```

### 方法三：在代码中使用

```javascript
import { getNonWorkingDays } from './getHolidays.js';

// 或者使用 require（Node.js）
// const { getNonWorkingDays } = require('./getHolidays.js');

async function example() {
  const holidays = await getNonWorkingDays(2025);
  console.log(holidays);
  // 输出: ['2025-01-01', '2025-01-04', '2025-01-05', ...]
}

example();
```

## API 说明

### `getNonWorkingDays(year)`

获取指定年份的非工作日列表（周末 + 法定节假日 - 补班）。

**参数：**
- `year` (number): 年份，例如 2025

**返回值：**
- `Promise<string[]>`: 非工作日日期数组，格式为 `['2025-01-01', '2025-01-04', ...]`

**示例：**
```javascript
const holidays = await getNonWorkingDays(2025);
console.log(holidays);
// ['2025-01-01', '2025-01-04', '2025-01-05', ...]
```

## 数据来源

使用 [Timor API](https://timor.tech/api/holiday) 提供的节假日数据：
- 主端点：`https://timor.tech/api/holiday/year/{year}?type=Y&week=Y`
- 兜底端点：`https://timor.tech/api/holiday/batch?d=YYYY-MM-DD&d=...`
- 参数说明：
  - `type=Y`: 返回每一天的类型信息
  - `week=Y`: 返回周末信息

## 类型说明（按 Timor 文档）

- `type = 0`: 工作日
- `type = 1`: 周末
- `type = 2`: 法定节假日
- `type = 3`: 调休工作日（补班，**会从结果中排除**）

## 注意事项

- 需要网络连接才能访问 API
- API 可能有限流，请合理使用
- 数据以 API 返回为准
