## 1. Cấu Trúc Dự Án (Chuẩn Clean Code & Module hóa)

```text
TKW/
│
├── html/                           # Thư mục giao diện HTML
│   ├── home.html                   # Trang chủ giới thiệu hệ thống, tính năng & CTA
│   ├── index.html                  # Trang Đăng nhập & Đăng ký (Chọn vai trò: Học sinh / Giáo viên)
│   ├── dashboard.html              # Bảng điều khiển Học sinh (Kho đề thi, Tìm kiếm, Lọc, Bookmark, Lịch sử)
│   ├── teacher-dashboard.html      # Bảng điều khiển Giáo viên (Quản lý đề, Tạo đề All-in-one, Báo cáo)
│   └── exam.html                   # Phòng thi trực tuyến, đếm ngược, chấm điểm & xem lại bài thi
│
├── css/                            # Thư mục CSS chuẩn phong cách DOLEARN
│   └── style.css                   # Định kiểu toàn diện (Màu sắc, Typography, Card, Modal, Toast, In A4)
│
├── js/                             # Thư mục xử lý logic JavaScript thuần (Vanilla JS)
│   ├── auth.js                     # Xác thực, Phân quyền RBAC, Route Guard, Quản lý phiên chống lỗi F5
│   ├── teacher.js                  # Nghiệp vụ Giáo viên: Tạo/sửa đề thi, soạn câu hỏi trắc nghiệm A-B-C-D
│   ├── dashboard.js                # Nghiệp vụ Học sinh: Bộ lọc môn học, tìm kiếm, lưu đề, xem trước
│   ├── exam.js                     # Phòng thi: Timer đếm ngược, lưu tạm trạng thái, nộp bài, xem lại đáp án
│   └── utils.js                    # Kho dữ liệu đề thi chuẩn, LocalStorage helpers, In đề A4 Quốc gia
│
├── index.html                      # File chuyển hướng root tự động vào html/home.html
└── README.md                       # Tài liệu thuyết minh dự án và hướng dẫn sử dụng
```

## 2. Các Phân Hệ Chức Năng Nổi Bật

### 2.1. Trang Chủ Giới Thiệu (`home.html`)
- Thanh điều hướng (Sticky Navbar) với nhận diện phiên: Tự động đổi nút **Bảng điều khiển** (Giáo viên / Học sinh) và nút **Đăng xuất**.
- Hero Banner, danh sách tính năng, kho đề thi nổi bật và luồng thi thử trực quan.

### 2.2. Đăng Ký & Đăng Nhập Phân Quyền (`auth.js` - `index.html`)
- **Phân vai trò khi đăng ký:** Thẻ `<select>` chọn rõ ràng giữa:
  - `Học sinh / Thí sinh` (role: `student`) -> Điều hướng sang `dashboard.html`.
  - `Giáo viên / Giảng viên` (role: `teacher`) -> Điều hướng sang `teacher-dashboard.html`.
- **Bảo vệ tuyến đường nghiêm ngặt (Route Guard):**
  - Học sinh cố tình vào trang Giáo viên sẽ bị chặn ngay tại `<head>` và tự động chuyển về `dashboard.html`.
  - Giáo viên truy cập nhầm vào trang làm bài thi của học sinh sẽ tự động được điều hướng về `teacher-dashboard.html`.
- **Chống lỗi F5:** Tuyệt đối không ghi đè tài khoản mẫu vào `localStorage`, giữ nguyên 100% phiên làm việc khi tải lại trang.
- **Tài khoản học sinh demo có sẵn:**
  - `hocsinh@utc.edu.vn` | Mật khẩu: `password123`
  - `sinhvien@utc.edu.vn` | Mật khẩu: `password123`

### 2.3. Bảng Quản Lý Giáo Viên (`teacher.js` - `teacher-dashboard.html`)
- **Tạo đề thi mới All-in-One:** Modal thông minh 2 phần gồm:
  - *Phần A (Thông tin chung):* Tên đề, Mã đề tự động gợi ý theo môn, Môn học, Thời gian (phút).
  - *Phần B (Thêm câu hỏi trắc nghiệm):* Nhập nội dung câu hỏi, 4 phương án A, B, C, D, chọn đáp án đúng qua radio button, danh sách câu hỏi nháp có thể xóa bớt trước khi lưu.
- **Đồng bộ thời gian thực:** Đề thi giáo viên vừa tạo hoặc chỉnh sửa sẽ xuất hiện ngay lập tức trong kho đề thi của học sinh.
- **In đề thi chuẩn A4 Quốc gia:** Xuất bản in đầy đủ chuẩn form Bộ Giáo dục & Đào tạo.

### 2.4. Bảng Điều Khiển Học Sinh (`dashboard.js` - `dashboard.html`)
- Kho đề thi trực quan dạng thẻ Card (Môn học, mã đề, thời lượng, số câu).
- Bộ lọc kết hợp: Ô tìm kiếm tức thì, lọc theo Môn học (Toán, Lý, Hóa, Sinh, Sử, Anh), Subject Pills 1-click.
- Lưu đề yêu thích (Bookmark) và Xem trước cấu trúc đề thi (Preview Modal).
- Tab **Kết quả & Lịch sử làm bài:** Lưu điểm số, thời gian và số câu đúng sau mỗi lần thi.

### 2.5. Phòng Thi Trực Tuyến & Chấm Điểm (`exam.js` - `exam.html`)
- **Đồng hồ đếm ngược:** Đếm lùi thời gian thực, tự động nhấp nháy đỏ khi còn dưới 60 giây và tự động thu bài khi hết giờ (`00:00`).
- **Lưu tạm trạng thái:** Tự động lưu tiến độ làm bài vào `localStorage` theo từng học sinh.
- **Chấm điểm tự động & Xếp loại:** Thang điểm 10 kèm nhận xét khách quan.
- **Chế độ xem lại bài làm (Review Mode):** Tô màu viền xanh lá `#10b981` cho câu trả lời đúng, tô viền đỏ `#ef4444` cho câu trả lời sai và chỉ ra đáp án đúng.

### 2.6. Module In Đề Thi Chuẩn A4 Bộ GD&ĐT (`utils.js`)
- Cấu trúc bản in chuẩn văn bản quy phạm:
  - Tiêu đề Quốc gia: **BỘ GIÁO DỤC VÀ ĐÀO TẠO** - ĐỀ THI CHÍNH THỨC.
  - Bảng thông tin thí sinh: Họ tên, Số báo danh, Phòng thi, Chữ ký.
  - **Phiếu trả lời trắc nghiệm tô tròn** Ⓐ Ⓑ Ⓒ Ⓓ.
  - Chữ ký xác nhận của 2 Cán bộ coi thi.

---

## 3. Hướng Dẫn Khởi Chạy

1. Mở thư mục dự án bằng VS Code hoặc bất kỳ trình soạn thảo mã nguồn nào.
2. Mở file `index.html` (ở thư mục gốc) hoặc `html/home.html` bằng trình duyệt web bất kỳ (Chrome, Edge, Firefox, Cốc Cốc).

