# TÀI LIỆU AGENT BLUEPRINT (GIWO) 
**Dự án:** VinFast Sales Advisory Agent
**Mã bài tập:** AI301_S02_BTVN_M1_BT1
**Người thiết kế:** [Tên của bạn]

---

## 1. GOAL (MỤC TIÊU) VÀ INPUT CONTRACT (DỮ LIỆU ĐẦU VÀO)

### 1.1. Mục tiêu (SMART Goal)
- **Cụ thể (Specific):** Tư vấn chính xác thông số kỹ thuật, chính sách giá và ưu đãi của dòng xe VinFast VF 8 cho khách hàng tại thị trường Bắc Mỹ (đặc biệt là California).
- **Đo lường được (Measurable):** Đạt tỷ lệ chuyển đổi (Conversion Rate) điều hướng khách hàng đăng ký lịch lái thử thành công ở mức 15% trên tổng số phiên tư vấn; thời gian phản hồi dưới 3 giây.
- **Khả thi (Achievable):** Sử dụng LLM tích hợp cơ sở dữ liệu (RAG) về sản phẩm của VinFast hiện có.
- **Thực tế (Realistic):** Phù hợp với định hướng mở rộng kinh doanh toàn cầu và xu hướng mua xe trực tuyến.
- **Thời hạn (Time-bound):** Hoàn thiện bản thử nghiệm Agent trong vòng 2 tuần.

### 1.2. Input Contract (Quy định cấu trúc dữ liệu đầu vào)
Hệ thống yêu cầu các trường dữ liệu JSON cơ bản từ phía người dùng/hệ thống CRM truyền vào trước khi bắt đầu phản hồi:
- `Customer_Location`: Nơi sinh sống của khách hàng (VD: "California, USA" - Bắt buộc để tính ưu đãi thuế).
- `Target_Car_Model`: Dòng xe khách hàng quan tâm (VD: "VF 8").
- `Customer_Query`: Câu hỏi thô bằng văn bản của khách (VD: "How much is the VF 8 and its range?").

---

## 2. WORKFLOW (CHUỖI QUY TRÌNH XỬ LÝ)

Agent được lập trình để đi qua chuỗi 4 bước logic theo dạng tuần tự và rẽ nhánh dựa trên Intent (ý định) của khách hàng:

**Bước 1: Tiếp nhận và phân tích ý định (Intent Classification)**
- Phân tích `Customer_Query` để phân loại thành các nhóm: Hỏi thông số (Specs), Hỏi giá (Pricing), hoặc Đặt lịch (Test Drive).

**Bước 2: Cung cấp Thông số kỹ thuật (Specs Advisory)**
- Truy xuất cơ sở dữ liệu: Nêu bật các ưu điểm của xe VF 8.
- *Nội dung chuẩn:* Quãng đường di chuyển 471 km/lần sạc đầy, động cơ điện mạnh mẽ với công suất 402 mã lực.

**Bước 3: Tư vấn Giá và Ưu đãi (Pricing & Incentives) - Cấu trúc rẽ nhánh**
- Nếu `Customer_Location` = "California": Báo giá niêm yết từ 47.000 USD. Thông báo ngay khoản ưu đãi thuế đặc biệt của bang là 2.000 USD.
- Nếu `Customer_Location` != "California": Chỉ báo giá niêm yết 47.000 USD và báo khách hàng chờ nhân viên kiểm tra chính sách thuế riêng của bang đó.

**Bước 4: Kêu gọi hành động (Call-to-Action / Test Drive)**
- Luôn kết thúc phiên tư vấn bằng một lời mời khéo léo để khách hàng trải nghiệm thực tế.
- Đề xuất gửi link đặt lịch lái thử trực tuyến (Booking Link) tại Showroom gần nhất.

---

## 3. OUTPUT SCHEMA VÀ GUARDRAILS (KẾT QUẢ VÀ RÀO CHẮN)

### 3.1. Output Schema (Định dạng đầu ra)
Kết quả trả về cho khách hàng trên giao diện chat phải được định dạng theo Markdown, đảm bảo dễ đọc và trực quan.
- Sử dụng **in đậm** cho các con số quan trọng (giá, công suất).
- Sử dụng *bullet points* (-) cho danh sách thông số.
- Kèm link hành động `[Đặt lịch lái thử ngay](link)`.

### 3.2. Guardrails (Hàng rào an toàn)
Để tránh rủi ro pháp lý và ảnh hưởng thương hiệu, Agent bị ràng buộc bởi 02 quy tắc nghiêm ngặt:
1. **Chống ảo giác giá (Price Hallucination Guardrail):** Tuyệt đối không được tự ý tính toán, tạo ra các mức giảm giá, hoặc hứa hẹn tặng quà ngoài khoản "ưu đãi thuế 2.000 USD tại California". Mọi con số về giá phải khớp 100% với tài liệu nguồn.
2. **Miễn trừ trách nhiệm (Disclaimer Requirement):** Trong mọi câu trả lời liên quan đến tài chính, Agent bắt buộc phải thêm dòng lưu ý: *"Lưu ý: Mức giá thực tế và ưu đãi có thể thay đổi tùy thuộc vào thời điểm giao dịch và các khoản phí tại địa phương của bạn."*