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
>*Khi temperature tăng từ 0.0 lên 1.5, phản hồi có xu hướng đa dạng và sáng tạo hơn về cách diễn đạt, trong khi temperature thấp cho câu trả lời ổn định và ít biến động hơn. Ở temperature 0.0, model thường trả lời khá trực tiếp và nhất quán; còn ở 1.0–1.5, nội dung hoặc cách kể có thể phong phú và khó đoán hơn. Tuy nhiên, temperature cao không đảm bảo câu trả lời chính xác hơn mà chủ yếu làm tăng mức độ ngẫu nhiên khi sinh nội dung.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi sẽ đặt temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức thấp giúp câu trả lời ổn định, nhất quán và ít “sáng tạo quá mức”, từ đó giảm nguy cơ model đưa ra thông tin sai hoặc trả lời lệch khỏi chính sách hỗ trợ.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
>*Với 10.000 người dùng, mỗi người gọi 3 lần/ngày và mỗi lần khoảng 350 token đầu ra, tổng là khoảng 10,5 triệu token output/ngày. Theo bảng giá của bài lab, GPT-4o tốn khoảng 105 USD/ngày, còn GPT-4o-mini khoảng 6,3 USD/ngày, tức GPT-4o đắt hơn khoảng 16,7 lần nếu chỉ xét token đầu ra. Tôi sẽ dùng GPT-4o cho các tác vụ khó cần suy luận và độ chính xác cao, còn GPT-4o-mini phù hợp với chatbot FAQ hoặc các câu hỏi đơn giản, số lượng lớn để tiết kiệm chi phí.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Với system prompt “giáo viên tiểu học”, câu trả lời thường ngắn hơn, dùng từ đơn giản và có ví dụ gần gũi để trẻ 8 tuổi dễ hiểu, chẳng hạn ví blockchain như một cuốn sổ ghi chép chung. Với system prompt “chuyên gia tài chính”, phản hồi có xu hướng dài và chuyên sâu hơn, sử dụng các thuật ngữ như sổ cái phân tán, cơ chế đồng thuận, tính bất biến và phi tập trung. System prompt vì vậy ảnh hưởng rõ đến cách model lựa chọn từ vựng, mức độ chi tiết và kiểu ví dụ phù hợp với đối tượng người đọc. Nội dung cốt lõi vẫn cùng chủ đề blockchain nhưng cách trình bày thay đổi theo vai trò được yêu cầu.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**


---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming quan trọng nhất khi phản hồi của model dài hoặc mất vài giây để tạo xong, vì người dùng có thể thấy nội dung xuất hiện dần ngay lập tức thay vì phải chờ toàn bộ câu trả lời hoàn tất. Điều này giúp trải nghiệm chatbot tự nhiên hơn và giảm cảm giác chờ đợi, dù tổng thời gian model xử lý không thực sự giảm. Non-streaming phù hợp hơn khi phản hồi ngắn, khi ứng dụng cần nhận toàn bộ kết quả rồi mới xử lý tiếp, hoặc khi dữ liệu đầu ra phải được kiểm tra đầy đủ trước khi hiển thị cho người dùng.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff giúp giảm tải cho server bằng cách tăng dần thời gian chờ sau mỗi lần retry, ví dụ 0,1s → 0,2s → 0,4s, nên hệ thống có thêm thời gian phục hồi khi đang quá tải. Nếu hàng nghìn client đều retry với một delay cố định giống nhau, chúng có thể cùng gửi lại request tại cùng một thời điểm, tạo ra một đợt tải lớn mới và khiến server tiếp tục quá tải. Vì vậy exponential backoff giúp phân tán các lần retry theo thời gian và làm hệ thống ổn định hơn.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *Tôi chọn persona là một trợ giảng AI thân thiện dành cho người mới bắt đầu. System prompt của tôi là: “Bạn là trợ giảng AI, hãy trả lời bằng tiếng Việt, giải thích ngắn gọn, dễ hiểu và đưa ví dụ đơn giản khi cần.” Tôi yêu cầu “trả lời bằng tiếng Việt” để phù hợp với người dùng mục tiêu, còn “ngắn gọn, dễ hiểu” giúp câu trả lời không quá dài và hạn chế dùng thuật ngữ phức tạp khi người dùng chưa có nhiều kiến thức nền.*

### Câu 4.2 — Hạn chế & cải thiện

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
