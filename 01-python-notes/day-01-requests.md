# Python `requests` Notes

### 2026-11-19 — Anatomy of an API URL & HTTP Status Codes
- **What I learned:** How an API URL is structured and how to read HTTP response status codes.
- **Domain:** The main server address (e.g., `https://example.com`).
- **Path / Endpoint:** The specific resource or route on the server (e.g., `/v2/joke/Any`).
- **Query Parameters (`params`):** Key-value pairs added after `?` to filter or customize the request.

#### Common HTTP Status Codes
- **`200` (OK):** The request succeeded and the server returned the expected data.
- **`400` (Bad Request):** The server could not understand the request (invalid syntax or parameters).
- **`401` / `403` (Unauthorized / Forbidden):** Missing or invalid API key / permissions.
- **`404` (Not Found):** The requested endpoint or resource does not exist.
- **`429` (Too Many Requests):** Rate limit exceeded; need to wait before making more requests.
- **`500` (Internal Server Error):** Something went wrong on the server side.

  ### Request / Response Lifecycle (Client & Server)
- **How it works:** A Client (Frontend or Python script) sends an HTTP **Request** to a Backend/API Server, which processes it and sends back an HTTP **Response**.

#### Request Components (Client -> Server)
- **Type / Method:** The action to perform (`GET` to read data, `POST` to send data, `PUT`, `DELETE`).
- **Path:** The specific endpoint being requested (e.g., `/courses/python`).
- **Headers:** Extra metadata sent with the request (e.g., API keys, `Content-Type`).
- **Body:** The data payload sent to the server (used mainly in `POST` or `PUT` requests).

#### Response Components (Server -> Client)
- **Status Code:** Numeric code indicating the result (e.g., `200`, `404`).
- **Headers:** Metadata returned by the server (e.g., content format, rate limits).
- **Body:** The actual data returned by the server, usually in **JSON** format.


```python
import requests

url = "https://example.com/data"
params = {"limit": 5, "currency": "usd"}

response = requests.get(url, params=params, timeout=10)

if response.status_code == 200:
    print("Success:", response.json())
else:
    print(f"Error {response.status_code}: Something went wrong.")
