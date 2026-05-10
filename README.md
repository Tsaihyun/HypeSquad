---
This is a very practical tool for the Discord community. Since the official interface for the HypeSquad quiz has been deprecated in recent UI updates, providing a direct API-based solution is the only way for new users to get those badges.

---

# 📘 HypeSquad One-Click Sync Tool

A lightweight tool built with native JavaScript designed to help Discord users join a HypeSquad house (**Bravery**, **Brilliance**, or **Balance**) directly via the Discord API.

**Note:** Discord has officially retired the HypeSquad web interface, meaning the quiz pages are gone. However, the underlying API endpoints remain active. You can use this tool to trigger the API and activate your HypeSquad badge!

---

## 🚀 Usage Instructions

1. **Access the Tool**: Open the [HypeSquad Sync Page](https://hyun.cc/i/discord/HypeSquad.html).
2. **Retrieve Your Token**:
* Open the Discord Web version in your desktop browser and log in.
* Press `F12` or `Ctrl+Shift+I` to open **Developer Tools** and switch to the **Network** tab.
* In the filter box, type `/api/v9/users/@me`.
* Refresh the page (`F5`) and click on the request that appears.
* In the **Request Headers** section, find the `authorization` field and copy the long string.


3. **Authentication**: Paste your Token into the input box on the tool page. Once verified, your profile info will be displayed.
4. **Select a House**: Click the icon of the house you wish to join.
5. **Sync Status**: Click the **"Sync Identity Status"** button. Restart your Discord client to see the badge on your profile.

---

## 🛠️ Technical Logic

If you have security concerns, here is the core logic behind the tool. You can audit the source code or create your own script based on this:

### 1. The Endpoint

When the official Discord client processes a HypeSquad quiz submission, it sends a `POST` request to the server. This tool simulates that specific behavior:

* **API URL**: `[https://discord.com/api/v9/hypesquad/online](https://discord.com/api/v9/hypesquad/online)`
* **Method**: `POST`
* **Payload (JSON)**: `{"house_id": <ID>}`

### 2. House ID Mapping

Within Discord’s database, House IDs are fixed constants:

| House Name | Internal ID | Color Theme |
| --- | --- | --- |
| **Bravery** | `1` | Red |
| **Brilliance** | `2` | Blue |
| **Balance** | `3` | Green |

### 3. Security & Privacy

* **No Backend Transmission**: All `fetch` requests are executed client-side (directly from your browser to `discord.com`).
* **Transparent Code**: Contains no third-party tracking or data reporting logic.
* **Ephemeral Storage**: Your Token is stored only in page variables; it is physically cleared upon refreshing the page and is never saved to `LocalStorage`.

---

## 📜 Custom Script Example

If you prefer to execute the logic directly in your browser console, use the following snippet:

```javascript
// Execute this directly in the Discord Web console
const token = "YOUR_TOKEN_HERE";
const houseId = 1; // 1: Bravery, 2: Brilliance, 3: Balance

fetch("https://discord.com/api/v9/hypesquad/online", {
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": token
    },
    body: JSON.stringify({ house_id: houseId })
}).then(res => res.status === 204 ? console.log("Sync Successful!") : console.error("Sync Failed"));

```

---

## ⚠️ Disclaimer

This project is for educational and exchange purposes only. While this API is a standard part of official mobile/web traffic, please avoid switching houses frequently to prevent triggering Discord's **Rate Limits** or security risk flags.

---

Would you like me to add any specific troubleshooting steps for users who might encounter `401 Unauthorized` errors during the token phase?
