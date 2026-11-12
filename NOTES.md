# Ghi chú phân tích · Day 04

Họ và tên: Triệu Tuấn Minh
MSSV:

## 1. Case được chọn

- ID case: Chưa chọn được; cả 22 case đều chưa được chấm.
- Câu trả lời của agent: Không có; model call thất bại trước khi agent tạo trace.
- Điểm ở lần chạy 1: `null` (0/22 case được chấm), không phải 0/100.

## 2. Bằng chứng trong trace

Trace và usage đều rỗng. Kết quả E1 trong `eval_before.json` báo HTTP 404;
kết quả E1 trong `eval_after.json` báo HTTP 402. Cả hai báo cáo có 22/22
case `unscored`. Chưa có bằng chứng về hành vi của agent vì model chưa trả lời.

```text
eval_before.json: status=error, error="RuntimeError: Provider trả HTTP 404..."
eval_after.json:  status=error, error="RuntimeError: Provider trả HTTP 402..."
summary mỗi lần: scored_cases=0, unscored_cases=22, score_0_100=null
```

## 3. Thay đổi đã thực hiện

- Sửa gì, ở đâu: Chưa sửa `agent.py`; cần khắc phục cấu hình/provider và chạy eval thành công trước.
- Vì sao chọn thay đổi này: Lỗi xảy ra khi gọi model, trước khi agent tạo trace. HTTP 404 cần kiểm tra model ID/provider; HTTP 402 cần kiểm tra billing hoặc quota. Chưa đủ căn cứ để quy lỗi cho agent.

## 4. So sánh hai lần chạy

| Chỉ số | `eval_before.json` | `eval_after.json` |
|---|---|---|
| Điểm tổng | `null` — 0/22 case được chấm | `null` — 0/22 case được chấm |
| Điểm case đã chọn | Chưa có | Chưa có |
| Số model calls thành công | 0 | 0 |
| Token, chi phí nếu có | Không có dữ liệu | Không có dữ liệu |

- Lỗi đã giải quyết: Chưa xác nhận; cả hai lần đều chưa chạy eval thành công.
- Lỗi còn lại hoặc mới xuất hiện: HTTP 404 ở lần trước và HTTP 402 ở lần sau. Cấu hình hai lần khác nhau nên chưa thể so sánh trước–sau.

## 5. Giới hạn

Chưa thể kết luận về chất lượng agent hoặc judge vì không có case nào được chấm. Sau khi kiểm tra model ID, provider, key và billing/quota, chạy lại hai lần với cùng cấu hình (agent model, judge model, max steps và dataset). Chỉ một lần chạy cho mỗi phiên bản cũng không chứng minh độ ổn định; điểm judge là ước lượng và snapshot có thể không phản ánh trạng thái live.
