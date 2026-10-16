# BÁO CÁO KIỂM THỬ HIỆU NĂNG BẰNG APACHE JMETER

## GIỚI THIỆU CHUNG VỀ APACHE JMETER

### 1. Apache JMeter là gì?
**Apache JMeter** là một phần mềm mã nguồn mở dựa trên nền tảng Java, được thiết kế để kiểm thử hiệu năng (Performance Testing), kiểm thử khả năng chịu tải (Load Testing) và kiểm thử áp lực (Stress Testing) các ứng dụng web cũng như nhiều dịch vụ protocol khác nhau. 

JMeter cho phép giả lập lượng lớn truy cập đồng thời từ nhiều người dùng ảo (Virtual Users/Threads) gửi đến máy chủ để đo đạc và đánh giá mức độ phản hồi của hệ thống.
<img width="1917" height="987" alt="image" src="https://github.com/user-attachments/assets/8cea132e-fcee-47e3-81b1-4e86dad2fb7c" />


### 2. Các thành phần cơ bản trong JMeter
| Thành phần | Vai trò |
| :--- | :--- |
| **Test Plan** | Kịch bản kiểm thử tổng thể chứa toàn bộ cấu hình. |
| **Thread Group** | Nhóm người dùng ảo (quy định số lượng user, thời gian ramp-up, số vòng lặp). |
| **Sampler** | Giao thức/Request gửi đi (HTTP Request, API, JDBC, FTP,...). |
| **Assertion** | Điều kiện kiểm tra kết quả trả về (Response Code 200/201, nội dung chứa chuỗi,...). |
| **Listener** | Thu thập, phân tích và hiển thị kết quả (View Results Tree, Aggregate Report,...). |
<img width="981" height="621" alt="image" src="https://github.com/user-attachments/assets/a25824b3-82bd-42c4-896d-c2ecf43a1140" />

<img width="652" height="511" alt="image" src="https://github.com/user-attachments/assets/d000a1a8-2cc2-41a5-a112-ed4ef29ddcf1" />

<img width="717" height="577" alt="image" src="https://github.com/user-attachments/assets/dc8b23bf-ac77-4bdb-8682-a81d462b3651" />

### 3. Ý nghĩa ba tham số quan trọng của Thread Group
- **Number of Threads (users):** Số lượng người dùng ảo truy cập đồng thời vào hệ thống.
- **Ramp-Up Period (seconds):** Khoảng thời gian (tính bằng giây) để JMeter kích hoạt toàn bộ số người dùng ảo.
- **Loop Count:** Số lần mỗi người dùng ảo thực hiện lại kịch bản kiểm thử.
<img width="512" height="142" alt="image" src="https://github.com/user-attachments/assets/963a18b7-29a2-41e4-b2b4-94585b31b799" />

### 4. Mục tiêu bài kiểm thử
- Thực hành xây dựng kịch bản kiểm thử hiệu năng cho **Trang web** (Cổng thông tin Sinh viên PSC Phenikaa) và **Dịch vụ API** (JSONPlaceholder).
- Phân tích các chỉ số hiệu năng cốt lõi: **Thời gian phản hồi (Response Time)**, **Tỷ lệ lỗi (Error Rate)** và **Thông lượng (Throughput)**.
- Đưa ra đánh giá về mức độ ổn định và khả năng chịu tải của hệ thống dưới kịch bản kiểm thử.

---

## 1. KIỂM TRA HIỆU NĂNG TRANG WEB (WEB TESTING)

### 1.1. Mục tiêu
- Sử dụng JMeter để mô phỏng 100 người dùng đồng thời truy cập vào Cổng thông tin Sinh viên Phenikaa: `https://psc.phenikaa-uni.edu.vn/vi`
- Phân tích tốc độ phản hồi và độ ổn định của hệ thống web dưới tải giả lập.

### 1.2. Kịch bản kiểm tra
- **Thread Group (Cấu hình người dùng):**
  - Số lượng người dùng (Number of Threads): `100`
  - Thời gian khởi tạo tải (Ramp-up Period): `10` giây
  - Số lần lặp (Loop Count): `1`
- **HTTP Request:**
  - Giao thức (Protocol): `https`
  - Tên máy chủ (Server Name): `psc.phenikaa-uni.edu.vn`
  - Phương thức (Method): `GET`
  - Đường dẫn (Path): `/vi`
  - Mã hóa (Content Encoding): `UTF-8`
- **Response Assertion:**
  - Field to Test: `Response Code`
  - Pattern Matching Rules: `Equals`
  - Patterns to Test: `200`
- **Listeners:**
  - View Results Tree
  - Aggregate Report

*Hình ảnh minh họa cấu hình kịch bản Web Testing:*
<img width="1475" height="625" alt="image" src="https://github.com/user-attachments/assets/61a100a0-064e-4b6e-aa9a-08d4d6fb2801" />

<img width="1475" height="531" alt="image" src="https://github.com/user-attachments/assets/6a08c333-b01d-4b10-8c79-119c045c752c" />

<img width="1917" height="983" alt="image" src="https://github.com/user-attachments/assets/5eaf2942-3970-43d0-adaf-78af4ae273d4" />


### 1.3. Kết quả kiểm tra
*Kết quả từ Listener Aggregate Report và View Results Tree:*
<img width="1473" height="110" alt="Aggregate Report Web" src="https://github.com/user-attachments/assets/9bbd9a22-7e83-4420-9aa1-74b47b5a3e01" />

<img width="1862" height="260" alt="View Results Tree Web" src="https://github.com/user-attachments/assets/cd081cf6-e6f2-4695-9a67-f56ebe46d7fe" />

### 1.4. Phân tích kết quả
Dựa trên bảng thống kê từ **Aggregate Report**:
- **Tổng số request đã gửi:** 100
- **Số lượng yêu cầu thành công:** 66/100 (66.0%)
- **Tỷ lệ lỗi (Error %):** 34.00% (Máy chủ bị quá tải nhẹ với 34 request trả về lỗi 500 Internal Server Error)
- **Thời gian phản hồi trung bình (Average):** 7156.79 ms (~7.16 giây)
- **Thời gian phản hồi trung vị (Median):** 8299.50 ms (~8.3 giây)
- **Thời gian phản hồi 90% (90% Line):** 10049.40 ms (~10.05 giây)
- **Thông lượng (Throughput):** 5.01 req/sec

### 1.5. Kết luận
Trang web Cổng thông tin Sinh viên PSC Phenikaa gặp hiện tượng quá tải nhẹ khi phải xử lý đồng thời 100 người dùng trong 10 giây. Tỷ lệ lỗi ở mức **34.00%** (do máy chủ phản hồi lỗi 500 Internal Server Error) và thời gian phản hồi trung bình kéo dài đến **7156.79 ms (~7.16 giây)**.

**Đánh giá:** Hệ thống web hiện tại chưa thực sự tối ưu để đáp ứng tốt lượng truy cập lớn đồng thời trong khoảng thời gian ngắn (như thời điểm sinh viên đăng ký học phần). Cần đề xuất tối ưu hóa bộ nhớ đệm (caching) hoặc nâng cấp hạ tầng server để đảm bảo tính ổn định hơn.

---

## 2. KIỂM TRA HIỆU NĂNG API (API TESTING)

### 2.1. Mục tiêu
- Giả lập tải gửi đến dịch vụ API công khai JSONPlaceholder: `https://jsonplaceholder.typicode.com/users`
- Kiểm tra tốc độ truy xuất dữ liệu JSON và tính tin cậy của dịch vụ API dưới áp lực truy cập đồng thời.

### 2.2. Kịch bản kiểm tra
- **Thread Group (Cấu hình người dùng):**
  - Số lượng người dùng (Number of Threads): `100`
  - Thời gian khởi tạo tải (Ramp-up Period): `10` giây
  - Số lần lặp (Loop Count): `1`
- **HTTP Request:**
  - Giao thức (Protocol): `https`
  - Tên máy chủ (Server Name): `jsonplaceholder.typicode.com`
  - Phương thức (Method): `GET`
  - Đường dẫn (Path): `/users`
- **Response Assertion:**
  - Field to Test: `Response Code`
  - Pattern Matching Rules: `Equals`
  - Patterns to Test: `200`
- **Listeners:**
  - View Results Tree
  - Aggregate Report

*Hình ảnh minh họa cấu hình kịch bản API Testing:*
<img width="1477" height="512" alt="image" src="https://github.com/user-attachments/assets/a6904121-f3b2-4644-be62-fb50eeb5486d" />

<img width="1917" height="822" alt="image" src="https://github.com/user-attachments/assets/fbec7009-60ee-4fa6-b622-93c91afba7a0" />


### 2.3. Kết quả kiểm tra
*Kết quả từ Listener Aggregate Report và View Results Tree:*
<img width="1467" height="77" alt="Aggregate Report API" src="https://github.com/user-attachments/assets/6bbe33e0-7abd-473d-96af-f3e4ad26a325" />

<img width="1867" height="262" alt="View Results Tree API" src="https://github.com/user-attachments/assets/c3c0d2f3-c3cf-4201-b184-590e5a9e5505" />

### 2.4. Phân tích kết quả
Dựa trên bảng thống kê từ **Aggregate Report**:
- **Tổng số request API:** 100
- **Số lượng yêu cầu thành công:** 100/100 (100.0%)
- **Tỷ lệ lỗi (Error %):** 0.00%
- **Thời gian phản hồi trung bình (Average):** 184.96 ms
- **Thời gian phản hồi trung vị (Median):** 184.00 ms
- **Thời gian phản hồi 90% (90% Line):** 195.10 ms
- **Thông lượng (Throughput):** 5.15 req/sec

### 2.5. Kết luận
Dịch vụ API `JSONPlaceholder` hoạt động cực kỳ ấn tượng và ổn định dưới tải kịch bản mô phỏng 100 người dùng đồng thời. 

**Đánh giá:** 
- Tỷ lệ lỗi đạt **0.00%** (100/100 request trả về mã trạng thái `200 OK`).
- Thời gian phản hồi trung bình chỉ **184.96 ms** (dưới 0.2 giây) và độ trễ ở mốc 90% người dùng chỉ **195.10 ms**, chứng tỏ khả năng xử lý nhanh chóng và đáp ứng hoàn hảo yêu cầu tích hợp dữ liệu thời gian thực.

---

## 3. CẤU TRÚC REPOSITORY
```text
.
├── README.md                 # Báo cáo chi tiết kiểm thử hiệu năng
├── lab8_test.jmx             # File kịch bản kiểm thử JMeter
├── lab8_test.csv             # File dữ liệu kết quả thô xuất từ JMeter
└── images/                   # Thư mục chứa các ảnh chụp kịch bản và kết quả
