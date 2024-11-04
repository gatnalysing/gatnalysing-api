# Guide to API Endpoints with `curl` Examples

This guide provides examples of how to interact with the `intrapi.py` API using `curl`. It covers essential endpoints, including turning devices on or off, setting modes, retrieving device states, and getting astro event information.

**Note:**

- Replace `localhost:5053` with your server's IP address or hostname and port where the `intrapi.py` service is running.
- Replace `{identifier}` with the device's `id` or `hs` value (with underscores `_` replaced by dashes `-` to make it URL-compliant).
- In this guide, we will use `storhofdi-7` as the example device identifier.
- **Important:** All API requests require authentication using the `CF-Access-Client-Id` and `CF-Access-Client-Secret` headers. Ensure you include these headers in all your requests.

---

## Table of Contents

1. [Authentication Headers](#1-authentication-headers)
2. [Turn ON](#2-turn-on)
3. [Turn OFF](#3-turn-off)
4. [Set Manual Mode](#4-set-manual-mode)
5. [Set Astro Mode](#5-set-astro-mode)
6. [Get Output State](#6-get-output-state)
7. [Get Device State](#7-get-device-state)
8. [Get Next Astro Event](#8-get-next-astro-event)
9. [Get Last Astro Event](#9-get-last-astro-event)
10. [Get All Devices](#10-get-all-devices)
11. [Get a Single Device Field](#11-get-a-single-device-field)
12. [Additional Notes](#12-additional-notes)

---

## 1. Authentication Headers

**Description:**

All API endpoints require authentication using the headers `CF-Access-Client-Id` and `CF-Access-Client-Secret`. These credentials are read from a file named `secret` on the server and should be securely provided to authorized clients.

**Example Headers:**

```bash
-H "CF-Access-Client-Id: your_id_here" \
-H "CF-Access-Client-Secret: your_secret_here"
```

**Replace:**

- `your_id_here` with the `Id` value from your `secret` file.
- `your_secret_here` with the `Secret` value from your `secret` file.

---

## 2. Turn ON

**Endpoint:**

```
POST /devices/{identifier}/state
```

**Description:**

Turns the specified device **ON**. The device must be in `MANUAL` mode (`astroman` set to `MANUAL`).

**Example `curl` Command:**

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"state": "ON"}' \
     http://localhost:5053/devices/storhofdi-7/state
```

**Expected Response (Success):**

```json
{
  "message": "Device storhofdi-7 turned ON successfully"
}
```

**Note:** If the device is not in `MANUAL` mode, you will receive an error message. Make sure to set the device to `MANUAL` mode first.

---

## 3. Turn OFF

**Endpoint:**

```
POST /devices/{identifier}/state
```

**Description:**

Turns the specified device **OFF**. The device must be in `MANUAL` mode.

**Example `curl` Command:**

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"state": "OFF"}' \
     http://localhost:5053/devices/storhofdi-7/state
```

**Expected Response (Success):**

```json
{
  "message": "Device storhofdi-7 turned OFF successfully"
}
```

---

## 4. Set Manual Mode

**Endpoint:**

```
POST /devices/{identifier}/astroman
```

**Description:**

Sets the device's mode to `MANUAL`, allowing manual control over the device's state.

**Example `curl` Command:**

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"astroman": "MANUAL"}' \
     http://localhost:5053/devices/storhofdi-7/astroman
```

**Expected Response:**

```json
{
  "message": "Device storhofdi-7 astroman set to MANUAL"
}
```

---

## 5. Set Astro Mode

**Endpoint:**

```
POST /devices/{identifier}/astroman
```

**Description:**

Sets the device's mode to `ASTRO`, allowing automatic control based on astro events.

**Example `curl` Command:**

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"astroman": "ASTRO"}' \
     http://localhost:5053/devices/storhofdi-7/astroman
```

**Expected Response:**

```json
{
  "message": "Device storhofdi-7 astroman set to ASTRO"
}
```

---

## 6. Get Output State

**Endpoint:**

```
GET /devices/{identifier}/fields/outputstate
```

**Description:**

Retrieves the current `outputstate` of the device (`ON` or `OFF`).

**Example `curl` Command:**

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     http://localhost:5053/devices/storhofdi-7/fields/outputstate
```

**Expected Response:**

```json
{
  "outputstate": "ON"
}
```

---

## 7. Get Device State

**Endpoint:**

```
GET /devices/{identifier}/state
```

**Description:**

Retrieves the device's current `outputstate` and `astroman` mode.

**Example `curl` Command:**

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     http://localhost:5053/devices/storhofdi-7/state
```

**Expected Response:**

```json
{
  "id": "storhofdi-7",
  "state": "ON",
  "astroman": "MANUAL"
}
```

---

## 8. Get Next Astro Event

**Endpoint:**

```
GET /devices/{identifier}/nextastro
```

**Description:**

Retrieves the device's next astro event time and operation.

**Example `curl` Command:**

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     http://localhost:5053/devices/storhofdi-7/nextastro
```

**Expected Response:**

```json
{
  "id": "storhofdi-7",
  "nextastrotime": "2024-11-04T16:58:12.450944",
  "nextastroOP": "ON"
}
```

---

## 9. Get Last Astro Event

**Endpoint:**

```
GET /devices/{identifier}/lastastro
```

**Description:**

Retrieves the device's last astro event time and operation.

**Example `curl` Command:**

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     http://localhost:5053/devices/storhofdi-7/lastastro
```

**Expected Response:**

```json
{
  "id": "storhofdi-7",
  "lastastrotime": "2024-11-04T09:22:01.722795",
  "lasastroOP": "OFF"
}
```

---

## 10. Get All Devices

**Endpoint:**

```
GET /devices
```

**Description:**

Retrieves a list of all devices in the database.

**Example `curl` Command:**

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     http://localhost:5053/devices
```

**Expected Response:**

A JSON array containing all devices, each represented as a JSON object.

---

## 11. Get a Single Device Field

**Endpoint:**

```
GET /devices/{identifier}/fields/{field_name}
```

**Description:**

Retrieves the value of a specific field for the specified device.

**Example `curl` Command:**

Get the `heimilisfang` (address) field of the device:

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     http://localhost:5053/devices/storhofdi-7/fields/heimilisfang
```

**Expected Response:**

```json
{
  "heimilisfang": "Stórhöfði 7"
}
```

---

## 12. Additional Notes

- **Device Identifiers:**

  - Use the device's `id` or `hs` value as `{identifier}`.
  - When using the `hs` value, replace underscores `_` with dashes `-` to make it URL-compliant.
  - For example, if the `hs` value is `845_1`, you would use `845-1` as the identifier.

- **Setting Device State:**

  - Before changing the device state to `ON` or `OFF`, ensure the device's `astroman` mode is set to `MANUAL`.

- **Content-Type Header:**

  - When making `POST` requests with JSON data, include the `Content-Type: application/json` header.

- **Authentication Headers:**

  - All requests must include the `CF-Access-Client-Id` and `CF-Access-Client-Secret` headers with the correct credentials.

- **Error Handling:**

  - The API returns appropriate HTTP status codes and error messages for various error conditions.
  - Common errors include:

    - **400 Bad Request:** Missing or invalid parameters.
    - **401 Unauthorized:** Missing or incorrect authentication headers.
    - **403 Forbidden:** Action not allowed (e.g., changing state when `astroman` is not `MANUAL`).
    - **404 Not Found:** Device or field not found.
    - **500 Internal Server Error:** Server encountered an unexpected condition.

- **Available Fields:**

  - To see all available fields for a device, retrieve the full device information:

    ```bash
    curl -H "CF-Access-Client-Id: your_id_here" \
         -H "CF-Access-Client-Secret: your_secret_here" \
         http://localhost:5053/devices/storhofdi-7
    ```

- **Character Encoding:**

  - The API returns JSON responses encoded in UTF-8. Non-ASCII characters (e.g., Icelandic characters) are displayed correctly.

---

### Example Workflow for `storhofdi-7`

1. **Set Device to Manual Mode:**

   ```bash
   curl -X POST -H "Content-Type: application/json" \
        -H "CF-Access-Client-Id: your_id_here" \
        -H "CF-Access-Client-Secret: your_secret_here" \
        -d '{"astroman": "MANUAL"}' \
        http://localhost:5053/devices/storhofdi-7/astroman
   ```

2. **Turn Device ON:**

   ```bash
   curl -X POST -H "Content-Type: application/json" \
        -H "CF-Access-Client-Id: your_id_here" \
        -H "CF-Access-Client-Secret: your_secret_here" \
        -d '{"state": "ON"}' \
        http://localhost:5053/devices/storhofdi-7/state
   ```

3. **Check Device State:**

   ```bash
   curl -H "CF-Access-Client-Id: your_id_here" \
        -H "CF-Access-Client-Secret: your_secret_here" \
        http://localhost:5053/devices/storhofdi-7/state
   ```

   **Expected Response:**

   ```json
   {
     "id": "storhofdi-7",
     "state": "ON",
     "astroman": "MANUAL"
   }
   ```

4. **Get Output State:**

   ```bash
   curl -H "CF-Access-Client-Id: your_id_here" \
        -H "CF-Access-Client-Secret: your_secret_here" \
        http://localhost:5053/devices/storhofdi-7/fields/outputstate
   ```

   **Expected Response:**

   ```json
   {
     "outputstate": "ON"
   }
   ```

5. **Get Next Astro Event:**

   ```bash
   curl -H "CF-Access-Client-Id: your_id_here" \
        -H "CF-Access-Client-Secret: your_secret_here" \
        http://localhost:5053/devices/storhofdi-7/nextastro
   ```

   **Expected Response:**

   ```json
   {
     "id": "storhofdi-7",
     "nextastrotime": "2024-11-04T16:58:12.450944",
     "nextastroOP": "ON"
   }
   ```

6. **Get Last Astro Event:**

   ```bash
   curl -H "CF-Access-Client-Id: your_id_here" \
        -H "CF-Access-Client-Secret: your_secret_here" \
        http://localhost:5053/devices/storhofdi-7/lastastro
   ```

   **Expected Response:**

   ```json
   {
     "id": "storhofdi-7",
     "lastastrotime": "2024-11-04T09:22:01.722795",
     "lasastroOP": "OFF"
   }
   ```

7. **Set Device Back to Astro Mode:**

   ```bash
   curl -X POST -H "Content-Type: application/json" \
        -H "CF-Access-Client-Id: your_id_here" \
        -H "CF-Access-Client-Secret: your_secret_here" \
        -d '{"astroman": "ASTRO"}' \
        http://localhost:5053/devices/storhofdi-7/astroman
   ```

---

This guide provides the essential commands for interacting with the `intrapi.py` API using `curl`, using `storhofdi-7` as the example device. By following the examples and including the necessary authentication headers, you can control devices, retrieve their states, and access astro event information securely.

---

## Corrections and Notes:

- **Authentication Requirement:**

  - All endpoints now require authentication using `CF-Access-Client-Id` and `CF-Access-Client-Secret` headers due to the implementation of the authentication decorator in `intrapi.py`.

- **Error Handling Update:**

  - The API now returns a **401 Unauthorized** error if the authentication headers are missing or incorrect.

- **Consistency in Examples:**

  - All `curl` examples have been updated to include the required authentication headers.
  - Ensured that the endpoint URLs and example responses match the current implementation.

- **Field Names:**

  - Double-checked field names such as `lasastroOP` in the `/lastastro` endpoint. If the correct field name is `lastastroOP`, make sure to update it in both the code and documentation.

- **Example Data:**

  - The data shown in expected responses (e.g., dates and times) are illustrative. Your actual data may vary.
