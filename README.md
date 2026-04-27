# Dự án: Phân tích phân khúc khách hàng dựa trên mô hình RFM kết hợp dữ liệu hành vi số - So sánh K-Means và Hierarchical Clustering ( A+ )

## 📖 Tổng quan
Dự án này thực hiện phân khúc khách hàng trong thương mại điện tử bằng cách mở rộng mô hình **RFM (Recency - Frequency - Monetary)** truyền thống kết hợp với dữ liệu hành vi trực tuyến (thời gian truy cập, số lượt nhấp chuột). 

Hai thuật toán phân cụm không giám sát – **K-Means** và **Hierarchical Clustering** – được áp dụng song song để so sánh hiệu quả phân nhóm khách hàng.

### Mục tiêu chính:
- Xác định các phân khúc khách hàng có ý nghĩa (trung thành, tiềm năng, mới, rời bỏ...).
- Đánh giá và so sánh chất lượng phân cụm giữa K-Means và Hierarchical dựa trên chỉ số Davies–Bouldin (DBI), tính ổn định, khả năng mở rộng và diễn giải.
- Cung cấp cơ sở để doanh nghiệp xây dựng chiến lược marketing cá nhân hóa.

---

## 📊 Dữ liệu
- **Nguồn:** Kaggle – E‑commerce Purchase Dataset
- **Kích thước:** 24.999 giao dịch, 7 đặc trưng
- **Các biến chính:**
  - `date`, `customer_id`, `product_category`, `payment_method`
  - `value` (giá trị giao dịch – USD)
  - `time_on_site` (phút ở lại website)
  - `clicks` (số lần nhấp chuột)
- **Chất lượng dữ liệu:** Không có giá trị khuyết, không trùng lặp. Tỷ lệ khách hàng tương tác cao (>100 phút và >20 clicks) chiếm 27,4%.

Sau khi tiền xử lý, bộ dữ liệu RFM mở rộng gồm 5 biến: `Recency`, `Frequency`, `Monetary`, `time_on_site`, `clicks`.

---

## ⚙️ Phương pháp

### 1. Tiền xử lý
- Loại bỏ ngoại lệ (phương pháp IQR) – đặc biệt trên biến Monetary.
- Chuẩn hóa Z‑score (`StandardScaler`) để các biến có cùng trọng số.
- Tính toán chỉ số RFM cho từng khách hàng.

### 2. Phân cụm
- **Xác định số cụm tối ưu:** Kết hợp phương pháp Elbow, Silhouette (cho K‑Means) và Davies–Bouldin Index (cho cả hai).
- **K‑Means:** Số cụm $k = 4$, khoảng cách Euclidean.
- **Hierarchical Clustering:** Phương pháp liên kết Ward, khoảng cách Euclidean, trực quan hóa bằng dendrogram.

### 3. Đánh giá
- **Chỉ số chính:** Davies–Bouldin Index (DBI) – càng thấp càng tốt.
- **So sánh bổ sung:** Thời gian xử lý, khả năng mở rộng, trực quan kết quả.

---

## 🏆 Kết quả chính

### Các phân khúc khách hàng (k = 4)

| Cụm | Nhãn | Đặc điểm nổi bật | Chiến lược đề xuất |
|:---:|---|---|---|
| **0** | Khách hàng giá trị trung bình | R ~6.0, F=1, M=378 | Tái kích hoạt, ưu đãi quay lại |
| **1** | Khách hàng giá trị thấp / không hoạt động | R ~7.0, F=1, M=59 | Chiến dịch win‑back, nếu không hiệu quả thì ngừng đầu tư |
| **2** | Khách hàng cốt lõi, giá trị cao | R ~3.0, F=2, M=442 | Chương trình VIP, ưu đãi cá nhân hóa |
| **3** | Khách hàng tiềm năng | R ~2.0, F=1, M=164 | Nuôi dưỡng, remarketing, tăng tần suất mua |

> **Ghi chú:** Trong Hierarchical Clustering, các cụm tương tự cũng được phát hiện nhưng có sự khác biệt nhỏ về đặc trưng (ví dụ: nhóm "Loyal Customers" có M trung bình thấp hơn).

### So sánh hai thuật toán

| Tiêu chí | K‑Means | Hierarchical Clustering |
|---|---|---|
| **DBI tại k=4** | **0,80** (thấp hơn – tốt hơn) | 0,85 |
| **Thời gian xử lý** | Nhanh, phù hợp với ~25.000 mẫu | Chậm hơn, tốn bộ nhớ |
| **Khả năng mở rộng** | Tốt, dễ triển khai trong thực tế | Hạn chế với dữ liệu lớn |
| **Trực quan** | Không thể hiện quan hệ phân cấp | Dendrogram cho thấy cấu trúc tầng bậc |
| **Ứng dụng** | Ra quyết định nhanh, phân khúc marketing | Khám phá dữ liệu, nghiên cứu chuyên sâu |

**Kết luận:** K‑Means cho chất lượng phân cụm tốt hơn một chút (DBI thấp hơn) và vượt trội về hiệu năng, phù hợp cho các hệ thống CRM quy mô lớn. Hierarchical vẫn hữu ích trong giai đoạn thăm dò để hiểu cấu trúc tự nhiên của khách hàng.

---

## 💻 Yêu cầu hệ thống
- **Python:** 3.8+
- **Các thư viện chính:**
  - `pandas`, `numpy`
  - `scikit-learn` (KMeans, AgglomerativeClustering, StandardScaler, metrics)
  - `matplotlib`, `seaborn`
  - `xgboost` (cho bước chọn đặc trưng)

---

## 🚀 Hướng dẫn sử dụng

```bash
# 1. Clone repository
git clone [https://github.com/your-repo/customer-segmentation-rfm-behavior.git](https://github.com/your-repo/customer-segmentation-rfm-behavior.git)
cd customer-segmentation-rfm-behavior

# 2. Cài đặt các thư viện phụ thuộc
pip install -r requirements.txt

# 3. Chạy pipeline chính (EDA + tiền xử lý + phân cụm)
python main.py
