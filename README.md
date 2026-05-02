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

---

# Saga Pattern: Giải Pháp Xử Lý Giao Dịch Phân Tán Trong Microservices

### 1. Tổng quan về Saga Pattern
**Saga Pattern** là một mẫu thiết kế (design pattern) quan trọng trong kiến trúc microservices. Nó được sử dụng để quản lý các giao dịch phân tán (distributed transactions) khi mỗi microservice sở hữu một cơ sở dữ liệu (DB) riêng biệt.

Vì các giao dịch (transaction) truyền thống thường chỉ hoạt động trong phạm vi một hệ quản trị cơ sở dữ liệu duy nhất, Saga pattern giúp đồng bộ hóa các thao tác cập nhật dữ liệu trên nhiều service mà không cần dựa vào các giao dịch phân tán phức tạp (như 2PC - Two-Phase Commit).

### 2. Khái niệm cốt lõi
- **Microservice và DB riêng biệt:** Mỗi service quản lý dữ liệu riêng (SQL hoặc NoSQL như MongoDB, Redis). Các hệ thống NoSQL thường không hỗ trợ giao dịch đa tài liệu (multi-document transactions) hoặc giao dịch phân tán.
- **Thách thức về tính toàn vẹn:** Trong một hệ thống du lịch (Đặt xe -> Đặt phòng -> Đặt vé -> Thanh toán), nếu một bước thất bại, hệ thống phải có cơ chế tự động hoàn tác (**rollback**) các bước trước đó để tránh sai lệch dữ liệu.
- **Local Transactions:** Saga chia một giao dịch lớn thành một chuỗi các giao dịch cục bộ (local transactions). Mỗi service thực hiện phần việc của mình và phát ra sự kiện (event) để kích hoạt bước tiếp theo.

### 3. Hai mô hình triển khai Saga Pattern

| Loại Saga | Đặc điểm chính | Ưu điểm / Nhược điểm |
| :--- | :--- | :--- |
| **Choreography (Điều phối phân tán)** | Các service giao tiếp qua sự kiện (events). Mỗi service hoàn thành việc sẽ phát event, service tiếp theo lắng nghe và thực hiện. | **Ưu:** Không cần bộ điều phối trung tâm.<br>**Nhược:** Khó theo dõi luồng (workflow) khi hệ thống trở nên phức tạp. |
| **Orchestration (Điều phối tập trung)** | Sử dụng một đối tượng trung tâm (**Orchestrator**) để chỉ đạo. Orchestrator gọi từng service và quyết định bước tiếp theo hoặc lệnh rollback. | **Ưu:** Dễ kiểm soát và quản lý luồng thực thi.<br>**Nhược:** Rủi ro "điểm chết duy nhất" (Single Point of Failure) tại Orchestrator. |

### 4. Các nguyên tắc và thách thức kỹ thuật
- **Giao dịch bù (Compensating Transaction):** Trong Saga, khi một bước đã "Commit" vào DB thì không thể rollback tự động kiểu truyền thống. Do đó, mỗi thao tác phải có một hành động ngược lại (Undo). Ví dụ: Thao tác "Đặt phòng" phải có thao tác "Hủy phòng" tương ứng.
- **Đánh đổi giữa Availability và Consistency:** Theo định lý CAP, hệ thống càng ưu tiên tính sẵn sàng (Availability) thì tính nhất quán dữ liệu tức thời (Strong Consistency) sẽ giảm đi. Saga hướng tới tính nhất quán sau cùng (**Eventual Consistency**).
- **Thiếu tính cô lập (Lack of Isolation):** Dữ liệu của một giao dịch cục bộ sẽ hiển thị ngay lập tức với các service khác dù toàn bộ chuỗi Saga chưa kết thúc.
    * *Giải pháp:* Sử dụng các trạng thái tạm thời như `Pending`, `Processing` hoặc cơ chế khóa thủ công (Semantic Locking).

### 5. Ví dụ minh họa: Quy trình đặt tour du lịch
Giả sử một khách hàng đặt một gói du lịch, quy trình sẽ diễn ra như sau:

| Bước | Microservice thực hiện | Hành động thực tế | Event phát ra |
| :--- | :--- | :--- | :--- |
| 1 | **Order Service** | Tạo đơn hàng mới ($OrderID$) | `OrderCreated` |
| 2 | **Car Rental Service** | Giữ xe cho khách | `CarReserved` |
| 3 | **Hotel Service** | Đặt phòng khách sạn | `HotelBooked` |
| 4 | **Flight Service** | Giữ chỗ vé máy bay | `FlightBooked` |
| 5 | **Payment Service** | Thực hiện thanh toán | `PaymentSuccess` hoặc `PaymentFailed` |

**Kịch bản thất bại:** Nếu bước 5 (Thanh toán) thất bại, Orchestrator (hoặc thông qua Event) sẽ yêu cầu Flight, Hotel và Car Rental thực hiện lệnh **Undo** để giải phóng tài nguyên.

### 6. Lời khuyên khi áp dụng
Việc triển khai Saga không hề đơn giản và đòi hỏi sự đầu tư nghiêm túc:
1.  **Đừng nôn nóng:** Đây là một kỹ thuật nâng cao. Bạn có thể mất vài tháng đến hàng năm để thực sự làm chủ các biến thể của nó.
2.  **Tập trung vào thực hành:** Lý thuyết chỉ là nền tảng, việc áp dụng vào các dự án thực tế mới giúp bạn hiểu rõ các vấn đề về độ trễ, lỗi mạng và trùng lặp event.
3.  **Luôn đặt câu hỏi:** Trước khi áp dụng, hãy tự hỏi: "Hệ thống có thực sự cần phân tán đến mức này không?" vì Saga làm tăng độ phức tạp của mã nguồn và việc debug.

### 7. Danh mục thuật ngữ quan trọng

| Thuật ngữ | Ý nghĩa |
| :--- | :--- |
| **Local Transaction** | Giao dịch cục bộ bên trong một service duy nhất. |
| **Compensating Transaction** | Giao dịch bù (thao tác ngược) dùng để hoàn tác dữ liệu. |
| **Eventual Consistency** | Tính nhất quán sau cùng (dữ liệu sẽ đồng nhất sau một khoảng thời gian). |
| **Orchestrator** | Bộ điều phối trung tâm trong mô hình Orchestration. |
| **Semantic Lock** | Khóa ngữ nghĩa (sử dụng trạng thái để ngăn chặn các thay đổi không hợp lệ). |

*Tóm lại, Saga pattern là "chìa khóa" để giải quyết bài toán giao dịch trong Microservices, nhưng nó yêu cầu tư duy thiết kế cẩn trọng về cả trường hợp thành công lẫn thất bại.*

---

# Microservices Và Twelve-Factor

Nội dung này trình bày về **12 nguyên tắc phát triển ứng dụng Cloud** theo phương pháp **Twelve-Factor App**, đặc biệt khi áp dụng vào kiến trúc Microservices. Tập trung giải thích 8 nguyên tắc đầu tiên (trong tổng số 12) cùng 3 nguyên tắc mở rộng được gọi là **12 Plus**.

### 1. Tổng quan về Twelve-Factor App trong môi trường Cloud

- **Twelve-Factor App** là tập hợp 12 quy tắc cốt lõi giúp thiết kế các ứng dụng Cloud (đám mây) linh hoạt, bền bỉ, có khả năng chịu đựng các ràng buộc về mạng, chi phí và môi trường thực thi.
- **Đặc thù của môi trường Cloud:** Thường có độ trễ mạng cao hơn, băng thông bị giới hạn và phát sinh chi phí khi truyền dữ liệu giữa các khu vực (regions/zones).
- **Mối liên hệ với Microservices:** Microservices có thể được xem là một dạng ứng dụng Cloud tiêu chuẩn, vì hầu hết các dịch vụ này đều chạy trên nền tảng đám mây để phục vụ người dùng từ xa (không phải local). Trong kiến trúc này, mỗi một microservice sẽ đóng vai trò tương đương như một ứng dụng (app) độc lập và cần tuân thủ các nguyên tắc Twelve-Factor.

### 2. Tóm tắt 8 nguyên tắc đầu tiên của Twelve-Factor App

| Nguyên tắc | Mô tả chính |
| :--- | :--- |
| **1. Codebase** (Cơ sở mã nguồn) | Mỗi microservice phải có một **codebase duy nhất**, được quản lý bằng hệ thống kiểm soát phiên bản (Version Control - ví dụ: Git). |
| **2. Dependencies** (Sự phụ thuộc) | Mỗi app phải tự khai báo và đóng gói các thư viện (dependencies) một cách rõ ràng, độc lập để tránh xung đột phiên bản giữa các app. |
| **3. Configuration** (Cấu hình) | Cấu hình ứng dụng phải được lưu trữ **tách biệt hoàn toàn khỏi code**, thường sử dụng các biến môi trường (Environment Variables) hoặc dịch vụ quản lý cấu hình. |
| **4. Backing Services** (Dịch vụ hỗ trợ) | Các dịch vụ bên ngoài (Database, Message Queue, Cache) phải được coi là tài nguyên gắn kèm (attached resources) và truy cập thông qua mạng. |
| **5. Build, Release, Run** (Xây dựng, Phát hành, Chạy) | Quá trình triển khai phải tách biệt rõ ràng ba giai đoạn: Build (biên dịch code), Release (kết hợp code đã build với cấu hình), và Run (chạy ứng dụng). |
| **6. Processes** (Tiến trình) | Mỗi microservice chạy dưới dạng một hoặc nhiều **tiến trình phi trạng thái (stateless)**. Mọi trạng thái cần lưu trữ phải được đẩy ra một dịch vụ hỗ trợ (ví dụ: Database, Redis). |
| **7. Port Binding** (Gắn kết cổng) | Microservice tự cung cấp dịch vụ của nó bằng cách lắng nghe trên một cổng (port) cụ thể và ánh xạ ra bên ngoài. |
| **8. Concurrency** (Đồng thời) | Ứng dụng phải hỗ trợ mở rộng theo chiều ngang (Horizontal Scaling) bằng cách thêm nhiều phiên bản (instances) thay vì phụ thuộc vào việc nâng cấp cấu hình phần cứng (Vertical Scaling). |

### 3. Các điểm nhấn quan trọng cần lưu ý

- **Quản lý codebase và dependencies độc lập** giúp các nhóm lập trình làm việc song song mà không dẫm chân lên nhau hay gây xung đột mã nguồn/thư viện.
- **Tách cấu hình khỏi code** giúp bạn dễ dàng thay đổi cấu hình cho từng môi trường (Dev, Test, Prod) mà không cần phải build lại mã nguồn.
- **Tách rời Backing Services** giúp giảm sự phụ thuộc cứng. Khi cần thiết, bạn có thể dễ dàng thay đổi hoặc mở rộng Database/Cache mà không ảnh hưởng đến code của Microservice.
- **Quy trình Build-Release-Run một chiều** không cho phép chỉnh sửa code trực tiếp trên môi trường Run (không hỗ trợ undo release), nhưng lại hỗ trợ an toàn việc rollback (quay hệ thống về bản phát hành ổn định trước đó) khi xảy ra sự cố.
- **Stateless Process (Tiến trình phi trạng thái)** là yếu tố sống còn để đảm bảo hệ thống có thể scale lên xuống linh hoạt mà không làm mất dữ liệu người dùng.

### 4. Kết luận

- **Twelve-Factor App** chính là bộ tiêu chuẩn vàng để thiết kế các ứng dụng Cloud Native hiện đại, và nó hoàn toàn khớp với triết lý của Microservices.
- Việc tuân thủ nghiêm ngặt các nguyên tắc này giúp ứng dụng của bạn linh hoạt, dễ dàng mở rộng, tự động hóa khâu triển khai và giảm thiểu rủi ro bảo trì trên Cloud.
