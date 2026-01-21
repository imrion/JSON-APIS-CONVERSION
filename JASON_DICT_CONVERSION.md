# 📌 JSON Functions in Python

এই README ফাইলে Python এর `json` module এর সবচেয়ে গুরুত্বপূর্ণ ৪টি function সহজভাবে ব্যাখ্যা করা হয়েছে।
এই ডকুমেন্টটি **Beginner Friendly**, **Exam Ready** এবং **GitHub Ready**।

---

## 🔹 Functions List

* `json.dump()`
* `json.dumps()`
* `json.load()`
* `json.loads()`

---

## 🔹 1. json.dump()

### ✅ কী করে?

Python object (dict) কে সরাসরি **JSON file** এ লিখে।

### 📌 Syntax

```python
json.dump(data, file_object)
```

### 📘 Example

```python
import json

data = {
    "name": "Imran",
    "age": 22
}

with open("info.json", "w") as f:
    json.dump(data, f)
```

### 🧠 Use Case

* JSON file এ data save করা
* Config file তৈরি করা

---

## 🔹 2. json.dumps()

### ✅ কী করে?

Python object (dict) কে **JSON string** এ convert করে।

### 📌 Syntax

```python
json_string = json.dumps(data)
```

### 📘 Example

```python
import json

data = {
    "name": "Imran",
    "age": 22
}

json_text = json.dumps(data)
print(json_text)
```

### 🧠 Use Case

* Console এ JSON print করা
* API request পাঠানো

---

## 🔹 3. json.load()

### ✅ কী করে?

**JSON file** থেকে data পড়ে Python object (dict) বানায়।

### 📌 Syntax

```python
data = json.load(file_object)
```

### 📘 Example

```python
import json

with open("info.json", "r") as f:
    data = json.load(f)

print(data)
```

### 🧠 Use Case

* JSON file থেকে data read করা
* Saved configuration load করা

---

## 🔹 4. json.loads()

### ✅ কী করে?

**JSON string** থেকে data পড়ে Python object (dict) বানায়।

### 📌 Syntax

```python
data = json.loads(json_string)
```

### 📘 Example

```python
import json

json_text = '{"name": "Imran", "age": 22}'

data = json.loads(json_text)
print(data)
```

### 🧠 Use Case

* API response process করা
* JSON string handle করা

---

## 🔄 Complete Comparison Table

| Function | Input       | Output      | Use Case       |
| -------- | ----------- | ----------- | -------------- |
| dump     | dict + file | JSON file   | Save to disk   |
| dumps    | dict        | JSON string | Print / API    |
| load     | JSON file   | dict        | Read from file |
| loads    | JSON string | dict        | API response   |

---

## 🧠 Memory Trick

```
dump   → file
dumps  → string
load   → file
loads  → string
```

---

## ✍️ One Line Summary

> `dump()` ও `load()` file নিয়ে কাজ করে, আর `dumps()` ও `loads()` string নিয়ে কাজ করে।

---

## 📎 Author

**Md Imran Hossain**
Python Learner | Software Engineering Aspirant

---

⭐ যদি এই repository টি কাজে লাগে, তাহলে **Star** দিতে ভুলবেন না!
