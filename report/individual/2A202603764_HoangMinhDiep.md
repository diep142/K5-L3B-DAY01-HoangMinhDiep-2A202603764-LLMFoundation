# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:** 2A202603764
- **Họ và tên:** Hoàng Minh Diệp
- **SEED** (in ở ô đầu notebook):3392

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | 	Notebook có output ở các cell đã thực thi. |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | 	Cell báo cáo cuối in thông báo đã lưu file này.|
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | Đã có ảnh bài tính tay |

## 2. Kết quả chính

| Mô hình | F1 test | F1 SUB 40 | Train (s) | p95 (ms/tin) | USD/1k tin |
|---|---:|---:|---:|---:|---:|
| BoW + LogReg (tự code) | 0.912 | 0.974 | 4.2 | 0.10 | 0.000 |
| BoW + MLP | 0.930 | 0.947 | 3.3 | 0.19 | 0.000 |
| Encoder (MiniLM) + kNN | 0.913 | 0.974 | 56.3 | 42.96 | 0.000 |
| Encoder (MiniLM) + LogReg | 0.011 | 0.000 | 56.5 | 37.74 | 0.000 |
| Encoder (MiniLM) + MLP head | 0.959 | 1.000 | 57.2 | 36.33 | 0.000 |
| LLM | — | — | — | — | — |

## 3. Dự đoán và thực tế

ml_f1 - Dự đoán: 0.85. Thực tế: F1 test của BoW + LogReg là 0.912.
dl_curve - Dự đoán: validation loss có thể tăng khi train loss giảm do overfitting; MLP có F1 cao hơn Logistic Regression. Thực tế: epoch có validation loss thấp nhất là 5; MLP F1 test 0.930, cao hơn LogReg 0.912.
dl_relu - Dự đoán: bỏ ReLU làm hai lớp Linear tương đương một phép biến đổi tuyến tính. Thực tế được notebook ghi: TEST F1 khi không có ReLU là 0.925.
emb_search - Dự đoán truy vấn sẽ gần spam. Thực tế: cả 6 hàng xóm gần nhất của truy vấn đều mang nhãn spam; cosine similarity từ 0.513 đến 0.563.
emb_tricky - Dự đoán: Encoder + LogReg đúng ít hơn BoW. Thực tế: BoW đúng 3/6; Encoder + LogReg đúng 2/6.
## 4. Phân tích 50 case khó

| Nhóm | Ham | Spam |
|---|---:|---:|
| ham_looks_spam | 10 | 0 |
| manipulation | 0 | 8 |
| natural_spam | 0 | 10 |
| obfuscation | 0 | 6 |
| prompt_injection | 1 | 5 |
| talks_about_spam | 4 | 0 |
| transactional_ham | 6 | 0 |

## 5. Thử thách cá nhân

*Đề theo SEED của bạn, cách làm, kết quả, giải thích.*

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*

## 7. AI log và điều đã học

| Phần | Bạn hỏi AI gì (prompt) | AI trả lời/viết gì | Bạn đã sửa / kiểm chứng gì |
|---|---|---|---|
| TODO 4.1 | Yêu cầu AI hoàn thiện hàm gọi OpenAI Chat Completions API, sử dụng OpenAI(), provider_kwargs(max_tokens), time.perf_counter() và trả về nội dung phản hồi cùng thời gian thực thi. | AI viết hàm gọi API, truyền model, messages, temperature, top_p và lấy nội dung phản hồi. | Bổ sung import time và from openai import OpenAI. Khi chạy thử, phát hiện lỗi NameError: OpenAI is not defined và khắc phục bằng cách import thư viện. |
| TODO 4.2 | Yêu cầu AI hoàn thiện hàm build_classification_prompt() để tạo prompt phân loại SMS thành spam/ham theo zero-shot và few-shot, đồng thời xử lý ví dụ mẫu và định dạng đầu ra. | AI viết hàm tạo prompt, bổ sung hướng dẫn phân loại, chèn examples nếu có và bỏ qua nhãn không hợp lệ. | Kiểm tra với cả hai trường hợp zero-shot và few-shot, xác nhận prompt kết thúc đúng bằng Message: """<text>""" và Label:. |
| TODO 4.3 | Yêu cầu AI hoàn thiện hàm parse_label() để chuyển phản hồi của LLM thành nhãn nhị phân: spam = 1, ham = 0. | AI viết hàm phân tích phản hồi, chuẩn hóa chữ hoa/thường, khoảng trắng và nhận diện nhãn spam/ham. ||
| TODO 4.4 | Yêu cầu AI hoàn thiện hàm llm_classify_batch() để phân loại nhiều SMS bằng LLM, đo thời gian xử lý và tính chi phí API. | AI viết hàm gọi API lần lượt cho từng tin nhắn, xử lý lỗi bằng retry, thu thập latency, dự đoán nhãn và thống kê chi phí. |  |

