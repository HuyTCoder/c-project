CÁC THÀNH PHẦN CỦA LINUX NHÚNG

Lựa chọn Linux:
- Định luật Moore: mật độ thành phần chip sẽ tăng gấp đôi sau mỗi 2 năm
- Linux có đủ chức năng cần thiết: lập lịch, ngăn xếp,hỗ trợ USB, Wifi, …
- Đc chuyển sang nhiều loại kiến trúc bộ xử lý: SoC-Arm, MIPS, x86, PowerPC
- Mã nguồn mở: tự do lấy và sửa đổi, ko có ràng buộc
- Có cộng đồng tích cực, đc cập nhật và hỗ trợ cho phần cứng, giao thức và tiêu chuẩn hiện tại

Ko lựa chọn Linux:
- Phức tạp
- Yêu cầu nhiều tài nguyên hơn so với phương pháp điều hành thời gian thực truyền thống (RTOS)
- Cần bộ kĩ năng phù hợp, khả năng diễn giải kết quả
- Vấn đề liên quan đến yêu cầu phê duyệt theo quy định (oto, máy bay,...)

Các bên tham gia (Players):
- Cộng đồng mã nguồn mở (Open source community)
- Các nhà kiến trúc CPU (CPU architects): Các tổ chức thiết kế CPU (Arm/Linaro, Intel, SiFive, IBM,...)
- Các nhà cung cấp SoC (SoC vendors) (Broadcom, Intel, Qualcomm,...): Lấy nhân kernel và toolchain từ các CPU architect và sửa đổi để hỗ trợ cho con chip của họ; tạo các bo mạch tham chiếu (reference boards) để tạo ra các bo mạch phát triển và các sản phẩm hoạt động đc
- Các nhà cung cấp bo mạch và OEM (Board vendors, OEMs): lấy bo mạch tham chiếu từ SoC vendors để chế tạo các sản phẩm cụ thể như camera, hoặc tạo ra bo mạch phát triển đa dụng hơn (BeagleBoard, Raspberry Pi,...)
- Các nhà cung cấp Linux thương mại (Commercial Linux vendors): cung cấp bản phân phối LInux thương mại đã trải qua quá trình xác minh và kiểm định theo quy định nghiêm ngặt trong nhiều ngành công nghiệp như y tế, ô tô

- Phần mềm nhúng đi qua một dây chuyền từ Cộng đồng mã nguồn mở -> Nhà thiết kế CPU -> Nhà cung cấp SoC -> Nhà sản xuất bo mạch. Bạn là lập trình viên ở cuối chuỗi này nên bị phụ thuộc, không thể cứ thích là tải phần mềm (như Linux kernel) mới nhất về dùng được ngay.
- Các hãng làm chip (SoC) thường lười đóng góp mã nguồn (upstream) ngược lại cho cộng đồng và chỉ tập trung hỗ trợ các dòng chip mới nhất. Hậu quả là các thiết bị nhúng thường phải chạy phần mềm cũ rích, thiếu tính năng và dính nhiều lỗ hổng bảo mật nghiêm trọng.
- Giải pháp: chọn lọc kỹ càng, kiểm tra chính sách cập nhật phần mềm của từng nhà cung cấp, ưu tiên chăm cập nhật và hỗ trợ cộng đồng tốt; chủ động tìm hiểu hệ thống để tự xoay xở, thay vì chọn một cách mù quáng

Đi qua vòng đời của dự án (Moving through the project life cycle)
- Giai đoạn này thường đan xen và lặp lại với nhau chứ không diễn ra theo một đường thẳng cứng nhắc
- Các thành phần của Linux Nhúng: thiết lập môi trường phát triển và tạo ra một nền tảng hoạt động được cho các giai đoạn sau (giai đoạn đưa bo mạch vào hoạt động board bring-up).
- Kiến trúc Hệ thống và Lựa chọn Thiết kế (System Architecture and Design Choices): xem xét một số quyết định thiết kế mà bạn sẽ phải đưa ra liên quan đến việc lưu trữ chương trình và dữ liệu, cách phân chia công việc giữa các trình điều khiển thiết bị (device drivers) trong nhân (kernel) và các ứng dụng, cũng như cách khởi tạo hệ thống.
- Viết Ứng dụng Nhúng (Writing Embedded Applications): hướng dẫn cách đóng gói và triển khai các ứng dụng Python, sử dụng hiệu quả mô hình luồng (thread) và tiến trình (process) của Linux, và cách quản lý bộ nhớ trong một thiết bị bị hạn chế về tài nguyên.
- Gỡ lỗi và Tối ưu hóa Hiệu suất (Debugging and Optimizing Performance): mô tả cách truy vết (trace), đo lường hiệu suất (profile) và gỡ lỗi (debug) mã nguồn của bạn trong cả ứng dụng lẫn nhân hệ thống (kernel), đồng thời cách thiết kế cho các hành vi thời gian thực (real-time behavior) khi có yêu cầu.

Bốn thành phần của Linux nhúng
- Chuỗi công cụ (Toolchain): Trình biên dịch (compiler) và các công cụ khác cần thiết để tạo mã (code) cho thiết bị mục tiêu của bạn
- Bộ nạp khởi động (Bootloader): Chương trình khởi tạo bo mạch và tải nhân Linux (Linux kernel)
- Nhân (Kernel): Đây là trái tim của hệ thống, quản lý tài nguyên hệ thống và giao tiếp với phần cứng
- Hệ thống tập tin gốc (Root filesystem): Chứa các thư viện và chương trình được chạy sau khi nhân đã hoàn thành việc khởi tạo
- Mã nguồn mở (Open Source):
- Giấy phép (Licenses): Mã nguồn mở cho phép xem, sửa, và phân phối lại mã nguồn, khác với freeware (chỉ cho dùng miễn phí bản build sẵn, giấu code) hay phần mềm miễn phí phi thương mại
- 2 loại:
    + Permissive (như MIT, BSD,...): có thể sửa đổi mã nguồn và sử dụng nó trong các hệ thống do bạn lựa chọn, bao gồm cả việc tích hợp nó vào các hệ thống có thể là độc quyền (proprietary systems),  miễn là bạn không sửa đổi các điều khoản của giấy phép dưới bất kỳ hình thức nào
    + Copyleft (như GNU General Public License - GPL): bắt buộc bạn phải chuyển giao quyền lấy và sửa đổi phần mềm cho người dùng cuối của bạn, tức là phải chia sẻ mã nguồn của mình; không thể kết hợp mã GPL vào các chương trình độc quyền

Lựa chọn phần cứng
- Một kiến trúc CPU phải được nhân (kernel) hỗ trợ, thường thấy nhất là Arm, MIPS, PowerPC và x86, mỗi loại đều có biến thể 32 và 64-bit, tất cả đều có bộ quản lý bộ nhớ (MMUs - memory management units).
- Một dung lượng RAM hợp lý (16 MiB là tối thiểu tốt).
- Bộ nhớ không bay hơi (non-volatile storage), thường là bộ nhớ flash (bộ nhớ NAND/NOR thô, thẻ nhớ SD, eMMC hay USB)
- Một cổng nối tiếp (serial port), tốt nhất là cổng nối tiếp dựa trên UART.
- Một số phương tiện để tải phần mềm (cổng giao tiếp JTAG - Joint Test Action Group hoặc khả năng boot trực tiếp từ thẻ SD, USB, UART)
- Các giao diện đến các phần cứng cụ thể mà thiết bị của bạn cần để thực hiện công việc