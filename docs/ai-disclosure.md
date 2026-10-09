# PHỤ LỤC – BẢNG KHAI BÁO SỬ DỤNG CÔNG CỤ AI

**Cách em làm việc với AI.** Em cung cấp đề bài, slide buổi 6 và luồng đã duyệt (L2, track SE), yêu cầu Claude soạn bản nháp **từng phần một**, đọc lại từng phần trước khi làm phần tiếp theo, rồi tự hoàn thiện và kiểm chứng. Phần em tự làm ghi ở dòng cuối bảng.

| Công cụ | Phần áp dụng | Cách dùng (tóm tắt yêu cầu đã gửi) | Em đã chỉnh sửa / kiểm chứng gì |
|---|---|---|---|
| Claude (Anthropic) – trợ lý AI dạng chat | Mục 1 – SRS: bảng thuật ngữ, vai trò, FR1–FR6, US1–US7, NFR1–NFR5, BR1–BR6, bảng truy vết | Gửi đề bài và slide buổi 6, nêu luồng L2 – track SE; yêu cầu soạn bản nháp SRS đủ 6 mục theo mẫu của đề | Đối chiếu số lượng tối thiểu của đề (≥ 4 FR, 5–7 US, ≥ 3 NFR có ngưỡng số, ≥ 3 quy tắc); kiểm bảng truy vết không còn ô trống. Đối chiếu BR1, BR3, BR4 với Bảng 9.1 của case study: `<ghi kết quả và chỗ đã sửa>` |
| Claude (Anthropic) – trợ lý AI dạng chat | Mục 2 – Use Case Diagram (`usecase.drawio`), đặc tả UC01 | Yêu cầu vẽ sơ đồ use case dạng file draw.io gốc và đặc tả use case chính có luồng ngoại lệ | Đồng ý đề xuất đưa *Từ chối* vào UC02 và cập nhật SRS cho khớp. Mở file `.drawio`, kiểm actor ngoài ranh giới, ≥ 5 use case, có chú thích, tên khớp bảng thuật ngữ |
| Claude (Anthropic) – trợ lý AI dạng chat | Mục 3 – sơ đồ kiến trúc (`architecture.drawio`), 4 câu lập luận | Yêu cầu thiết kế kiến trúc phân lớp cho luồng L2 và viết câu lập luận theo khuôn "Vì NFR… yêu cầu…, tôi chọn…, đánh đổi là…" | Kiểm từng câu có đủ mã NFR, ngưỡng, quyết định và đánh đổi; kiểm tên bảng ở lớp lưu trữ khớp ERD. |
| Claude (Anthropic) – trợ lý AI dạng chat | Mục 4 – ERD (`erd.drawio`), `db/schema.sql` | Yêu cầu thiết kế ERD 4–6 bảng và SQL DDL skeleton cho PostgreSQL | Claude chạy thử `schema.sql` trên PostgreSQL 16, thử dữ liệu vi phạm BR2 và Từ chối thiếu lý do – đều bị chặn. Em kiểm ERD theo 5 lỗi ở slide buổi 6 |
| Claude (Anthropic) – trợ lý AI dạng chat | Mục 5 – Wireframe 3 màn hình (`wireframe.drawio`) | Yêu cầu vẽ M1, M2, M3 khớp thuật ngữ, khớp mô hình dữ liệu và có chỗ hiển thị cho mọi luồng ngoại lệ | Kiểm từng trường trên wireframe có trong ERD và mỗi luồng ngoại lệ (2a, 3a, 4a, 5a, 5b, 5c, 7a…) có chỗ hiển thị thông báo |
| Claude (Anthropic) – trợ lý AI dạng chat | Trình bày hồ sơ: file Word, `docs/srs.md`, `README.md`, `api-contract.md`, `.gitignore`, `.env.example` | Chuyển sang file Word, rút gọn theo mục 3.1 của đề, gộp vào `srs.md`, tạo các file còn thiếu của repo | Em tự chèn hình, điền thông tin cá nhân, xuất PDF và đặt tên file theo mẫu; mở lại PDF để kiểm tra trước khi nộp |
| — không dùng — | Chọn luồng nghiệp vụ L2 và track SE (giảng viên duyệt ở buổi 2); đối chiếu quy tắc với Bảng 9.1 của case study; điền thông tin cá nhân; chèn hình và xuất PDF; tạo repo GitHub và commit | Không áp dụng | Không áp dụng |

Tôi xác nhận đã đọc, hiểu và chịu trách nhiệm về toàn bộ nội dung nộp.

**Họ tên:** `<Họ tên>` · **MSSV:** `<MSSV>` · **Ngày:** `<dd/mm/2026>`
