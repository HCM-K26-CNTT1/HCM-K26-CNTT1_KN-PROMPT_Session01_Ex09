# BÀI 9: KỸ THUẬT RAG GROUNDING DẬP TẮT ẢO GIÁC HỢP ĐỒNG THU MUA

## PHẦN 1. PHÂN TÍCH NGUYÊN NHÂN ẢO GIÁC

### 1. Nguyên nhân AI trả lời sai

Nhân viên Thu mua hỏi ChatGPT nhưng không cung cấp hợp đồng MartX.

AI dựa trên kiến thức đã học từ dữ liệu Pre-training và cơ chế dự đoán Token tiếp theo để tạo câu trả lời có vẻ hợp lý.

Do không có tài liệu nguồn để đối chiếu, AI đã tự suy đoán mức phạt 20% và đình chỉ nhập hàng 30 ngày. Đây là hiện tượng **Hallucination (ảo giác AI)**.

### 2. Phân biệt hai cách hỏi

| Hỏi LLM thông thường | RAG Grounding |
|---|---|
| Không cung cấp tài liệu gốc | Cung cấp hợp đồng làm tài liệu nguồn |
| AI dựa vào kiến thức đã học | AI tra cứu thông tin trong tài liệu được cung cấp |
| Có nguy cơ tự suy đoán | Giảm nguy cơ ảo giác |
| Khó kiểm chứng căn cứ | Có thể yêu cầu trích dẫn điều khoản |

## PHẦN 2. XÂY DỰNG RAG PROMPT HOÀN CHỈNH

### 1. Văn bản hợp đồng chuẩn

"Phạt 5% giá trị lô bị dập nát và yêu cầu đổi hàng trong 24h."

### 2. Câu hỏi nghiệp vụ

Theo hợp đồng MartX, nếu nhà cung cấp giao rau bị dập nát trên 10% thì mức phạt là bao nhiêu? Nhà cung cấp phải thực hiện trách nhiệm gì và trong thời hạn bao lâu?

### 3. Negative Constraint (Khóa an toàn)

- Chỉ sử dụng thông tin có trong văn bản hợp đồng được cung cấp.
- Không sử dụng kiến thức bên ngoài để bổ sung điều khoản.
- Không tự suy đoán mức phạt, thời hạn hoặc hình thức xử lý.
- Nếu thông tin không có trong văn bản, tuyệt đối trả lời: "Không có dữ liệu trong tài liệu".
- Nêu rõ căn cứ từ văn bản khi đưa ra câu trả lời.

### 4. Prompt RAG hoàn chỉnh

```
Bạn là trợ lý AI hỗ trợ nghiệp vụ thu mua của MartX.

**TÀI LIỆU NGUỒN:**

"Phạt 5% giá trị lô bị dập nát và yêu cầu đổi hàng trong 24h."

**CÂU HỎI:**

Theo hợp đồng MartX, nếu nhà cung cấp giao rau bị dập nát trên 10% thì mức phạt là bao nhiêu? Nhà cung cấp phải thực hiện trách nhiệm gì và trong thời hạn bao lâu?

**NEGATIVE CONSTRAINT:**

1. Chỉ sử dụng thông tin trong tài liệu nguồn.
2. Không tự suy đoán hoặc bổ sung điều khoản không có trong hợp đồng.
3. Nếu thông tin không có trong văn bản, tuyệt đối trả lời: "Không có dữ liệu trong tài liệu".
4. Trích dẫn nội dung hợp đồng làm căn cứ cho câu trả lời.

**ĐỊNH DẠNG TRẢ LỜI:**

- Mức phạt: 
- Trách nhiệm nhà cung cấp:
- Thời hạn:
- Căn cứ hợp đồng:
```

### 5. Kết quả mong đợi

- Mức phạt: 5% giá trị lô bị dập nát.
- Trách nhiệm nhà cung cấp: Đổi hàng.
- Thời hạn: Trong 24 giờ.
- Căn cứ hợp đồng: "Phạt 5% giá trị lô bị dập nát và yêu cầu đổi hàng trong 24h."
