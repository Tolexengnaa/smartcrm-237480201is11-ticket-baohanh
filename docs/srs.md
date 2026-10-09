# BẢN ĐẶC TẢ YÊU CẦU PHẦN MỀM (SRS) RÚT GỌN

**Học phần:** Chuyên đề tốt nghiệp 1 · **Bài tập 1** – Phân tích và Thiết kế
**Case study:** Smart CRM – Mekong Mobile
**Luồng nghiệp vụ:** L2 – Tiếp nhận và phân loại yêu cầu bảo hành · **Track:** SE
**Sinh viên:** `<MINGBOUPPHA XAYKHAM>` – `<237480201IS11>` – `<261_71ITGR40203_03>` · **Phiên bản:** 1.0 · **Ngày:** 09/10/2026

---

## 1. Giới thiệu và phạm vi

### 1.1. Bối cảnh

Mekong Mobile tiếp nhận yêu cầu bảo hành thiết bị (điện thoại, máy tính bảng) tại cửa hàng. Hiện nay việc ghi nhận còn thủ công nên khó biết phiếu nào đang chờ, phiếu nào trễ hạn và ai đang xử lý. Luồng **L2** số hoá đoạn từ lúc khách mang thiết bị đến cửa hàng cho tới khi **phiếu bảo hành** được phân loại, đặt **mức ưu tiên** và giao cho **kỹ thuật viên**.

### 1.2. Phạm vi



**Trong phạm vi (L2):** tạo phiếu bảo hành; phân loại theo danh mục lỗi và đặt mức ưu tiên; phân công kỹ thuật viên; tra cứu danh sách phiếu; cập nhật trạng thái và lưu lịch sử trạng thái.

**Chủ ý KHÔNG làm (mức WON'T của MoSCoW):**

- Gửi SMS / email thông báo cho khách hàng (FR6).
- Quá trình sửa chữa, quản lý linh kiện, trả máy cho khách (các bước sau trạng thái *Đang xử lý*).
- Tính phí sửa chữa cho thiết bị hết hạn bảo hành, thanh toán.
- Khách hàng tự tạo phiếu trực tuyến; báo cáo thống kê.

### 1.3. Bảng thuật ngữ

Toàn bộ tài liệu, sơ đồ, ERD và wireframe chỉ dùng **đúng một tên** cho mỗi khái niệm dưới đây.

| Thuật ngữ (dùng trong tài liệu) | Tên trong CSDL / mã nguồn | Định nghĩa |
|---|---|---|
| Phiếu bảo hành | `ticket` | Bản ghi một lần khách yêu cầu bảo hành cho một thiết bị. |
| Mã phiếu       | `ticket.ticket_code` | Mã duy nhất dạng `BH-YYYY-NNNNNN`, do hệ thống sinh. |
| Khách hàng     | `customer` | Người mang thiết bị đến bảo hành; định danh bằng số điện thoại. |
| Thiết bị       | `device` | Điện thoại/máy tính bảng của khách; định danh bằng IMEI. |
| Ngày hết hạn bảo hành | `device.warranty_end_date` | Ngày cuối cùng thiết bị còn được bảo hành. |
| Danh mục lỗi   | `issue_category` | Nhóm lỗi dùng để phân loại phiếu (Màn hình, Pin, Nguồn, Phần mềm…). |
| Mức ưu tiên    | `ticket.priority` | Một trong ba giá trị: **Cao**, **Trung bình**, **Thấp**. |
| Hạn xử lý      | `ticket.due_date` | Thời điểm phiếu phải được xử lý xong, tính theo mức ưu tiên (BR4). |
| Trạng thái phiếu | `ticket.status` | **Mới → Đã phân loại → Đã phân công → Đang xử lý**; hoặc **Từ chối**. |
| Lịch sử trạng thái | `ticket_status_log` | Bản ghi mỗi lần trạng thái phiếu thay đổi: ai đổi, đổi từ gì sang gì, lúc nào. |
| Nhân viên      | `staff` | Người dùng nội bộ của hệ thống; có đúng một vai trò. |
| Nhân viên tiếp nhận | `staff.role = RECEPTIONIST` | Nhân viên tại quầy, tạo phiếu bảo hành. |
| Điều phối viên | `staff.role = COORDINATOR` | Nhân viên phân loại, đặt mức ưu tiên và phân công phiếu. |
| Kỹ thuật viên  | `staff.role = TECHNICIAN` | Nhân viên nhận phiếu được phân công để xử lý. |

---

## 2. Các bên liên quan và vai trò

| Vai trò | Được làm | Không được làm |
|---|---|---|
| **Nhân viên tiếp nhận** | Tra cứu khách hàng theo số điện thoại; tạo khách hàng mới; tạo phiếu bảo hành; xem danh sách và chi tiết phiếu. | Phân loại, đặt mức ưu tiên, phân công; chuyển trạng thái phiếu. |
| **Điều phối viên** | Mọi quyền xem; phân loại và đặt mức ưu tiên; phân công kỹ thuật viên; chuyển phiếu sang *Từ chối*. | Xoá phiếu; sửa lịch sử trạng thái. |
| **Kỹ thuật viên** | Xem phiếu được phân công cho mình; chuyển phiếu *Đã phân công → Đang xử lý*. | Xem phiếu của kỹ thuật viên khác; phân loại; phân công; từ chối phiếu. |

---

## 3. Yêu cầu chức năng

### 3.1. Danh sách yêu cầu chức năng

| Mã | Yêu cầu (phát biểu kiểm chứng được) | MoSCoW |
|---|---|---|
| **FR1** | Hệ thống cho phép Nhân viên tiếp nhận tạo phiếu bảo hành từ số điện thoại khách hàng, IMEI thiết bị và mô tả lỗi. Khi lưu, hệ thống sinh mã phiếu duy nhất, đặt trạng thái **Mới** và đánh dấu thiết bị còn/hết hạn bảo hành dựa trên ngày hết hạn bảo hành. | MUST |
| **FR2** | Hệ thống cho phép Điều phối viên chọn danh mục lỗi và mức ưu tiên cho phiếu ở trạng thái **Mới**; khi lưu, trạng thái thành **Đã phân loại** và hạn xử lý được tính theo BR4. | MUST |
| **FR3** | Hệ thống cho phép Điều phối viên phân công phiếu **Đã phân loại** cho đúng một kỹ thuật viên đang hoạt động; khi lưu, trạng thái thành **Đã phân công**. | MUST |
| **FR4** | Hệ thống hiển thị danh sách phiếu bảo hành, lọc được theo trạng thái, mức ưu tiên, khoảng hạn xử lý; tìm theo mã phiếu hoặc số điện thoại; phân trang 20 phiếu/trang; phiếu quá hạn xử lý được đánh dấu. | SHOULD |
| **FR5** | Mỗi lần trạng thái phiếu thay đổi, hệ thống ghi một bản ghi lịch sử trạng thái (trạng thái cũ, trạng thái mới, nhân viên thực hiện, thời điểm) và hiển thị trên màn hình chi tiết phiếu. | SHOULD |
| **FR6** | Hệ thống gửi SMS cho khách hàng khi trạng thái phiếu thay đổi. | WON'T |

### 3.2. User Story

| Mã | User Story | MoSCoW | Tiêu chí chấp nhận (cho story MUST) |
|---|---|---|---|
| **US1** | Là **Nhân viên tiếp nhận**, tôi muốn tạo phiếu bảo hành bằng số điện thoại khách hàng và IMEI, để yêu cầu của khách được ghi nhận với một mã phiếu duy nhất. | MUST | (1) Nhập SĐT đã có → tự điền tên khách hàng. (2) SĐT chưa có → tạo khách hàng mới ngay trên màn hình. (3) Lưu thành công → hiện mã phiếu `BH-YYYY-NNNNNN`, trạng thái *Mới*. (4) IMEI đang có phiếu chưa đóng → không cho lưu, báo lỗi (BR2). |
| **US2** | Là **Điều phối viên**, tôi muốn chọn danh mục lỗi và mức ưu tiên cho phiếu mới, để phiếu gấp được xử lý trước. | MUST | (1) Chỉ phiếu *Mới* mới phân loại được. (2) Bắt buộc chọn cả danh mục lỗi và mức ưu tiên. (3) Sau khi lưu, hạn xử lý hiển thị đúng BR4. |
| **US3** | Là **Điều phối viên**, tôi muốn giao phiếu đã phân loại cho một kỹ thuật viên, để mỗi phiếu có người chịu trách nhiệm. | MUST | (1) Danh sách chọn chỉ gồm kỹ thuật viên đang hoạt động. (2) Phiếu chưa phân loại → nút phân công bị khoá. (3) Người không phải Điều phối viên gọi chức năng → bị từ chối ở máy chủ (NFR4). |
| **US4** | Là **Điều phối viên**, tôi muốn xem danh sách phiếu lọc theo trạng thái và hạn xử lý, để biết phiếu nào sắp trễ. | SHOULD | — |
| **US5** | Là **Kỹ thuật viên**, tôi muốn xác nhận đã nhận phiếu được giao, để Điều phối viên biết phiếu đang được xử lý. | SHOULD | — |
| **US6** | Là **Điều phối viên**, tôi muốn xem lịch sử trạng thái của một phiếu, để biết ai đã thay đổi gì và vào lúc nào. | SHOULD | — |
| **US7** | Là **khách hàng**, tôi muốn nhận SMS khi phiếu bảo hành đổi trạng thái, để không phải gọi điện hỏi. | WON'T | — |

---

## 4. Yêu cầu phi chức năng

| Mã | Loại | Yêu cầu và ngưỡng đo được | Cách đo |
|---|---|---|---|
| **NFR1** | Hiệu năng | Màn hình danh sách phiếu (FR4) trả về trang đầu trong **≤ 2 giây** (phân vị 95) với **10.000** phiếu bảo hành trong CSDL. | Sinh 10.000 bản ghi mẫu, đo 50 lần gọi API danh sách. |
| **NFR2** | Khả dụng | Nhân viên tiếp nhận mới tạo xong một phiếu bảo hành trong **≤ 3 phút** và **≤ 3 màn hình**, sau tối đa **15 phút** hướng dẫn. | Thử với 5 người, bấm giờ. |
| **NFR3** | Bảo trì | Thay đổi một quy tắc nghiệp vụ (BR1–BR6) chỉ phải sửa **đúng 1 module** của lớp nghiệp vụ; **0** thay đổi ở lớp giao diện. | Đếm số file bị sửa trong commit thay đổi quy tắc. |
| **NFR4** | Bảo mật | **100%** API thay đổi phiếu kiểm tra vai trò ở máy chủ; **0** lần phân loại/phân công thành công khi người gọi không phải Điều phối viên. | Bộ ≥ 10 ca kiểm thử phân quyền, tỉ lệ đạt 100%. |
| **NFR5** | Toàn vẹn dữ liệu | **100%** thay đổi trạng thái phiếu có đúng 1 bản ghi lịch sử trạng thái tương ứng; không có trường hợp phiếu đổi trạng thái mà thiếu lịch sử. | Truy vấn đối chiếu số lần đổi trạng thái với số bản ghi `ticket_status_log`. |

---

## 5. Ràng buộc và quy tắc nghiệp vụ

| Mã | Quy tắc | Nguồn |
|---|---|---|
| **BR1** | Mỗi phiếu bảo hành gắn với đúng **một** khách hàng và **một** thiết bị. | `<Bảng 9.1 – đối chiếu>` |
| **BR2** | Một IMEI chỉ có **tối đa một** phiếu chưa đóng (*Mới, Đã phân loại, Đã phân công, Đang xử lý*) tại một thời điểm. | Phân tích của em |
| **BR3** | Thiết bị có ngày tiếp nhận sau ngày hết hạn bảo hành vẫn được tạo phiếu nhưng phiếu bị đánh dấu **Hết hạn bảo hành**; Điều phối viên quyết định tiếp nhận hay chuyển *Từ chối*. | `<Bảng 9.1 – đối chiếu>` |
| **BR4** | Hạn xử lý = thời điểm phân loại + **24 giờ** (Cao) / **72 giờ** (Trung bình) / **120 giờ** (Thấp). | `<Bảng 9.1 – đối chiếu>` |
| **BR5** | Chỉ Điều phối viên được phân loại, phân công và từ chối phiếu; chỉ phân công phiếu *Đã phân loại* cho kỹ thuật viên đang hoạt động. | Phân tích của em |
| **BR6** | Trạng thái chỉ đi theo thứ tự *Mới → Đã phân loại → Đã phân công → Đang xử lý*, không quay lui; *Từ chối* chỉ được chọn từ *Mới* hoặc *Đã phân loại*. Phiếu không bao giờ bị xoá. | Phân tích của em |

---

## 6. Bảng truy vết yêu cầu

| Yêu cầu chức năng | User Story | Use Case | MoSCoW | Bảng dữ liệu | Màn hình |
|---|---|---|---|---|---|
| FR1 · Tạo phiếu bảo hành | US1 | UC01 Tạo phiếu bảo hành (include UC07 Tra cứu khách hàng) | MUST | customer, device, ticket | M1 Tạo phiếu bảo hành |
| FR2 · Phân loại và đặt mức ưu tiên | US2 | UC02 Phân loại phiếu bảo hành | MUST | ticket, issue_category | M3 Chi tiết phiếu bảo hành |
| FR3 · Phân công kỹ thuật viên | US3 | UC03 Phân công kỹ thuật viên | MUST | ticket, staff | M3 Chi tiết phiếu bảo hành |
| FR4 · Tra cứu danh sách phiếu | US4 | UC04 Tra cứu danh sách phiếu | SHOULD | ticket (index `status`, `due_date`) | M2 Danh sách phiếu bảo hành |
| FR5 · Cập nhật trạng thái và lưu lịch sử | US5, US6 | UC05 Cập nhật trạng thái phiếu; UC06 Xem lịch sử trạng thái | SHOULD | ticket, ticket_status_log, staff | M3 Chi tiết phiếu bảo hành |
| FR6 · Gửi SMS cho khách hàng | US7 | Không có (ngoài phạm vi) | WON'T | Không có (ngoài phạm vi) | Không có (ngoài phạm vi) |

**Kiểm tra ngược:** mọi bảng của ERD (`customer`, `device`, `ticket`, `issue_category`, `staff`, `ticket_status_log`) đều xuất hiện ít nhất một lần ở cột *Bảng dữ liệu*; mọi FR mức MUST/SHOULD đều có User Story, Use Case, bảng và màn hình.
