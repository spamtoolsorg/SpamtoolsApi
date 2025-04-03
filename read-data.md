# 📤 Fetching Data from Live Panel

To read logs from your SpamTools Live Panel, you must provide both your **Security Code** and your **Subscription Hash** for secure access.

Unlike the `add-subscription-data` endpoint, the fetch endpoint has **strict security requirements** to prevent unauthorized access to sensitive data.

---

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

---

## 🔑 Where to Find Security Code

1. Go to your [SpamTools Dashboard](https://spamtools.org/dashboard)
2. Look for the **Security Code** in the top header area
3. Combine it with your Subscription Hash using a colon `:`

> Example: `R4nD0mS3cC0d3:abc123hashvalue`

---

## 📦 Example Payload

```json
{
  "securitycode_hash": "R4nD0mS3cC0d3:abc123hashvalue"
}
```

---

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

---

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

---

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

---

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

---

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

---

## 📌 Notes

- This endpoint **requires both** a valid security code and an active subscription hash.
- Data is grouped by session, and multiple entries will appear as newline-separated strings.
- Use this endpoint to pull results for reporting, analysis, or dashboard integration.

---

## 🔚 That’s It!

You’ve now learned how to:

- ✅ Authenticate your hash  
- ✅ Add data from phishing kits or loggers  
- ✅ Securely fetch results with your security code

Need help? Reach out to us on Telegram: [@spamtoolsorg](https://t.me/spamtoolsorg)
