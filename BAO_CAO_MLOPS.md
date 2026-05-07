# BÁO CÁO KẾT QUẢ DỰ ÁN MLOPS: TỰ ĐỘNG HÓA QUY TRÌNH CI/CD CHO AI

**Người thực hiện:** Hà Hữu An 
**Dự án:** Dự báo chất lượng rượu (Wine Quality Prediction)  
**Công nghệ sử dụng:** Python, Scikit-learn, MLflow, DVC, GitHub Actions, FastAPI.

---

## 1. Mục tiêu dự án
Xây dựng một hệ thống MLOps hoàn chỉnh có khả năng:
*   Theo dõi và quản lý các thí nghiệm huấn luyện mô hình.
*   Tự động hóa quy trình kiểm thử và huấn luyện lại (CI/CD) mỗi khi có thay đổi về mã nguồn.
*   Tự động cập nhật mô hình khi có dữ liệu mới (Continuous Training).
*   Phục vụ dự báo qua API (Model Serving).

---

## 2. Giai đoạn 1: Quản lý thí nghiệm với MLflow
Trong giai đoạn này, tôi đã thực hiện huấn luyện mô hình RandomForest cục bộ và sử dụng MLflow để ghi lại các chỉ số.
*   **Tham số tối ưu:** `n_estimators: 500`, `max_depth: None`.
*   **Kết quả tốt nhất (Local):** Accuracy đạt **0.6780**.
*   **Nhận xét:** MLflow giúp việc so sánh các bộ siêu tham số trở nên trực quan và dễ dàng lựa chọn mô hình tốt nhất.

![Giao diện MLflow](Screenshot%202026-05-07%20231711.png)
---

## 3. Giai đoạn 2: Xây dựng Pipeline CI/CD tự động
Tôi đã thiết lập GitHub Actions để tự động hóa quy trình. Mỗi khi code được đẩy lên GitHub, hệ thống sẽ tự động thực hiện 4 bước (Jobs):
1.  **Unit Test:** Đảm bảo code không có lỗi logic.
2.  **Train:** Huấn luyện lại mô hình trên môi trường Cloud của GitHub.
3.  **Eval:** Kiểm tra chất lượng mô hình qua Accuracy Gate (Ngưỡng 0.65).
4.  **Smoke Test:** Chạy thử server FastAPI để đảm bảo API sẵn sàng phục vụ.

*Kết quả: Toàn bộ Pipeline đã vượt qua tất cả các bài kiểm tra (Green status).*

![GitHub Actions Pipeline](Screenshot%202026-05-07%20231923.png)

---

## 4. Giai đoạn 3: Huấn luyện liên tục khi có dữ liệu mới
Ở giai đoạn này, tôi mô phỏng việc bổ sung thêm **2998 mẫu dữ liệu mới** (Phase 2). Ngay khi file dữ liệu được cập nhật và push lên GitHub, Pipeline đã tự động kích hoạt lượt huấn luyện mới.

**So sánh kết quả trước và sau khi thêm dữ liệu:**

| Chỉ số | Phase 1 (2998 mẫu) | Phase 2 (5996 mẫu) |
|---|---|---|
| **Accuracy** | 0.6760 | 0.7460 |
| **F1-score** | 0.6748 | 0.7451 |

*   **Nhận xét:** Việc tự động hóa giúp mô hình luôn được cập nhật với dữ liệu mới nhất mà không cần sự can thiệp thủ công của con người. Hệ thống đảm bảo tính ổn định cao nhờ các bước kiểm định tự động.

![Kết quả sau khi thêm dữ liệu](Screenshot%202026-05-07%20232049.png)

---

## 5. Kết luận
Dự án đã chứng minh khả năng triển khai MLOps trong thực tế. Việc kết hợp GitHub Actions và các công cụ quản lý mô hình không chỉ giúp giảm thiểu sai sót do con người mà còn rút ngắn thời gian từ lúc có dữ liệu mới đến lúc triển khai mô hình thành công.
