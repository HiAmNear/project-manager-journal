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

# Phase 2: Environment Readiness & Data Prototyping Framework

## 1. Mục tiêu (Objective)
Sau khi hoàn thành Phase 1 (Discovery & Milestone), Phase 2 tập trung vào việc hiện thực hóa các kịch bản kiểm thử (Test Scenarios). Mục tiêu là đảm bảo mọi "điểm chạm" logic trong tài liệu đều có dữ liệu đối soát thực tế trước khi bắt đầu giai đoạn Coding.

---

## 2. Công thức Tính thời gian Tái hiện (Deep-Dive Setup Formula)

Thời gian tái hiện ($T_{reproduce}$) được tính dựa trên độ sâu của cấu trúc nhánh dữ liệu và các ràng buộc hệ thống (Constraints).

$$T_{reproduce} = \sum (N_{root} \times T_{base}) + \sum (N_{deep} \times T_{base} \times D)$$

### Trong đó:
* **$N_{root}$**: Các Section có cấu trúc nông (Cấp 1-3). Logic đơn giản, dễ tạo Data.
* **$N_{deep}$**: Các Section có cấu trúc sâu (Cấp > 4). Logic lồng ghép, nhiều điều kiện ràng buộc.
* **$T_{base}$**: Thời gian cơ bản để thiết lập 1 bộ Data mẫu (Khuyên dùng: $1.0h$).
* **$D$ (Depth Factor)**: Hệ số nhân độ sâu (Gánh nặng của việc truy vết điều kiện If-Else):
    * Cấp 4: $D = 1.25$
    * Cấp 5: $D = 1.5$
    * Cấp 6: $D = 1.65$
    * Cấp 7: $D = 1.75$

---

## 3. Case Study: Ước lượng thực tế cho 21 Sections

Dựa trên danh sách 21 điểm ảnh hưởng (Impact Points) phân bổ từ Chương 5 đến Chương 8.8:

### Phân rã định lượng:

| Phân nhóm Section | Số lượng | Công thức ($T_{base} = 1.0h$) | Kết quả |
| :--- | :---: | :--- | :--- |
| **Nhánh nông (Cấp 2-3)** | 8 | $8 \times 1.0h$ | 8.0h |
| **Nhánh trung bình (Cấp 4)** | 1 | $1 \times 1.0h \times 1.25$ | 1.25h |
| **Nhánh sâu (Cấp 5)** | 1 | $1 \times 1.0h \times 1.5$ | 1.5h |
| **Nhánh cực sâu (Cấp 7)** | 1 | $1 \times 1.0h \times 1.75$ | 1.75h |

**=> Tổng cộng ($T_{reproduce}$): ~12.5 Giờ làm việc.**

---

## 4. Ý nghĩa chiến lược (Strategic Value)

1. **Khắc phục sự mơ hồ**: Việc dành ~12.5h để setup data giúp làm sáng tỏ các điểm "mù" trong tài liệu mà việc đọc thuần túy không thể phát hiện.
2. **Loại bỏ "Bottleneck"**: Khi Junior bắt đầu code, họ đã có sẵn môi trường và data chuẩn. Điều này triệt tiêu thời gian chờ đợi (Idle time) và giảm thiểu rủi ro lỗi Logic cha (Parent Logic errors).
3. **Chỉ số Seniority**: Khả năng định lượng thời gian setup dựa trên cấu trúc lồng ($Depth$) chứng minh năng lực quản trị rủi ro và am hiểu hệ thống sâu sắc của người lập kế hoạch.

---
*Documented by Nhat Ma*
