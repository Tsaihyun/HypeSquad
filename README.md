---

## 📘 HypeSquad 一键阵营同步工具

这是一个基于原生 JavaScript 开发的轻量化工具，旨在帮助 Discord 用户通过 API 直接加入 HypeSquad 阵营（Bravery / Brilliance / Balance）。

Discord 已经停止了 HypeSquad 服务，这意味着相关的页面也消失了。你无法去申请加入HypeSquad，不过，幸好，它的API 接口仍然存在。你可以通过API 来激活HypeSquad图标！

### 🚀 使用说明 (Usage)

1.  **访问项目页**：打开 [HypeSquad](https://hyun.cc/i/discord/HypeSquad.html)。
2.  **获取 Token**：
    *   在电脑端打开 Discord 网页版并登录。
    *   按下 `F12` 或 `Ctrl+Shift+I` 打开开发者工具，切换到 **Network (网络)** 标签页。
    *   在过滤框中输入 `/api/v9/users/@me`。
    *   刷新页面（F5），点击出现的请求项。
    *   在 **Request Headers** 中找到 `authorization`，复制其后的长字符串。
3.  **身份验证**：将 Token 粘贴至网页输入框，验证成功后将显示您的个人信息。
4.  **选择阵营**：点击你心仪的阵营图标。
5.  **同步状态**：点击“同步身份状态”按钮，完成后重启 Discord 即可见效。

---

### 🛠️ 技术原理详解 (Technical Logic)

如果你对安全有顾虑，以下是该工具背后的核心逻辑，你可以随时审查源码或自制脚本：

#### 1. 核心接口 (The Endpoint)
Discord 官方客户端在处理 HypeSquad 测试提交时，最终会向服务器发送一个 `POST` 请求。本工具正是模拟了这一行为：
*   **API 地址**: `[https://discord.com/api/v9/hypesquad/online](https://discord.com/api/v9/hypesquad/online)`
*   **请求方法**: `POST`
*   **有效载荷 (JSON)**: `{"house_id": <ID>}`

#### 2. 阵营 ID 映射表
在 Discord 的数据库中，阵营 ID 是固定的。代码中定义了如下映射：
| 阵营名称 | 内部 ID | 意义 |
| :--- | :--- | :--- |
| **Bravery** | `1` | 勇气 (红色) |
| **Brilliance** | `2` | 卓越 (蓝色) |
| **Balance** | `3` | 平衡 (绿色) |

#### 3. 安全性设计 (Privacy & Security)
*   **无后台传输**：所有的 `fetch` 请求均在你的浏览器前端（Client-side）直接发往 `discord.com`。
*   **代码透明**：不包含任何第三方追踪代码或数据上报逻辑。
*   **本地存储**：Token 仅存储在页面变量中，刷新页面即物理清除，不存储于 LocalStorage。

---

### 📜 自定义脚本示例 (Scripting)

如果你更倾向于直接在浏览器控制台执行代码，其核心逻辑仅需这几行：

```javascript
// 直接在 Discord 控制台执行
const token = "你的TOKEN";
const houseId = 1; // 1:勇气, 2:卓越, 3:平衡

fetch("https://discord.com/api/v9/hypesquad/online", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": token
    },
    body: JSON.stringify({ house_id: houseId })
}).then(res => res.status === 204 ? console.log("同步成功！") : console.error("失败"));
```

---

### ⚠️ 免责声明
本项目仅供学习交流使用。虽然该 API 属于官方移动端/网页端正常调用的范畴，但请勿频繁切换阵营，以免触发 Discord 的速率限制（Rate Limit）或安全风控。

---
