# Kontrak API SisaBaik - Chapter 7

Spesifikasi endpoint REST API SisaBaik yang terintegrasi dengan PostgreSQL (`sisabaik_dev`).

---

## Daftar Endpoint

### **1. Health Check**
- **GET `/api/health`**
  - **Status Success**: `200 OK`
  - **Response**: `{ "success": true, "data": { "status": "ok", "database": "connected", "waktu": "..." } }`

---

### **2. Penawaran (`/api/penawaran`)**

- **GET `/api/penawaran`**
  - **Query Params**: `q`, `kategori`, `status`, `tersedia`, `page`, `limit`
  - **Status Success**: `200 OK`

- **GET `/api/penawaran/:id`**
  - **Path Param**: `id` (string digit).
  - **Status Success**: `200 OK`
  - **Status Fail**: `400` (ID invalid), `404` (Penawaran tidak ditemukan).

- **POST `/api/penawaran`**
  - **Header Required**: `Content-Type: application/json`
  - **Status Success**: `201 Created` (+ Header `Location: /api/penawaran/:id`)
  - **Status Fail**: `400`, `413`, `415`.

- **PUT `/api/penawaran/:id`**
  - **Status Success**: `200 OK`

- **DELETE `/api/penawaran/:id`**
  - **Status Success**: `204 No Content` (tanpa body).
  - **Status Fail**: `409 Conflict` (jika ada riwayat pesanan).

---

### **3. Pesanan (`/api/pesanan`)**

- **GET `/api/pesanan`**
  - **Query Params**: `page`, `limit`
  - **Status Success**: `200 OK`

- **GET `/api/pesanan/:id`**
  - **Status Success**: `200 OK`

- **POST `/api/pesanan`**
  - **Header Required**: `Content-Type: application/json`
  - **Body JSON**:
    ```json
    {
      "emailPembeli": "siti.pembeli@example.test",
      "items": [
        { "penawaranId": "1", "kuantitas": 2 }
      ]
    }
    ```
  - **Aturan Bisnis**: Single-provider, snapshot harga, row-locking stok.
  - **Status Success**: `201 Created` (+ Header `Location: /api/pesanan/:id`)
  - **Status Fail**: `400`, `404`, `409`.