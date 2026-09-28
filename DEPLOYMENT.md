# Thông Tin Deploy - Checkpoint 5

## Thông Tin Học Viên

| Mục | Nội dung |
|-----|----------|
| Họ và tên | Lê Hoàng Thiên Phú |
| Mã học viên | 2A202602908 |
| Repo | https://github.com/thienphu7/K4-L3A-DAY12-LeHoangThienPhu-2A202602908-CloudServiceAndDeployment |

## Service

| Mục | Nội dung |
|-----|----------|
| Public URL | https://k4-l3a-day12-lehoangthienphu-2a202602908-cloudse-production.up.railway.app |
| Platform | Railway |
| Ngày deploy | 2026-09-28 |

## Biến Môi Trường Đã Set Trên Cloud

Chỉ liệt kê tên biến và nguồn giá trị, không ghi giá trị secret.

| Biến | Đã set | Ghi chú |
|------|--------|---------|
| `PORT` | Có | Railway tự gán, không đặt thủ công |
| `AGENT_API_KEY` | Có | Đặt trong Railway Variables, không nằm trong repo |
| `REDIS_URL` | Có | Railway Redis service |
| `RATE_LIMIT_PER_MINUTE` | Có | Đặt trong Railway Variables |
| `MONTHLY_BUDGET_USD` | Có | Đặt trong Railway Variables |
| `LOG_LEVEL` | Có | Đặt trong Railway Variables |

## Lệnh Kiểm Tra

```powershell
$URL="https://k4-l3a-day12-lehoangthienphu-2a202602908-cloudse-production.up.railway.app"

curl.exe -i "$URL/health"
curl.exe -i "$URL/ready"

$body = '{"question":"Hello"}'
curl.exe -i -X POST "$URL/ask" -H "Content-Type: application/json" --data-binary $body

$KEY="<AGENT_API_KEY đã set trong Railway Variables>"
Invoke-WebRequest -Method POST "$URL/ask" `
  -ContentType "application/json" `
  -Headers @{"X-API-Key"=$KEY; "X-User-Id"="sv01"} `
  -Body '{"question":"Docker la gi?"}'
```

## Kết Quả Chạy Thật

```text
GET /health
HTTP/1.1 200 OK
{"status":"ok","service":"day12-agent","version":"1.0.0"}

GET /ready
HTTP/1.1 200 OK
{"status":"ready","redis":true}

POST /ask không có API key
HTTP/1.1 401 Unauthorized

POST /ask có X-API-Key và X-User-Id
HTTP/1.1 200 OK
Response có đủ các trường: answer, user_id, history_length, cost_usd và tokens.
```

## Ảnh Chụp Màn Hình

Ảnh minh chứng đã được lưu trong thư mục `screenshots/`:

- `screenshots/dashboard.png` - Trang Railway hiển thị service đang active, deployment successful và public URL.
- `screenshots/health.png` - Kết quả gọi endpoint `/health` trả `200 OK`.

## Ghi Chú

Service đã được deploy trên Railway bằng Dockerfile. Redis chạy bằng Railway Redis và ứng dụng kết nối qua biến `REDIS_URL`. Giá trị `AGENT_API_KEY` không được ghi vào repository.
