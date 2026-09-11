# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Temperature thấp cho phản hồi ổn định hơn, temperature cao tạo nhiều cách trả lời khác nhau và ngẫu nhiên hơn

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Khoảng 0.2–0.4, vì chatbot CSKH cần ổn định, chính xác và ít trả lời ngẫu nhiên, không đặt là 0 vì chatbot CSKH vẫn có thể cần linh hoạt để diễn đạt tự nhiên

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Ước tinh GPT-4o đắt hơn mini khoảng 10-30 lần. GPT-4o đắt hơn đáng kể nên phù hợp với tác vụ phức tạp cần chất lượng cao, còn GPT-4o-mini phù hợp với tác vụ đơn giản, số lượng lớn như FAQ

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Prompt giáo viên tạo câu trả lời ngắn, từ ngữ đơn giản và ví dụ dễ hiểu. Prompt chuyên gia tạo câu trả lời dài hơn, nhiều từ kỹ thuật hơn. System prompt định hướng vai trò, phong cách và cách trả lời của model. Khi thay đổi system prompt từ giáo viên sang chuyên gia, model sẽ thay đổi độ sâu, thuật ngữ và cách giải thích cho phù hợp. System prompt có mức ưu tiên cao hơn user prompt nên ảnh hưởng trực tiếp đến cách model xử lý yêu cầu

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Hai con số có thể chênh lệch khá nhiều. Tokenizer chia text thành token chứ không đơn giản theo từ. Tiếng Việt thường có cách tokenization kém hiệu quả hơn tiếng Anh do có dấu thanh bị tách thành nhiều token

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming phù hợp với câu trả lời dài vì người dùng thấy kết quả ngay, cho cảm giác model hoạt động nhanh hơn. Non-streaming phù hợp khi cần toàn bộ response trước khi xử lý tiếp, ví dụ API hoặc pipeline tự động

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff tăng dần thời gian chờ, giúp server có thời gian hồi phục. Nếu hàng nghìn client retry cùng lúc với delay cố định, chúng có thể tạo thêm một đợt quá tải mới, có thể giải quyết bằng cách thêm random jitter

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngtếôn ngữ...):**
> Persona: Trợ lý kỹ thuật AI/software, trả lời ngắn gọn và thực tế. System prompt yêu cầu “ngắn gọn” để tránh lan man và “thực tế” để ưu tiên giải pháp có thể triển khai thực tế

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ giữ 3 lượt nên dễ mất context cũ. Có thể cải thiện bằng cách lưu summary của các cuộc hội thoại cũ và retrieve lại khi cần

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
