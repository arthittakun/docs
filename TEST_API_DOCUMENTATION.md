# Test API Documentation

เอกสารการใช้งาน Test API Endpoints สำหรับทดสอบระบบ

## Base URL
```
http://api.mefarmhug.com/api/ai/test
```

---

## 📋 Endpoints

### 1. Health Check
ตรวจสอบสถานะการทำงานของ API

**Endpoint:** `GET /api/ai/test/`

**Authentication:** ไม่ต้องการ

**Request:**
```bash
GET http://api.mefarmhug.com/api/ai/test/
```

**Response:**
```json
{
  "status": "success",
  "message": "Test API is running",
  "data": {
    "timestamp": "2025-11-26T10:30:00.123456",
    "service": "mefarm-ai",
    "version": "1.0.0"
  }
}
```

---

### 2. Generate Token
สร้าง WebSocket token สำหรับเชื่อมต่อ

**Endpoint:** `POST /api/ai/test/token`

**Authentication:** ✅ ต้องการ (Bearer Token)

**Headers:**
```
Authorization: Bearer <your_access_token>
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| FarmUserPlotId | string | ✅ | ID ของแปลงที่ต้องการเชื่อมต่อ |

**Request Example:**
```bash
POST http://api.mefarmhug.com/api/ai/test/token?FarmUserPlotId=0c358862-8c2c-47d2-b80e-c2527e28f2e5
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**cURL Example:**
```bash
curl -X POST "http://api.mefarmhug.com/api/ai/test/token?FarmUserPlotId=0c358862-8c2c-47d2-b80e-c2527e28f2e5" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

**Response (Success):**
```json
{
  "status": "success",
  "message": "Token generated",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "room_id": "room_abc123xyz",
    "FarmUserPlotId": "0c358862-8c2c-47d2-b80e-c2527e28f2e5",
    "user_id": "user123"
  }
}
```

**Response (Failed):**
```json
{
  "status": "failed",
  "message": "Cannot create connection"
}
```

**Error Cases:**
- `401 Unauthorized`: Token หมดอายุหรือไม่ถูกต้อง
- `404 Not Found`: ไม่พบ FarmUserPlotId
- `500 Internal Server Error`: เกิดข้อผิดพลาดในระบบ

---

### 3. Get Mock Product Data
ดึงข้อมูลสินค้าตัวอย่างสำหรับทดสอบ

**Endpoint:** `GET /api/ai/test/product`

**Authentication:** ไม่ต้องการ

**Request:**
```bash
GET http://api.mefarmhug.com/api/ai/test/product
```

**cURL Example:**
```bash
curl -X GET "http://api.mefarmhug.com/api/ai/test/product"
```

**Response:**
```json
{
  "status": "success",
  "message": "Mock product data retrieved",
  "data": [
    {
      "id": "1",
      "name": "ข้าวหอมมะลิ",
      "category": "พืชไร่",
      "price": 25.50,
      "unit": "กิโลกรัม",
      "stock": 1500,
      "description": "ข้าวหอมมะลิคุณภาพดี เกรด A",
      "image_url": "https://example.com/rice.jpg"
    },
    {
      "id": "2",
      "name": "มะเขือเทศ",
      "category": "ผัก",
      "price": 35.00,
      "unit": "กิโลกรัม",
      "stock": 500,
      "description": "มะเขือเทศสด ปลอดสารพิษ",
      "image_url": "https://example.com/tomato.jpg"
    },
    {
      "id": "3",
      "name": "กล้วยหอม",
      "category": "ผลไม้",
      "price": 45.00,
      "unit": "หวี",
      "stock": 200,
      "description": "กล้วยหอมสุก พร้อมรับประทาน",
      "image_url": "https://example.com/banana.jpg"
    },
    {
      "id": "4",
      "name": "ไข่ไก่",
      "category": "ปศุสัตว์",
      "price": 5.00,
      "unit": "ฟอง",
      "stock": 3000,
      "description": "ไข่ไก่สดใหม่ทุกวัน",
      "image_url": "https://example.com/egg.jpg"
    },
    {
      "id": "5",
      "name": "ปุ๋ยอินทรีย์",
      "category": "ปุ๋ย",
      "price": 150.00,
      "unit": "กระสอบ",
      "stock": 100,
      "description": "ปุ๋ยอินทรีย์คุณภาพสูง 25 กก./กระสอบ",
      "image_url": "https://example.com/fertilizer.jpg"
    }
  ]
}
```

**Product Fields:**
| Field | Type | Description |
|-------|------|-------------|
| id | string | รหัสสินค้า |
| name | string | ชื่อสินค้า |
| category | string | หมวดหมู่ (พืชไร่, ผัก, ผลไม้, ปศุสัตว์, ปุ๋ย) |
| price | number | ราคาต่อหน่วย |
| unit | string | หน่วยนับ |
| stock | number | จำนวนคงเหลือ |
| description | string | รายละเอียดสินค้า |
| image_url | string | URL รูปภาพ |

---

## 🔐 Authentication

สำหรับ endpoints ที่ต้องการ authentication:

1. เรียก API เพื่อขอ token ก่อน (จาก `/api/ai/auth/login` หรือ endpoint อื่นๆ)
2. ใส่ token ใน Header:
   ```
   Authorization: Bearer <your_token>
   ```

---

## 📝 ตัวอย่างการใช้งาน

### ตัวอย่างที่ 1: ตรวจสอบ API
```bash
# ตรวจสอบว่า API ทำงานหรือไม่
curl http://api.mefarmhug.com/api/ai/test/
```

### ตัวอย่างที่ 2: สร้าง Token
```bash
# สร้าง token สำหรับ WebSocket
curl -X POST "http://api.mefarmhug.com/api/ai/test/token?FarmUserPlotId=abc123" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### ตัวอย่างที่ 3: ดึงข้อมูลสินค้า
```bash
# ดึงข้อมูลสินค้าทั้งหมด
curl http://api.mefarmhug.com/api/ai/test/product
```

---

## 🧪 Testing with Postman

### Setup:
1. สร้าง Collection ใหม่ชื่อ "MeFarm Test API"
2. ตั้งค่า Environment Variable:
   - `base_url`: `http://api.mefarmhug.com`
   - `access_token`: `<your_token>`

### Requests:

**1. Health Check**
- Method: GET
- URL: `{{base_url}}/api/ai/test/`

**2. Generate Token**
- Method: POST
- URL: `{{base_url}}/api/ai/test/token?FarmUserPlotId=abc123`
- Headers: `Authorization: Bearer {{access_token}}`

**3. Get Products**
- Method: GET
- URL: `{{base_url}}/api/ai/test/product`

---

## ⚠️ หมายเหตุ

- API นี้ใช้สำหรับการทดสอบเท่านั้น
- ข้อมูลสินค้าเป็น Mock data ไม่ใช่ข้อมูลจริง
- Token ที่สร้างจาก `/token` endpoint สามารถใช้ทดสอบ WebSocket ได้
- สำหรับ Production ให้ใช้ `/api/ai/auth/gettoken-websocket` แทน

---

## 📞 Support

หากมีปัญหาหรือข้อสงสัย กรุณาติดต่อทีมพัฒนา

