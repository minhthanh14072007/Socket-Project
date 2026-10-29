# P2P LAN Chat & File Transfer

Dự án ứng dụng trò chuyện (Chat) và gửi file trực tiếp theo mô hình Peer-to-Peer (P2P) trong mạng nội bộ (LAN) chạy trên giao diện Terminal/Console. Đồ án ứng dụng linh hoạt cả hai giao thức **UDP** và **TCP** cho các mục đích tối ưu khác nhau.

## 📌 Tính năng chính

* **Tự động tìm kiếm đồng cấp (Peer Discovery - UDP):** Tự động phát hiện các thiết bị đang hoạt động trong cùng mạng LAN thông qua UDP Broadcast.

* **Cập nhật trạng thái (Online Status - UDP):** Tự động xóa người dùng khỏi danh sách khi ngắt kết nối hoặc timeout (quá 10 giây không có tín hiệu). Cho phép chuyển trạng thái `online` / `away`.

* **Trò chuyện P2P (TCP Chat):** Nhắn tin trực tiếp giữa 2 máy đảm bảo tin nhắn không bị mất hoặc sai thứ tự.

* **Truyền tệp tin (TCP File Transfer):** Gửi/nhận file qua kết nối TCP riêng biệt với thanh tiến trình (progress bar), không làm gián đoạn luồng chat.

* **Giao diện Terminal thông minh:** Hỗ trợ vừa gõ tin nhắn vừa nhận tin nhắn/file mà không bị ghi đè hay vỡ dòng chữ đang nhập.

## 🛠️ Công nghệ sử dụng

* **Ngôn ngữ:** Python 3.x

* **API Mạng:** Native Python `socket` API (`AF_INET`, `SOCK_STREAM`, `SOCK_DGRAM`)

* **Đa luồng:** `threading` & `threading.Lock` để xử lý đồng thời nhiều kết nối và quản lý màn hình Console

* **Giao diện:** Python Terminal / Console Custom Redraw

## 📁 Sơ đồ cấu trúc thư mục

```
P2P_LAN_Chat/
│
├── main.py                 # Entry point chính, khởi tạo ứng dụng và vòng lặp giao diện
├── protocol.py             # Định nghĩa cấu trúc khung gói tin (MSG, FILE, HELLO, BYE)
├── state.py                # Quản lý trạng thái toàn cục (danh sách user, lock, unread)
│
├── udp_discovery.py        # Module quản lý UDP Broadcast discovery & presence status
├── tcp_chat.py             # Module quản lý kết nối TCP Chat Server & Client
├── tcp_file.py             # Module quản lý gửi/nhận File truyền qua TCP
│
├── ui_handler.py           # Quản lý hiển thị màn hình terminal, xử lý đè dòng
├── requirements.txt        # Thư viện phụ trợ (nếu có)
└── downloads/              # Thư mục mặc định chứa các tệp tin nhận được

```

## 👥 Phân công công việc (3 Thành viên)

| 

| **Thành viên** | **Nhiệm vụ phân công** | **Module phụ trách** | 
| **Thành viên 1** | *Chưa phân công* | `udp_discovery.py` | 
| **Thành viên 2** | *Chưa phân công* | `tcp_chat.py` | 
| **Thành viên 3** | *Chưa phân công* | `tcp_file.py` | 

## 📡 Định nghĩa Giao thức (Protocol Spec)

### 1. UDP Broadcast Presence

Mỗi peer rải gói tin broadcast mỗi **3 giây** tới port `9999`:

```
HELLO|<Name>|<TCP_Port>|<Status>
Ví dụ: HELLO|Quan|5000|online

```

### 2. TCP Message Framing

Mọi gói tin TCP gửi đi đều có header xác định kiểu và độ dài:

* **Tin nhắn chat:** `MSG|<length>|<utf-8 text>`

* **Truyền File:** `FILE|<filename>|<size_bytes>|<raw binary payload>`

* **Ngắt kết nối:** `BYE`

## 💻 Hướng dẫn Cài đặt & Chạy ứng dụng

### 1. Clone repository

```
git clone https://github.com/your-username/p2p-lan-chat.git
cd p2p-lan-chat

```

### 2. Chạy ứng dụng

Mở terminal và chạy lệnh:

```
python main.py --name "Tên_Của_Bạn" --port 5000

```

> **Lưu ý Test trên cùng 1 máy:** Mở nhiều cửa sổ Terminal và đổi hằng số `--port` khác nhau (Ví dụ: 5000, 5001, 5002).

## 🕹️ Danh sách lệnh trong Chat (`/help`)

| **Lệnh** | **Mô tả** | 
| `/list` | Hiển thị danh sách các máy đang online trong LAN | 
| \`/chat <name | num>\` | 
| `<nội dung>` | Gửi tin nhắn trực tiếp cho đối phương | 
| `/send <path_file>` | Gửi tệp tin cho người đang chat cùng | 
| `/leave` | Rời khỏi cuộc trò chuyện hiện tại | 
| \`/status <online | away>\` | 
| `/quit` | Thoát ứng dụng | 
