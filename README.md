# project-manager-journal

# Software Estimation Framework: Blind Discovery Phase

## 1. Tổng quan (Context)
Trong giai đoạn đầu của dự án (Discovery Phase), khi chỉ có tài liệu (Documents) mà chưa có mã nguồn (Source Code) hoặc hệ thống thực tế, việc đưa ra Milestone ngay lập tức là một rủi ro lớn. Tài liệu kỹ thuật (đặc biệt là tài liệu Nhật) thường có độ trễ về thông tin và cấu trúc phân tầng phức tạp.

Framework này giúp định lượng thời gian cần thiết để nghiên cứu tài liệu và đưa ra Milestone chính xác dựa trên các chỉ số vật lý của Document.

---

## 2. Công thức Ước lượng mù (Blind Estimation Formula)

Thời gian cần thiết để thẩm thấu tài liệu và đưa ra Milestone ($T_{milestone}$) được tính bằng tổng thời gian nghiên cứu sâu từng cụm logic cộng với thời gian thiết lập dữ liệu mô phỏng.

$$T_{milestone} = \sum (T_{deep\_dive} \times W_d) + T_{mapping}$$

### Trong đó:
* **$T_{deep\_dive}$**: Thời gian nghiên cứu gốc ($Số\ cấp\ lồng\ của\ Section \times 0.25h$).
* **$W_d$ (Discovery Weight)**: Hệ số trọng số dựa trên mức độ lạ lẫm và rủi ro của Section:
    * $W_d = 1.0$: Section đầu tiên của mỗi Chương (Context mới hoàn toàn).
    * $W_d = 0.4$: Các Section tiếp theo trong cùng một Chương (Khả năng tái sử dụng Context cao).
    * $W_d = 1.5$: Các Section có độ sâu cấu trúc > 5 cấp (Rủi ro logic lồng ghép, cần audit kỹ).
* **$T_{mapping}$**: Thời gian bắt buộc để Mapping yêu cầu khách hàng vào tài liệu và thiết lập Data Test mẫu (thường dao động từ $4h - 8h$ tùy quy mô).

---

## 3. Case Study: Ước lượng cho 21 Sections (11 Requirements)

Giả định thực tế cho một tập hồ sơ 8000 dòng với các Section rải rác:

### Phân rã kỹ thuật:

1.  **Cụm Chương 5 & 7 (Context độc lập):**
    * $2\ mục \times 2\ cấp \times 0.25h \times 1.0 = 1.0h$
2.  **Cụm Chương 8.1 & 8.3 (Logic rời rạc):**
    * $2\ mục \times 2\ cấp \times 0.25h \times 1.0 = 1.0h$
3.  **Cụm Chương 8.5 (Trọng tâm - 6 mục liên đới):**
    * Mục sâu nhất (4 cấp): $4 \times 0.25h \times 1.5\ (Hệ\ số\ sâu) = 1.5h$
    * 5 mục vệ tinh: $5 \times 2\ cấp \times 0.25h \times 0.4\ (Hệ\ số\ cùng\ chương) = 1.0h$
4.  **Cụm Chương 8.8 (Cấu trúc trung bình):**
    * $2\ mục \times 3\ cấp \times 0.25h \times 1.0 = 1.5h$
5.  **Mapping & Data Setup (Bắt buộc):**
    * Thiết lập môi trường và tạo record mẫu cho 11 Requirements: **4.0h**

### Kết quả cuối cùng:
**Tổng thời gian ($T_{milestone}$): ~10 Giờ làm việc (Tương đương 1.5 - 2 ngày công).**

---

## 4. Lập luận với Stakeholders (PM/Client)

1.  **Tính khoa học**: Con số 1.5 ngày không phải là ước lượng cảm tính mà dựa trên cấu trúc phân tầng ($Hierarchy\ Depth$) của tài liệu.
2.  **Quản trị rủi ro**: Việc dành 10 giờ nghiên cứu giúp loại bỏ rủi ro "báo láo" Milestone. Nếu ép thời gian nghiên cứu, sai số Milestone có thể lên đến 200%.
3.  **Tính sẵn sàng**: Sau 1.5 ngày, team đã có sẵn Data mẫu và hiểu rõ luồng đi của biến số qua các tầng If-Else, giúp giai đoạn Coding diễn ra trơn tru, hạn chế tối đa việc sửa lỗi (Bug fixing) về sau.
