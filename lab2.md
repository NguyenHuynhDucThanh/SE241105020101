1. Single Responsibility Principle (SRP)

Nguyên tắc này yêu cầu một lớp chỉ có một lý do để thay đổi, tức là một trách nhiệm duy nhất.

Ứng dụng trong BSA:

Tách các trách nhiệm thành các lớp riêng biệt:
* BankStatementCSVParser: Chịu trách nhiệm đọc và phân tích dữ liệu từ tệp CSV.
* BankStatementProcessor: Xử lý dữ liệu giao dịch, tính toán tổng chi phí, tổng lợi nhuận, hoặc lọc giao dịch theo tháng.
* BankStatementAnalyzer: Kết hợp các phần và trả kết quả cho người dùng.

Lợi ích:
* Mỗi lớp đơn giản hơn, dễ hiểu và dễ bảo trì.
* Thay đổi một trách nhiệm không ảnh hưởng đến các phần khác. Ví dụ: thay đổi định dạng tệp sang JSON chỉ cần sửa BankStatementCSVParser.

2. Cohesion (Tính kết nối chặt chẽ)

Cohesion đo mức độ các thành phần trong một lớp liên quan đến nhau. Lớp có tính kết nối cao khi các phương thức hỗ trợ một mục đích cụ thể.

Ứng dụng trong BSA:

Lớp BankStatementCSVParser có tính kết nối cao:
* Tất cả các phương thức (như parseFromCSV() và parseLinesFromCSV()) liên quan đến việc phân tích dữ liệu CSV.
* Lớp BankStatementProcessor tập trung vào xử lý giao dịch, như:
* calculateTotalAmount(): Tính tổng giao dịch.
* calculateTotalInMonth(): Tính giao dịch trong một tháng.
* calculateTotalForCategory(): Tổng số tiền theo danh mục.

Lợi ích:
* Dễ hiểu vì các phương thức trong lớp thực hiện nhiệm vụ tương tự.
* Dễ mở rộng mà không gây ảnh hưởng đến logic không liên quan.

3. Coupling (Tính phụ thuộc lẫn nhau)

Coupling đo mức độ các lớp phụ thuộc vào nhau. Mục tiêu là giảm Coupling để dễ thay đổi và bảo trì.

Ứng dụng trong BSA:
* Giảm coupling bằng cách sử dụng interface:
* BankStatementParser: Là interface trừu tượng, cho phép hỗ trợ các định dạng khác như JSON hoặc XML mà không làm thay đổi BankStatementAnalyzer.
* BankStatementAnalyzer không phụ thuộc vào một lớp cụ thể mà chỉ phụ thuộc vào interface BankStatementParser.

Lợi ích:
* Tăng tính linh hoạt: có thể thêm định dạng mới (như JSON) mà không cần thay đổi code ở lớp phân tích chính.
* Giảm ảnh hưởng khi thay đổi, ví dụ: nếu thay đổi cách đọc CSV, các lớp khác không bị tác động.

![Diagram](https://www.planttext.com/api/plantuml/png/p5DBRi8m4Dtx55w6HJX0L24e5L8bMY61kX-SWLeuDh8dbUZdP5tqIBr2RISKWnBKPRE8Pk9vRsPUdhy_lyQEm59TASQa9hGR4CXlGU-M18GLQbg0TMZv6-F-sOTaPasLAQcnu4koKcy7HOIiB6W7WgJHf-AvhtE_25VmkNHqq-16WpQzG8_O1sD2WNFdeqoNJ_zeceKr0fqppJGvNv_3N1zR64Q04hNoH2j3e2QLgJLbldzfwhN2Zf9x_M0qFMTLU1Sy9jVgPyPEVRmrzf29DaW4Qd7TYqqqgCChfdjTkA7eiaYS0Xfijf4A7w5AVygBaULvnqIMjbVf6RrknYzpvyFu3Q8woQw-39smSX-0nmQn-nOfm8AIAKSyw1Gv04vSuR1bytDsQvBdeyxbUumw7Bx_eVuA-z5ahCTxHqHgTpP6NOOx_GC00F__0m00)

Giải thích cách tác giả sử dụng để giải quyết các vấn đề về: SRP, cohesion, coupling

1. Single Responsibility Principle (SRP)
Vấn đề được đặt ra: Trong tài liệu, ban đầu tác giả trình bày việc thực hiện tất cả các chức năng trong một lớp đơn (God Class). Điều này dẫn đến mã nguồn khó hiểu, khó bảo trì và không linh hoạt khi thêm yêu cầu mới.

Cách giải quyết của tác giả: Tách trách nhiệm thành các lớp nhỏ hơn, mỗi lớp xử lý một nhiệm vụ duy nhất:

BankStatementAnalyzer: 
* Lớp chính điều phối toàn bộ hệ thống. 
* Nó chịu trách nhiệm: Gọi các lớp khác để phân tích dữ liệu.
* Tổng hợp kết quả từ BankStatementProcessor và trả về cho người dùng.
  
BankStatementCSVParser:
* Chịu trách nhiệm đọc file CSV và chuyển đổi dữ liệu thành danh sách BankTransaction.
* Mọi logic liên quan đến việc xử lý định dạng file CSV đều được thực hiện tại đây.

BankStatementProcessor:
* Chịu trách nhiệm xử lý logic kinh doanh như:
* Tính tổng số tiền (calculateTotalAmount).
* Tính tổng giao dịch trong một tháng (calculateTotalInMonth).
* Tính tổng số tiền trong một danh mục (calculateTotalForCategory).
* BankTransaction: Đóng vai trò mô hình dữ liệu (domain model), lưu trữ thông tin của mỗi giao dịch (ngày, số tiền, mô tả).
  
Lợi ích của SRP trong tài liệu:

Mã nguồn dễ bảo trì:
* Nếu cần thêm logic cho một loại định dạng file mới (ví dụ JSON), chỉ cần thêm một lớp parser mới (ví dụ BankStatementJSONParser) mà không ảnh hưởng đến các lớp khác.
Dễ dàng mở rộng: 
* Tách riêng logic kinh doanh (BankStatementProcessor) khỏi logic xử lý file (BankStatementCSVParser).
* Logic kinh doanh có thể hoạt động với mọi loại parser, miễn là parser tuân theo interface BankStatementParser.
Tăng độ rõ ràng:
* Người phát triển mới có thể dễ dàng hiểu vai trò của từng lớp trong hệ thống.

2. Cohesion (Tính kết nối chặt chẽ)

Vấn đề được đặt ra: Cohesion thấp xảy ra khi một lớp hoặc phương thức đảm nhận quá nhiều chức năng không liên quan, dẫn đến mã khó hiểu và không hiệu quả.

Cách giải quyết của tác giả:

Tăng cường Cohesion bằng cách nhóm các chức năng liên quan vào cùng một lớp:
* BankStatementCSVParser: Chỉ tập trung vào xử lý file CSV, cung cấp hai phương thức liên quan:
* parseFrom(String line):  Phân tích một dòng CSV.
* parseLinesFrom(List<String> lines):  Phân tích toàn bộ file CSV.
Cả hai phương thức đều liên quan đến xử lý file CSV, đảm bảo tính kết nối chặt chẽ.
* BankStatementProcessor: Tập trung vào xử lý logic kinh doanh liên quan đến danh sách giao dịch:
* calculateTotalAmount:  Tính tổng số tiền từ các giao dịch.
* calculateTotalInMonth:  Lọc và tính tổng giao dịch trong một tháng cụ thể.
* calculateTotalForCategory:  Tính tổng số tiền trong một danh mục.
* BankTransaction: Là mô hình dữ liệu đơn giản, chỉ chứa các thuộc tính liên quan đến giao dịch: date, amount, description.

Lợi ích của Cohesion trong tài liệu:
* Tăng tính rõ ràng và dễ hiểu: Mỗi lớp có một vai trò rõ ràng, các phương thức trong lớp phục vụ cùng một mục tiêu.
* Dễ dàng kiểm thử: Các chức năng được tách biệt, việc kiểm thử từng lớp trở nên đơn giản và ít phụ thuộc.
* Giảm rủi ro khi mở rộng: Thêm hoặc thay đổi logic xử lý file sẽ không ảnh hưởng đến lớp logic kinh doanh hoặc lớp mô hình dữ liệu.

3. Coupling (Tính phụ thuộc lẫn nhau)

Vấn đề được đặt ra:  Khi các lớp phụ thuộc mạnh mẽ vào nhau, thay đổi ở một lớp có thể gây ra lỗi hoặc yêu cầu thay đổi ở nhiều lớp khác.

Cách giải quyết của tác giả:

Sử dụng interface để giảm Coupling:
* BankStatementParser: Là một interface trừu tượng, định nghĩa cách phân tích file giao dịch.
* Các lớp parser cụ thể (như BankStatementCSVParser) chỉ cần thực hiện interface này.
* BankStatementAnalyzer không phụ thuộc vào một parser cụ thể, mà làm việc với interface BankStatementParser. 
* Điều này cho phép dễ dàng thay thế hoặc thêm các parser mới (ví dụ BankStatementJSONParser).

Phân tách trách nhiệm giữa các lớp:
* BankStatementAnalyzer chỉ tập trung vào luồng xử lý chính, không cần biết chi tiết file được phân tích như thế nào.
* BankStatementProcessor chỉ làm việc với danh sách BankTransaction, không quan tâm file được xử lý như thế nào.

 Lợi ích của Coupling thấp trong tài liệu:
* Dễ dàng mở rộng: Có thể thêm parser mới (như JSON hoặc XML) mà không cần sửa đổi lớp BankStatementAnalyzer.
* Giảm lỗi khi thay đổi: Thay đổi logic trong BankStatementCSVParser không ảnh hưởng đến BankStatementProcessor.
* Tăng tính linh hoạt: Hệ thống có thể xử lý nhiều loại định dạng file mà không cần sửa đổi phần lõi.
