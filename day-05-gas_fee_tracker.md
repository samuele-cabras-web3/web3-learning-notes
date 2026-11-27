## Grouped Error Handling (`RequestException`) vs Specific Exceptions

In earlier scripts, we caught specific errors like `requests.exceptions.Timeout` or checked status codes manually with `if`. When building continuous scripts (like a polling tracker), we can group all request-related errors into a single `except` block:

* **`requests.exceptions.RequestException` (The Parent Class):** This is the base exception for the entire `requests` library. Catching it handles **all** connection issues (no internet, DNS failure, timeout) **and** HTTP errors (`404`, `429`, `500`) in one place, provided you call `response.raise_for_status()`.
* **`KeyboardInterrupt` (Graceful Exit):** Triggered when the user stops the script manually from the terminal using `Ctrl + C`. Catching it prevents ugly crash tracebacks and lets the program exit cleanly.

### Practical Code Example

```python
import requests
import time

url = "https://example.com"

try:
while True:
try:
response = requests.get(url, timeout=10)
# Converts HTTP error codes (4xx, 5xx) into an exception
response.raise_for_status()
data = response.json()
print("Data fetched successfully!")

# Catches Timeout, ConnectionError, and HTTPError all together
except requests.exceptions.RequestException as e:
print(f"Network or HTTP error: {e}")

time.sleep(15)

# Catches Ctrl+C outside the loop to stop the script safely
except KeyboardInterrupt:
print("\nPolling stopped safely by user.")
```
