# How to get start app to use
- cmd: pip install -r requirements.txt


<!-- ## การตั้งค่า google vertex 
# windows cmd
- ตัวอย่าง 
set GOOGLE_APPLICATION_CREDENTIALS=C:\Users\tuems\OneDrive\Desktop\mefarm.ai.service\me-farm-435601-c192a0f91496.json

เช็คว่าถูกต้องหรือไม่
echo %GOOGLE_APPLICATION_CREDENTIALS%

# linux
- ตัวอย่าง 
export GOOGLE_APPLICATION_CREDENTIALS="/path/to/service-account-key.json" -->


# run app
- cmd: uvicorn main:app --port 8108 --host 127.0.0.1 --reload ##ถ้าไม่ได้ตั้งค่า run ใน main ให้ใช้คำสั่งนี้
- cmd: python main.py ##ถ้าตั้งค่า run("main:app", host="127.0.0.1", port=8108 , reload=True) เเล้วให้ใช้คำสั่งนี้

# WebSocket API Usage

## การเชื่อมต่อ WebSocket
```
ws://127.0.0.1:8108/ws?token=your-token&room_id=your-room-id
```

## ประเภทข้อความที่รองรับ

### 1. ข้อความธรรมดา (AI Chat)
```json
{
  "type": "message",
  "prompt": "สวัสดีครับ",
  "image_data_list": [],
  "streaming": false
}
```

### 2. ข้อความที่มีรูปภาพ
```json
{
  "type": "message", 
  "prompt": "อธิบายรูปนี้หน่อย",
  "image_data_list": ["image-id-1", "image-id-2"],
  "streaming": false
}
```

### 3. ข้อความแบบ Streaming
```json
{
  "type": "message",
  "prompt": "เล่าเรื่องสั้นให้ฟัง",
  "image_data_list": ["image-id-1", "image-id-2"],
  "streaming": true
}
```



### 4. ดึงประวัติการแชท
```json
{
  "type": "getmessage",
  "page": 1,
  "page_size": 20
}
```

## ตัวอย่าง Response

### AI Response (ปกติ)
```json
{
  "status": "success",
  "type": "ai_response", 
  "message": "สวัสดีครับ! มีอะไรให้ช่วยไหม",
  "timestamp": "2025-07-02T10:30:00.123456",
  "model": {"client_id": "uuid", "room_id": "room-123"}
}
```
### AI Response (Streaming) ระหว่างตอบเเบบเรียลไทม์
```json
{
  "data" : "สวัสดี"
}
```
### AI Response (Streaming) เมื่อตอบเสร็จ
```json
{
    "status": "complete",
    "type": "ai_streaming_complete",
    "full_response": "(รอรูปภาพจากผู้ใช้)",
    "timestamp": "2025-07-02T19:45:39.649626",
    "model": {
        "room_id": "b1359ab7-b289-4935-b538-e9f365cea3b8"
    }
}
```

### Chat History Response
```json
{
  "status": "success",
  "type": "chat_history",
  "data": [
    {
      "id": "msg-1",
      "type": "user",
      "message": {
        "text": "สวัสดี",
        "image": []
      },
      "created_at": "2025-07-02T10:30:00.123456"
    }
  ],
  "pagination": {
    "current_page": 1,
    "page_size": 20,
    "total_count": 150,
    "total_pages": 8,
    "has_next": true,
    "has_previous": false
  }
}
```

### Error Response
```json
{
  "status": "error",
  "type": "validation_error",
  "message": "Invalid message format",
  "timestamp": "2025-07-02T10:30:00.123456"
}
```
