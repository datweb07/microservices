# Monolithic - Microservices

Giới thiệu về sự tiến hóa của các kiến trúc phần mềm, từ cách viết chương trình nguyên khối cơ bản nhất cho đến các hệ thống phân tán và vi dịch vụ hiện đại.

## 1. Kiến trúc Monolithic (Nguyên khối)
Đây là cách tiếp cận cơ bản và đơn giản nhất mà mọi lập trình viên đều học đầu tiên (ví dụ: chương trình Hello World). 
- **Đặc điểm:** Toàn bộ mã nguồn của chương trình được biên dịch cùng nhau thành một file thực thi duy nhất. Khi chạy, toàn bộ phần mềm được nạp vào bộ nhớ và chạy trong cùng một tiến trình duy nhất.
- **Tính chất nạp dữ liệu:** Không thể chia nhỏ để chạy trên các tiến trình khác nhau. Khi nạp vào bộ nhớ, hoặc là toàn bộ chương trình được nạp thành công, hoặc không có gì được nạp.
- **Sử dụng thư viện:** Kể cả khi phần mềm phình to và sử dụng các thư viện liên kết động (**DLL - Dynamic Link Library**) chỉ nạp vào bộ nhớ khi cần thiết để tiết kiệm tài nguyên, nó vẫn được coi là kiến trúc Monolithic. Lý do là các thư viện này vẫn được nạp vào chung một tiến trình với chương trình chính.
- **Hạn chế:** Khi cần xử lý công việc lớn, đòi hỏi năng lực tính toán cao, cách duy nhất để mở rộng là chạy trên một máy tính có cấu hình phần cứng mạnh hơn (scale-up). Tuy nhiên, phần cứng luôn có giới hạn và không thể nâng cấp mãi mãi, đồng thời cũng gây lãng phí tài nguyên khi ứng dụng không có nhiều người sử dụng.

## 2. Kiến trúc Client-Server (Khách - Chủ)
Để giải quyết bài toán giới hạn phần cứng của Monolithic, người ta tạo ra các hệ thống phân tán, chia ứng dụng chạy trên nhiều máy tính khác nhau.
- **Đặc điểm:** Chia phần mềm thành hai phần riêng biệt: Client (Giao diện, tương tác người dùng) và Server (Xử lý tính toán nặng, lưu trữ dữ liệu).
- **Cách hoạt động:** Khi Client cần thực hiện một tính năng, nó sẽ kết nối, gửi yêu cầu đến Server. Server xử lý và trả kết quả lại cho Client. Cả hai chạy trên các tiến trình riêng biệt và thường ở các máy tính khác nhau.
- **Phân loại:**
  - *Thick Client / Fat Server:* Đa số khối lượng công việc được dồn xử lý ở phía Server.
  - *Fat Client / Thin Server:* Client đảm nhận phần lớn việc xử lý, Server chỉ xử lý các tác vụ nhỏ.
- Mọi trang web, cơ sở dữ liệu hiện đại đều đang ứng dụng mô hình này. Tuy nhiên, nó vẫn chưa đủ tốt để xử lý những logic doanh nghiệp quá phức tạp.

## 3. Kiến trúc N-Tier (Đa tầng)
Khi Server phải chứa quá nhiều thành phần phức tạp, người ta tiếp tục chia nhỏ ứng dụng ra thành nhiều tầng (tier) để dễ quản lý và mở rộng.
- **Mô hình 3 tầng (3-Tier) phổ biến nhất:**
  - *Presentation Tier:* Chịu trách nhiệm về giao diện hiển thị.
  - *Business Logic Tier:* Xử lý các nghiệp vụ.
  - *Data Tier:* Chịu trách nhiệm lưu trữ.
- **Quy tắc luồng gọi:** Tầng trên chỉ được gọi xuống tầng ngay dưới nó (ví dụ: Presentation gọi Business, Business gọi Data). Ở mô hình N-Tier chuẩn, không có việc gọi nhảy cóc trực tiếp từ Presentation xuống Data.
- **Tier khác với Layer:** "Tier" (Tầng) ám chỉ việc phân tách về mặt vật lý (chạy trên các tiến trình/máy chủ khác nhau), trong khi "Layer" (Lớp) thường nói về sự phân tách logic bên trong mã nguồn. 
- **Công cụ hỗ trợ:** Để các tầng biết cách gọi hàm lẫn nhau từ xa, người ta cần đến các **Application Server** (ví dụ: nền tảng Java J2EE).
- **Hạn chế:** Hệ thống thường phụ thuộc vào một Registry trung tâm để các tầng tìm kiếm lẫn nhau (discovery). Nếu dịch vụ Registry này chết, toàn bộ hệ thống có thể sập theo. Việc triển khai ứng dụng N-Tier trên máy chủ vật lý trước đây cũng rất tốn kém và cồng kềnh.

## 4. Kiến trúc Microservices (Vi dịch vụ)
Kiến trúc này chia nhỏ ứng dụng thành các dịch vụ độc lập và tương tác với nhau. Dù khái niệm này đã có từ lâu, nhưng nó mới bùng nổ gần đây nhờ sự phát triển của công nghệ Ảo hóa (Virtualization), Cloud và Container (ví dụ: Docker), giúp việc triển khai và mở rộng quy mô các dịch vụ trở nên cực kỳ dễ dàng.

Để một hệ thống được gọi là Microservices, nó phải tuân thủ nghiêm ngặt các bộ quy tắc:
1. **Chia theo tính năng nghiệp vụ (Business Feature):** Ứng dụng phải được chia thành các dịch vụ độc lập dựa trên nghiệp vụ cụ thể (VD: Dịch vụ Giỏ hàng, Dịch vụ Thanh toán, Dịch vụ Quản lý kho). Tuyệt đối không chia theo thành phần vật lý kiểu N-Tier (chỉ có dịch vụ chuyên lưu trữ hoặc chuyên giao diện).
2. **Quy mô đủ nhỏ:** Các service phải đủ nhỏ để một nhóm (team) có thể tự xây dựng, vận hành và quản trị hoàn toàn.
3. **Quản lý dữ liệu độc lập:** Mỗi Microservice phải sở hữu và tự quản lý một cơ sở dữ liệu riêng của nó. Nếu nhiều service vẫn gọi chung vào một database trung tâm thì đó không phải là Microservices.

*Lưu ý:* Việc phá vỡ các quy tắc đồng nghĩa với việc đánh mất lợi ích của Microservices. Bản thân mỗi một service nhỏ trong Microservices khi hoạt động có thể xem như một ứng dụng Monolithic, và khi các service gọi cho nhau, đó chính là mô hình Client-Server. Để xây dựng được các kiến trúc phức tạp như Microservices hay N-Tier tốt, thì điều tiên quyết là đội ngũ phải có khả năng xây dựng tốt một kiến trúc Monolithic trước tiên.
