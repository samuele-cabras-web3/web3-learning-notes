# Environment Variables, Secret Management & Time Module Notes

## 2026-11-23 — Protecting API Keys with `.env` and Controlling Loops with `time`

* **What I Learned:** How to keep sensitive credentials (like API keys) out of public GitHub repositories using `.env` files and `python-dotenv`, and how to pace automated API requests using `time.sleep()` to avoid rate limits.

## Core Security Concepts (Environment Variables)

* **Hardcoding Secrets (Bad Practice):** Writing API keys or private keys directly inside `.py` files exposes them to anyone who views the repository on GitHub.
* **`.env` File:** A hidden local configuration file that stores sensitive key-value pairs (e.g., `ETHERSCAN_API_KEY=your_secret_key`). It stays only on your local machine.
* **`.gitignore`:** A special Git file that tells Git which files or folders to ignore. Adding `.env` to `.gitignore` guarantees your secrets are never pushed to GitHub.
* **`os.getenv("KEY_NAME")`:** A built-in Python function from the `os` module that retrieves the value of an environment variable. It returns `str` if found, or `None` if missing (`str | None`).

## How to Set Up and Load API Keys (3-Step Rule)

1. **Install the Library:** Run `pip install python-dotenv` in the terminal so Python can read `.env` files.
2. **Create `.env` & Update `.gitignore`:** Create a file named `.env` in the root folder with `API_KEY=value`, then add `.env` on a new line inside `.gitignore`.
3. **Load in Python:** Call `load_dotenv()` at the top of the script before reading variables with `os.getenv()`.

## Controlling Execution with `time` & Loop Exceptions

* **`time.sleep(seconds)`:** Pauses program execution for a specific number of seconds (accepts `int` or `float`, e.g., `time.sleep(10)`). Crucial in `while True` loops to respect API rate limits (avoiding HTTP `429 Too Many Requests`).
* **`KeyboardInterrupt` (`try/except`):** Triggered when the user stops an infinite loop manually in the terminal by pressing `Ctrl + C`. Catching it allows the script to exit cleanly without crashing with an ugly traceback error.

## Practical Code Example

```python
import os
import time
from dotenv import load_dotenv

# Load variables from the local .env file into the environment
load_dotenv()

def get_api_key() -> str:
api_key: str | None = os.getenv("ETHERSCAN_API_KEY")
if api_key is None:
raise ValueError("Missing ETHERSCAN_API_KEY in .env file")
return api_key

def start_polling(interval: int) -> None:
key: str = get_api_key()
print(f"API Key loaded successfully (starts with: {key[:4]}...)")

try:
while True:
print("Fetching latest blockchain data...")
# requests.get(...) will go here in Project 5
time.sleep(interval)
except KeyboardInterrupt:
print("\nPolling stopped safely by user.")

start_polling(5)
```
