# Dự Án Dự Đoán Rời Bỏ Khách Hàng Ngân Hàng

> Xây dựng và so sánh các mô hình học máy từ đầu nhằm dự đoán khả năng khách hàng rời bỏ dịch vụ ngân hàng (churn).

---

## Tổng Quan

Dự án tập trung vào bài toán phân loại nhị phân: dự đoán liệu một khách hàng có rời bỏ ngân hàng hay không, dựa trên các thông tin nhân khẩu học và hành vi tài chính. Điểm đặc biệt của dự án là toàn bộ các thuật toán đều được **cài đặt từ đầu** (không sử dụng thư viện mô hình có sẵn), giúp củng cố hiểu biết sâu về cơ chế hoạt động bên trong từng phương pháp.

Quy trình bao gồm các bước: khám phá và tiền xử lý dữ liệu (có kết hợp truy vấn cơ sở dữ liệu), huấn luyện ba mô hình khác nhau, và tổng hợp kết quả so sánh.

---

## Cấu Trúc Thư Mục

```
.
├── data/
│   └── Bank Customer Churn Prediction.csv   # Dữ liệu gốc
├── processed_data/                           # Dữ liệu đã tiền xử lý
│   ├── X_train, X_test                       # Tập huấn luyện và kiểm thử
│   ├── y_train, y_test                       # Nhãn tương ứng
│   ├── feature_columns                       # Danh sách đặc trưng
│   └── results/                             # Kết quả từng mô hình (định dạng JSON)
├── results/                                  # Lưu kết quả tổng hợp
├── report/                                   # Báo cáo (nếu có)
│
├── 1_2_EDA + preprocessing.ipynb            # Khám phá và tiền xử lý dữ liệu
├── 3_LogisticRegression_FromScratch.ipynb   # Hồi quy Logistic từ đầu
├── 4_KNN.ipynb                              # K Láng Giềng Gần Nhất từ đầu
├── 5_RandomForest.ipynb                     # Rừng Ngẫu Nhiên từ đầu
└── 6_Model_Comparison.ipynb                 # So sánh và tổng hợp kết quả
```

---

## Dữ Liệu

**Tệp gốc:** `data/Bank Customer Churn Prediction.csv`

| Tên cột | Mô tả |
|---|---|
| `customer_id` | Mã định danh khách hàng |
| `credit_score` | Điểm tín dụng |
| `country` | Quốc gia cư trú |
| `gender` | Giới tính |
| `age` | Tuổi |
| `tenure` | Số năm gắn bó với ngân hàng |
| `balance` | Số dư tài khoản |
| `products_number` | Số sản phẩm đang sử dụng |
| `credit_card` | Có sở hữu thẻ tín dụng không (0/1) |
| `active_member` | Là thành viên tích cực không (0/1) |
| `estimated_salary` | Thu nhập ước tính |
| `churn` | Nhãn mục tiêu — khách hàng rời bỏ (1) hay không (0) |

---

## Hướng Dẫn Chạy

### Bước 1 — Khám Phá và Tiền Xử Lý Dữ Liệu

Mở notebook `1_2_EDA + preprocessing.ipynb` và chạy toàn bộ các ô.

Notebook này thực hiện:
- Phân tích thống kê mô tả và trực quan hoá phân phối dữ liệu.
- Phát hiện và xử lý giá trị thiếu, ngoại lệ.
- Mã hoá biến phân loại, chuẩn hoá đặc trưng số.
- Chia tập dữ liệu thành tập huấn luyện và tập kiểm thử.
- Lưu toàn bộ dữ liệu đã xử lý vào thư mục `processed_data/`.

> **Lưu ý (tuỳ chọn):** Notebook có hướng dẫn tạo cơ sở dữ liệu và nhập dữ liệu vào SQL Server (qua SSMS), sau đó đọc lại bằng `pandas` để đối chiếu. Bước này không bắt buộc.

---

### Bước 2 — Huấn Luyện Mô Hình

Chạy lần lượt từng notebook mô hình theo thứ tự sau:

#### Hồi Quy Logistic (`3_LogisticRegression_FromScratch.ipynb`)
Cài đặt thuật toán hồi quy logistic từ đầu sử dụng gradient descent. Bao gồm các bước tính hàm sigmoid, hàm mất mát cross-entropy và cập nhật trọng số theo từng vòng lặp.

#### K Láng Giềng Gần Nhất (`4_KNN.ipynb`)
Cài đặt thuật toán KNN từ đầu với khoảng cách Euclidean. Notebook trình bày cách lựa chọn siêu tham số `k` thông qua kiểm định chéo.

#### Rừng Ngẫu Nhiên (`5_RandomForest.ipynb`)
Cài đặt từ đầu bao gồm: xây dựng cây quyết định (dựa trên tiêu chí Gini), lấy mẫu bootstrap, và tổng hợp kết quả từ nhiều cây qua biểu quyết đa số.

---

### Bước 3 — So Sánh Kết Quả

Mở notebook `6_Model_Comparison.ipynb`. Notebook sẽ tải kết quả của cả ba mô hình từ thư mục `processed_data/results/`, tổng hợp thành bảng so sánh và vẽ biểu đồ trực quan bao gồm đường cong ROC, ma trận nhầm lẫn và biểu đồ so sánh chỉ số.

---

## Kết Quả Tham Khảo

Bảng dưới đây tổng hợp hiệu năng của ba mô hình trên tập kiểm thử:

| Mô hình | Độ chính xác | Độ chuẩn xác | Độ phủ | Điểm F1 | Diện tích ROC |
|---|---|---|---|---|---|
| Hồi quy Logistic | 0.7741 | 0.4600 | 0.6641 | 0.5435 | 0.7864 |
| K Láng Giềng (k=5) | 0.7414 | 0.4148 | 0.6744 | 0.5137 | 0.8615 |
| Rừng Ngẫu Nhiên | **0.8370** | **0.5826** | 0.6872 | **0.6306** | 0.8497 |

**Nhận xét tổng quan:**
- **Rừng Ngẫu Nhiên** đạt hiệu năng tổng thể tốt nhất trên hầu hết các chỉ số, đặc biệt là độ chính xác và điểm F1.
- **K Láng Giềng** có diện tích dưới đường cong ROC cao nhất (0.8615), cho thấy khả năng phân biệt tốt giữa hai nhóm khi điều chỉnh ngưỡng quyết định.
- **Hồi quy Logistic** tuy đơn giản nhất nhưng vẫn cho kết quả cạnh tranh và dễ diễn giải nhất.

---

## Lưu Ý Kỹ Thuật

- Toàn bộ thuật toán được cài đặt **từ đầu bằng Python thuần**, không sử dụng các lớp mô hình từ thư viện `scikit-learn`. Mục tiêu là hiểu rõ cơ chế toán học bên trong.
- Nếu không muốn chạy lại bước tiền xử lý, có thể bỏ qua notebook `1_2_EDA + preprocessing.ipynb` và sử dụng trực tiếp các tệp có sẵn trong thư mục `processed_data/`.
- Kết quả của từng mô hình được lưu tự động dưới định dạng JSON vào `results/`, phục vụ cho bước so sánh tổng hợp ở bước 3.# BTL_KHDL
