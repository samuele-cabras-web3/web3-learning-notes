# Python Type Hints & HTTP Error Handling Notes

## 2026-11-22 — Function Type Hints, Tuples & Network Exceptions vs Status Codes

* **What I Learned:** How to deduce input and return type hints (`int`, `float`, `str`, `tuple`, `None`) by reading what enters and exits a function, and the exact difference between network exceptions (`Timeout`) and HTTP status codes (`404`, `429`).

### Core Python Data Types (Type Hinting)

* **`int`:** Whole numbers that naturally cannot have decimals (e.g., `status_code: int`, `retries: int`, `block_number: int`).
* **`float`:** Numbers that can have decimals, also used as the standard for general math operations (e.g., `price: float`).
* **`str`:** Text enclosed in quotes, including formatted numbers with symbols (e.g., `coin: str`, `"$3250.50"`).
* **`bool`:** Logical values (`True` or `False`).
* **`tuple[type1, type2]`:** Used when a function returns multiple values separated by a comma (e.g., `return price, change` -> `tuple[float, float]`).
* **Union Operator (`|`):** Means "OR" and is used when a function can accept or return more than one type (e.g., `float | None` when a function returns a price or `None` on failure).

### How to Deduce Function Type Hints (3-Step Rule)

1. **Check the Parameters `(param: type)`:** Look at how the variable is used inside the function (e.g., `if price >= 1:` means `price` is a number -> `price: float`).
2. **Check the `return` Statement `(-> type)`:** Look directly at what comes after `return` (e.g., `return f"${price:.2f}"` has quotes -> `-> str`).
3. **Check for Multiple Outputs:** If `return` outputs two values separated by a comma, wrap their types inside `tuple[...]`.

### Network Exceptions (`try/except`) vs HTTP Status Codes (`if`)

* **Network Exceptions (e.g., `requests.exceptions.Timeout`):** Happen when the server never replies within the time limit (e.g., `timeout=10`). No HTTP status code is received; `requests` cuts the connection and Python jumps directly to the `except` block.
* **HTTP Status Codes (e.g., `404`, `429`):** Happen when the server **does** reply in time, but reports an issue (such as `404 Not Found` or `429 Too Many Requests`). These do not trigger `except` and must be checked with `if response.status_code == ...`.

### Practical Code Example

```python
import requests

# 1. Function with clear input and return types
def format_price(price: float) -> str:
if price >= 1:
return f"\${price:.2f}"
return f"\${price:.6f}"

# 2. Function returning a tuple or None on failure
def fetch_price(url: str) -> tuple[float, float] | None:
try:
response = requests.get(url, timeout=10)
if response.status_code == 429:
print("Rate limit exceeded. Wait before retrying.")
return None
if response.status_code == 200:
data = response.json()
return data["price"], data["change"]
return None
except requests.exceptions.Timeout:
print("Request timed out: no response from server.")
return None
```
