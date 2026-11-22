## 2026-11-21 — Parsing JSON with the `json` module

* **What I learned:** How to convert a raw API response (text string) into a navigable Python object using the built-in `json` module, and how to pretty-print the output to inspect its structure.

### The `json` module (built-in)
No need to install it via `pip`, just import it using `import json`.

* **`json.loads(string)` ("Load String"):** Converts a JSON string (e.g., `response.text`) into a Python **dictionary (`dict`)** or **list (`list`)**.
* *Note compared to last time:* `requests`'s `response.json()` does this automatically, whereas `json.loads()` is Python's standard function that works on any JSON string (even outside of `requests`).
* **`json.dumps(object, indent=2)` ("Dump String"):** Does the opposite—converts a Python dictionary/list back into a JSON string. Adding `indent=2` formats the JSON cleanly so it is readable in the terminal (very useful for inspecting nested responses).

### Extracting data (Parsing)
Once the JSON is loaded into a variable (e.g., `data = json.loads(response.text)`):

1. **Dictionaries `{}`:** Accessed via key → `data["key"]` or `data.get("key")` (safer, returns `None` instead of crashing with a `KeyError` if the key is missing).

* **Nested dictionaries (safe chaining):** Use `{}` as a fallback default (`data.get("a", {}).get("b")`) so it returns an empty dictionary instead of `None`, avoiding an `AttributeError` if an intermediate key is missing.

2. **Lists `[]`:** Accessed via index → `data[0]` or by iterating with a `for` loop:
```python
for item in data["results"]:
print(item["name"])
```

### Full practical example
```python
import requests
import json

response = requests.get("https://example.com")

# Convert the raw response text into a Python dict/list
data = json.loads(response.text)

# Pretty-print the JSON to inspect its structure (debugging)
print(json.dumps(data, indent=2))

# Extract a specific nested value
value = data["items"][0]["id"]
```
