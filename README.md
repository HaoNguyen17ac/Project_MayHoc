# Project_MayHoc: Dự đoán khách hàng đồng ý gửi tiết kiệm có kỳ hạn
Đồ án cá nhân môn Máy học sử dụng bộ dữ liệu Bank Marketing để dự đoán khách hàng có đồng ý gửi tiết kiệm có kỳ hạn (yes/`no`) hay không. Mục tiêu là hỗ trợ ưu tiên danh sách khách hàng trước khi gọi điện.

Các nội dung thực hành và kết quả được lưu trong notebook Google Colab; báo cáo tổng hợp được soạn trên Google Docs.
1. Danh sách tài liệu
Tài liệu	Nội dung	Liên kết
Tuần 0	Làm quen Google Colab, pandas, trực quan hóa và quy trình máy học cơ bản	Mở notebook Tuần 0
Tuần 1	Bài toán, dữ liệu, khám phá dữ liệu (EDA), rò rỉ dữ liệu và mô hình mốc E0	Mở notebook Tuần 1
Tuần 2	Tiền xử lý; Logistic Regression (E1), kNN (E2) và so sánh trên validation	Mở notebook Tuần 2
Tuần 3	MLP (E3), thử nghiệm số nơ-ron và seed, cải tiến nhóm tuổi (E4), chọn mô hình và ngưỡng	Mở notebook Tuần 3
Tuần 4	Đánh giá trên test, ma trận nhầm lẫn, phân tích lỗi và dự đoán khách hàng giả định	Mở notebook Tuần 4
Báo cáo đồ án	Tổng hợp phương pháp, kết quả, phân tích lỗi, kết luận và phụ lục	Mở báo cáo Google Docs


2. Dữ liệu và công cụ
- Dữ liệu: Bank Marketing - UCI Machine Learning Repository, gồm 45.211 bản ghi, 16 thuộc tính đầu vào ban đầu và nhãn đích y (yes/`no`).
- Chia dữ liệu: train 27.126, validation 9.042, test 9.043; chia phân tầng theo nhãn với random_state=42.
- Tiền xử lý: loại duration vì không biết thời lượng cuộc gọi trước khi gọi; dùng One-Hot Encoding cho thuộc tính phân loại và StandardScaler cho thuộc tính số. Mô hình cơ bản dùng 50 đặc trưng sau biến đổi.
- Công cụ: Python, Google Colab, NumPy, pandas, Matplotlib và scikit-learn.
- Seed cá nhân để lấy mẫu: 79.
3. Nội dung thực hiện theo tuần
Tuần 0 – Làm quen công cụ

Thực hành thao tác với notebook, đọc CSV, thống kê bằng pandas, lấy mẫu tái lập và vẽ biểu đồ. Notebook còn có ví dụ minh họa TF-IDF/Logistic Regression và phần Word2Vec–LSTM; đây là bài luyện công cụ, không phải mô hình dự đoán Bank Marketing của đồ án.

Tuần 1 – Khám phá dữ liệu

Kiểm tra phân bố nhãn (khoảng 11,7% khách đồng ý), thống kê theo tuổi, kênh liên lạc, tháng và kết quả chiến dịch trước. Giải thích vì sao phải loại duration để tránh rò rỉ thông tin. Mô hình mốc E0 luôn đoán no đạt Accuracy 0,883 nhưng F1-yes bằng 0.

Tuần 2 – Tiền xử lý và mô hình đầu tiên

Xây dựng pipeline cho E1 – Logistic Regression với trọng số lớp cân bằng và E2 – kNN (k = 15). Tại ngưỡng 0,5 trên validation, E1 đạt F1-yes 0,375, E2 đạt 0,267. Các phép thử cho thấy tác động của trọng số lớp, chuẩn hóa và rò rỉ thông tin do duration.

Tuần 3 – MLP và lựa chọn mô hình

Thử MLP với 4, 16, 64, 256 nơ-ron ẩn, mỗi cấu hình qua 5 seed; theo dõi dấu hiệu quá khớp. Thử thêm nhóm tuổi cho Logistic Regression (E4), rồi so sánh các ứng viên và ngưỡng dự đoán trên validation. Chọn E3 – MLP 256 nơ-ron, seed 42, ngưỡng 0,2 và khóa quyết định trước khi đánh giá test.

Tuần 4 – Đánh giá cuối và phân tích lỗi

Tái tạo đúng mô hình đã khóa; đánh giá trên test ở ngưỡng 0,5 và 0,2; lập ma trận nhầm lẫn, so sánh validation–test. Phân tích kết quả theo contact, poutcome, 3 trường hợp bỏ lỡ (FN), 3 trường hợp gọi nhầm (FP) và thử dự đoán 4 khách hàng giả định.
4. Kết quả chính
So sánh mô hình trên validation
Mô hình	Ngưỡng được chọn	F1-yes
E0 – Dự đoán lớp đa số (mốc, ngưỡng mặc định)	—	0,000
E1 – Logistic Regression	0,6	0,446
E2 – kNN	0,2	0,412
E3 – MLP (256 nơ-ron, seed 42)	0,2	0,4761
E4 – Logistic Regression + nhóm tuổi	0,7	0,458


Ngưỡng của E1–E4 được lựa chọn theo F1-yes trên validation; E0 chỉ là mốc so sánh.
Mô hình E3 trên test (ngưỡng đã khóa 0,2)
Chỉ số	Kết quả
Accuracy	0,8614
Precision-yes	0,4246
Recall-yes	0,5189
F1-yes	0,4670
Số khách được chọn gọi	1.293 / 9.043
Dự đoán đúng khách đồng ý (TP)	549
Bỏ lỡ khách đồng ý (FN)	509
Gọi nhầm khách từ chối (FP)	744
Dự đoán đúng khách từ chối (TN)	7.241


Trong nhóm 1.293 khách được mô hình chọn, 549 khách thực sự đồng ý, tương đương tỷ lệ 42,5%. F1-yes trên test thấp hơn validation khoảng 0,0091. Không thay đổi mô hình hoặc ngưỡng dựa trên kết quả test.
5. Cách xem và chạy
1. Mở notebook cần xem bằng liên kết Google Colab ở Mục 1.
2. Chọn Runtime → Run all để chạy lại các ô theo thứ tự khi cần. Mỗi notebook có phần thiết lập và các ô tự kiểm tra riêng.
3. Xem output ngay dưới các ô code và phần [BÁO CÁO], [CÂU HỎI SUY NGẪM] trong notebook.
4. Mở báo cáo Google Docs để xem phần tổng hợp theo 7 mục chính, bảng, hình, tài liệu tham khảo và phụ lục.
Lưu ý: Tuần 0 là phần luyện công cụ. Đồ án Bank Marketing chính được thực hiện ở Tuần 1–4. Kết quả trình bày trong README được lấy từ các output đã lưu trong notebook và báo cáo.
