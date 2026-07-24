# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 14h00–18h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.7, 1.2 và 1.8 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Hà Nội."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi? Ở mức nào phản hồi bắt đầu
kém mạch lạc?** (2–3 câu)
> Khi temperature tăng từ 0.0 lên 1.8, em nhận thấy phản hồi có xu hướng đa dạng và sáng tạo hơn nhưng cũng ít ổn định hơn. Ở temperature 0.0–0.7, câu trả lời khá trực tiếp và mạch lạc; từ khoảng 1.2 trở lên cách diễn đạt bắt đầu biến đổi nhiều hơn, còn ở 1.8 phản hồi có nguy cơ lan man hoặc kém nhất quán.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**
> Với trợ lý soạn thảo hợp đồng pháp lý, em chọn temperature khoảng 0.1–0.2 để phản hồi ổn định, chính xác và hạn chế sáng tạo không cần thiết. Với trợ lý viết slogan quảng cáo, em chọn khoảng 0.9–1.2 để tạo ra nhiều cách diễn đạt đa dạng và sáng tạo hơn.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**
> Tổng lượng output là 20 triệu token mỗi ngày. Với giá trong template, em tính GPT-4o tốn khoảng 200 USD/ngày, còn GPT-4o-mini khoảng 12 USD/ngày, tức model lớn đắt hơn khoảng 16,7 lần. Theo em, model lớn phù hợp với các tác vụ khó cần suy luận và chất lượng cao, ví dụ phân tích tài liệu phức tạp; model nhỏ phù hợp với các tác vụ số lượng lớn và đơn giản như FAQ hoặc phân loại yêu cầu người dùng.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích máy học (machine learning) là gì?"** nhưng hai system prompt
khác nhau:
- "Bạn là một nhà thơ, trả lời mọi thứ bằng hình ảnh ví von, tránh thuật ngữ."
- "Bạn là kỹ sư phần mềm senior, trả lời chính xác, có ví dụ code khi phù hợp."

**Hai phản hồi khác nhau như thế nào (giọng văn, độ dài, mức kỹ thuật)?
Từ đó rút ra system prompt điều khiển được những khía cạnh nào của phản hồi?**
(3–4 câu)
> Em nhận thấy persona nhà thơ tạo phản hồi giàu hình ảnh ví von, ít thuật ngữ và thiên về giải thích trực quan. Persona kỹ sư phần mềm sử dụng ngôn ngữ kỹ thuật hơn, cấu trúc rõ ràng và có thể đưa ra ví dụ cụ thể hoặc code. Từ đó, em thấy system prompt có thể điều khiển vai trò, giọng văn, mức độ kỹ thuật, cách trình bày và độ chi tiết của phản hồi.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**
> Với đoạn văn tiếng Việt gồm 160 từ, em đo được 184 token bằng tiktoken, trong khi cách ước lượng số từ/0.75 cho khoảng 213.33 token, chênh lệch khoảng 13.75%. Nếu dùng cách ước lượng thô này để dự toán ngân sách API cho tiếng Việt, em sẽ dự toán thừa vì số token thực tế nhỏ hơn số token được ước lượng theo số từ. Theo em, nguyên nhân là cách mã hóa token không tương ứng trực tiếp một-một với số từ, vì vậy dùng tokenizer thực tế sẽ cho kết quả chính xác hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)
> Theo em, chatbot văn bản và trợ lý giọng nói đều hưởng lợi từ streaming vì người dùng có thể nhận nội dung ngay khi model bắt đầu sinh phản hồi thay vì phải chờ toàn bộ câu trả lời hoàn tất. Trợ lý giọng nói hưởng lợi nhiều nhất vì có thể bắt đầu đọc phản hồi sớm, làm giảm cảm giác độ trễ. Ngược lại, pipeline dịch tài liệu chạy ngầm ban đêm hầu như không cần streaming vì người dùng chủ yếu quan tâm đến kết quả hoàn chỉnh sau khi xử lý xong.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**
> Theo em, exponential backoff giúp thời gian chờ tăng dần sau mỗi lần thất bại, từ đó giảm số request gửi tới API khi hệ thống đang quá tải và tạo thời gian để server phục hồi. Tuy nhiên, nếu tất cả client cùng dùng một lịch retry giống nhau, chúng vẫn có thể đồng loạt gửi lại request tại cùng thời điểm. Kỹ thuật jitter thêm một khoảng trễ ngẫu nhiên để phân tán thời điểm retry giữa nhiều client, từ đó giảm hiện tượng nhiều request cùng dồn vào server một lúc.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**
> System prompt em sử dụng là: "Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt." Nếu bỏ phần "trợ giảng của khóa AI", trợ lý sẽ mất định hướng chuyên môn và có thể trả lời như một chatbot chung. Nếu bỏ phần "trả lời ngắn gọn bằng tiếng Việt", phản hồi có thể dài hơn hoặc sử dụng ngôn ngữ khác thay vì luôn ưu tiên tiếng Việt.
### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**
> Ví dụ, ở lượt đầu em cho trợ lý biết rằng dự án đang sử dụng Gemini và cung cấp một số yêu cầu cấu hình, sau đó em tiếp tục hỏi nhiều câu khác. Đến lượt thứ năm hoặc thứ sáu, thông tin ban đầu có thể đã bị loại khỏi history nên trợ lý có thể quên rằng dự án đang dùng Gemini và đưa ra hướng dẫn dành cho OpenAI. Theo em, một cách khắc phục là tóm tắt các lượt hội thoại cũ thành một phần context ngắn rồi giữ bản tóm tắt đó cùng với 4 lượt gần nhất.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
