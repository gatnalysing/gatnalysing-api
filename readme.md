# Guide to API Endpoints with `curl` Examples

This guide provides examples for interacting with the `intrapi.py` API using `curl`. It includes endpoints for turning devices on or off, setting modes, retrieving device states, and accessing astro event information.

**Note:**

- Replace `api.gatnalysing.is` with your server's URL if different.
- Use the device's correct `id` or `hs` value, formatted as shown (e.g., underscores `_` replaced with dashes `-` for URL compliance).
- **Important:** All API requests require authentication headers: `CF-Access-Client-Id` and `CF-Access-Client-Secret`.

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

All API endpoints require authentication using the `CF-Access-Client-Id` and `CF-Access-Client-Secret` headers, with credentials stored in the `secret` file on the server.

**Example Headers:**

```bash
-H "CF-Access-Client-Id: your_id_here" \
-H "CF-Access-Client-Secret: your_secret_here"
```

Replace:
- `your_id_here` with the `Id` from your `secret` file.
- `your_secret_here` with the `Secret` from your `secret` file.

---

## 2. Turn ON

**Endpoint:**

```
POST /devices/{identifier}/state
```

Turns the specified device **ON**. The device must be in `MANUAL` mode.

**Example `curl` Command:**

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"state": "ON"}' \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/state
```

---

## 3. Turn OFF

**Endpoint:**

```
POST /devices/{identifier}/state
```

Turns the specified device **OFF**. The device must be in `MANUAL` mode.

**Example `curl` Command:**

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"state": "OFF"}' \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/state
```

---

## 4. Set Manual Mode

Sets the device to `MANUAL` mode, enabling manual control over its state.

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"astroman": "MANUAL"}' \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/astroman
```

---

## 5. Set Astro Mode

Enables automatic control based on astro events by setting `astroman` to `ASTRO`.

```bash
curl -X POST -H "Content-Type: application/json" \
     -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     -d '{"astroman": "ASTRO"}' \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/astroman
```

---

## 6. Get Output State

Retrieve the current `outputstate` of the device.

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/fields/outputstate
```

---

## 7. Get Device State

Retrieve the device's `outputstate` and `astroman` mode.

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/state
```

---

## 8. Get Next Astro Event

Retrieve the device's upcoming astro event time and operation.

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/nextastro
```

---

## 9. Get Last Astro Event

Retrieve the last astro event time and operation for the device.

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/lastastro
```

---

## 10. Get All Devices

Retrieve a list of all devices in the database.

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     https://api.gatnalysing.is/devices
```

---

## 11. Get a Single Device Field

Retrieve the value of a specific field for a device.

Example: Get the address field (`heimilisfang`) of the device.

```bash
curl -H "CF-Access-Client-Id: your_id_here" \
     -H "CF-Access-Client-Secret: your_secret_here" \
     https://api.gatnalysing.is/devices/hs845-storhofdi-7/fields/heimilisfang
```

---

## 12. Additional Notes

- **Device Identifiers:** Use the correct identifier from your database. Example: `hs845-storhofdi-7`.
- **Content-Type Header:** Include `Content-Type: application/json` for `POST` requests.
- **Authentication Headers:** Required for all requests.
- **Error Handling:** The API returns HTTP status codes such as:
  - **400** Bad Request
  - **401** Unauthorized
  - **403** Forbidden
  - **404** Not Found
  - **500** Internal Server Error
- **Available Fields:** Retrieve all fields by fetching the full device object.
