# 🔐 Authentication

To use any SpamTools API route, you must validate your **subscription hash**.  
This hash is provided after purchasing a product and represents your active access.

> 🧠 You can find your hash in your [SpamTools Dashboard](https://spamtools.io/dashboard) under the active subscription.

---

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

---

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

---

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

---

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

