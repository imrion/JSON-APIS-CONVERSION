# GET Request ও POST Request — Learning Theory (API Perspective)

এই ডকুমেন্টটি **CSE student ও API learner**দের জন্য লেখা। এখানে **GET ও POST request**–এর তত্ত্ব (theory), ব্যবহার, নিরাপত্তা (security), এবং **attacker–defender mindset** একসাথে ব্যাখ্যা করা হয়েছে—যাতে বাস্তব সিস্টেম (যেমন social media, ERP, banking API) বোঝা সহজ হয়।

---

## 1. HTTP Request কী?

HTTP request হলো **client → server** যোগাযোগের নিয়ম।
Client (browser / app / script) সার্ভারকে বলে:

* কী চাই (method)
* কোথা থেকে (URL)
* কী ডাটা (headers/body)

Server সিদ্ধান্ত নেয়:

* Accept / Reject
* Response কী হবে

---

## 2. GET Request — Theory

### সংজ্ঞা

**GET request** ব্যবহার হয় **server থেকে ডাটা আনার জন্য**।
এটি সাধারণত **read-only** অপারেশন।

### বৈশিষ্ট্য

* ডাটা যায় **URL (query string)** দিয়ে
* Server-এর state পরিবর্তন করে না
* Cache করা যায়
* Bookmark করা যায়

### উদাহরণ (Conceptual)

```
GET /posts?userId=1
```

অর্থ: “userId=1 এর posts দাও”

### Python Code Example (GET)

```python
import requests

url = "http://localhost:5000/posts"
params = {
    "userId": 1
}

response = requests.get(url, params=params)

print(response.status_code)
print(response.json())
```

### কোথায় ব্যবহার হয়

* Post list দেখা
* Product list
* Profile data fetch
* Report view

### সংজ্ঞা

**GET request** ব্যবহার হয় **server থেকে ডাটা আনার জন্য**।
এটি সাধারণত **read-only** অপারেশন।

### বৈশিষ্ট্য

* ডাটা যায় **URL (query string)** দিয়ে
* Server-এর state পরিবর্তন করে না
* Cache করা যায়
* Bookmark করা যায়

### উদাহরণ (Conceptual)

```
GET /posts?userId=1
```

অর্থ: “userId=1 এর posts দাও”

### কোথায় ব্যবহার হয়

* Post list দেখা
* Product list
* Profile data fetch
* Report view

---

## 3. POST Request — Theory

### সংজ্ঞা

**POST request** ব্যবহার হয় **server-এ নতুন ডাটা পাঠাতে বা পরিবর্তন করতে**।
এটি **state-changing** অপারেশন।

### বৈশিষ্ট্য

* ডাটা যায় **request body** তে
* URL পরিষ্কার থাকে
* Cache করা যায় না
* Security তুলনামূলক বেশি

### উদাহরণ (Conceptual)

```
POST /react
Body:
{ user_id, post_id, reaction }
```

অর্থ: “এই user এই post-এ react দিতে চায়”

### Python Code Example (POST)

```python
import requests

url = "http://localhost:5000/react"

data = {
    "user_id": 1,
    "post_id": 10,
    "reaction": "LIKE"
}

response = requests.post(url, json=data)

print(response.status_code)
print(response.json())
```

### কোথায় ব্যবহার হয়

* Login / Register
* Order create
* Payment submit
* Reaction / Comment

### সংজ্ঞা

**POST request** ব্যবহার হয় **server-এ নতুন ডাটা পাঠাতে বা পরিবর্তন করতে**।
এটি **state-changing** অপারেশন।

### বৈশিষ্ট্য

* ডাটা যায় **request body** তে
* URL পরিষ্কার থাকে
* Cache করা যায় না
* Security তুলনামূলক বেশি

### উদাহরণ (Conceptual)

```
POST /react
Body:
{ user_id, post_id, reaction }
```

অর্থ: “এই user এই post-এ react দিতে চায়”

### কোথায় ব্যবহার হয়

* Login / Register
* Order create
* Payment submit
* Reaction / Comment

---

## 4. GET vs POST — পার্থক্য টেবিল

| বিষয়         | GET       | POST               |
| ------------ | --------- | ------------------ |
| কাজ          | Data read | Data create/update |
| Data যায়     | URL       | Body               |
| Security     | কম        | বেশি               |
| Cache        | হয়        | হয় না              |
| State change | না        | হ্যাঁ              |

---

## 5. Client–Server Flow (Mechanism)

```
Client Action (Click / Submit)
        ↓
HTTP Request (GET/POST)
        ↓
Server Validation
        ↓
Business Logic
        ↓
Database Change (or Read)
        ↓
HTTP Response
```

### Simple Flask Server Example

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

reactions = []

@app.route('/react', methods=['POST'])
def react():
    data = request.json
    reactions.append(data)
    return jsonify({"message": "Reaction added", "total": len(reactions)})

if __name__ == '__main__':
    app.run(debug=True)
```

**গুরুত্বপূর্ণ কথা:**

> URL কিছু পরিবর্তন করে না—**server logic পরিবর্তন করে**।

```
Client Action (Click / Submit)
        ↓
HTTP Request (GET/POST)
        ↓
Server Validation
        ↓
Business Logic
        ↓
Database Change (or Read)
        ↓
HTTP Response
```

**গুরুত্বপূর্ণ কথা:**

> URL কিছু পরিবর্তন করে না—**server logic পরিবর্তন করে**।

---

## 6. Social Media Reaction — API View (Theory)

* React button click = **POST request**
* Server checks:

  * User authenticated?
  * One user → one reaction rule?
  * Behavior suspicious?
* Accept হলে:

  * Reaction save হয়
  * Count বাড়ে

👉 **Reaction count UI দিয়ে না, server সিদ্ধান্তে বাড়ে**।

---

## 7. Attacker Mindset (Theory Only)

Attacker ভাবে:

* “একটা action = একটা request”
* “বারবার পাঠালে কী হয়?”
* “Limit কোথায়?”

❌ এটা শেখা মানে attack করা না
✅ এটা বোঝা মানে **defense design শেখা**

---

## 8. Defender Mindset (Most Important)

Defender ভাবে:

* “এই POST misuse হলে কী হবে?”
* “GET endpoint থেকে sensitive data leak হচ্ছে?”
* “Behavior abnormal কিনা?”

### Defender Controls

* Rate limiting
* Authentication
* Authorization
* Behavior analysis

---

## 9. Common Security Concepts (GET/POST Related)

* **Authentication:** কে request পাঠাচ্ছে?
* **Authorization:** সে কি allowed?
* **Rate limit:** কতবার পারবে?
* **Validation:** input ঠিক আছে?
* **Idempotency:** একই request বারবার গেলে কী হবে?

---

## 10. Learning Takeaways

* GET = read
* POST = change
* URL = entry point
* Power = server logic
* Security = behavior + rules

এই তত্ত্ব বুঝলে তুমি:

* Secure API বানাতে পারবে
* Abuse detect করতে পারবে
* Large-scale system (Facebook/ERP/Bank) বুঝতে পারবে

---

## 11. Ethical Note

এই ডকুমেন্টটি **learning & defense** উদ্দেশ্যে।
কোনো real platform misuse করা উচিত না।

---

**Author:** Imran (CSE Student)
