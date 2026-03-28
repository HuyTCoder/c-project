## Chương 3: Bootloaders

Bootloader (Bộ nạp khởi động) là thành phần thứ hai của Linux nhúng. Nó là phần khởi động hệ thống và nạp nhân hệ điều hành (kernel). Trong chương này, chúng ta sẽ xem xét vai trò của bootloader và đặc biệt là cách nó chuyển giao quyền điều khiển từ chính nó sang kernel bằng cách sử dụng một cấu trúc dữ liệu được gọi là device tree (cây thiết bị), còn được gọi là flattened device tree hay FDT. Tôi sẽ đề cập đến những kiến thức cơ bản về device tree, vì điều này sẽ giúp bạn theo dõi các kết nối được mô tả trong device tree và liên hệ nó với phần cứng thực tế.

## Yêu cầu kỹ thuật
# Công cụ và ví dụ
- Bootloader phổ biến: U-Boot
- Ví dụ sử dụng: BeagleBone Black
- Cần chuẩn bị:
    + Toolchain (Crosstool-NG)
    + Linux host + tool build
    + Thẻ nhớ, cáp serial, nguồn, board

# Bootloader là gì?
Là thành phần khởi động hệ thống Linux nhúng.
Nhiệm vụ chính: nạp kernel và chuyển quyền điều khiển cho kernel.

# Vai trò chính của Bootloader
- Bootloader có 2 nhiệm vụ cốt lõi: Khởi tạo hệ thống ở mức cơ bản
- Ban đầu hệ thống rất “trống”:
    + Chưa có RAM (DRAM chưa init)
    + Chưa truy cập được bộ nhớ (NAND, MMC…)
    + Chỉ có: CPU, RAM nhỏ trên chip, Boot ROM
- Nạp kernel vào RAM và chạy nó
- Sau khi đủ điều kiện tối thiểu → load kernel
- Chuẩn bị môi trường để kernel hoạt động

# Quá trình khởi động (Boot sequence)
- Gồm nhiều giai đoạn (phases), mỗi giai đoạn sẽ kích hoạt thêm phần cứng
- Kết thúc khi:
    + Kernel được load vào RAM
    + Bootloader trao quyền điều khiển cho kernel

# Giao tiếp Bootloader ↔ Kernel
Bootloader phải truyền 2 thông tin quan trọng:
1. Device Tree (FDT)
- Mô tả phần cứng hệ thống
- Giúp kernel hiểu thiết bị đang chạy trên gì
2. Kernel command line
- Chuỗi cấu hình để điều khiển hành vi kernel

# Sau khi kernel chạy
- Bootloader không còn cần thiết
- Bộ nhớ nó dùng có thể giải phóng

# Chức năng phụ của Bootloader
- Cung cấp chế độ bảo trì (maintenance mode):
- Cập nhật firmware / boot image
- Thay đổi cấu hình boot
- Chạy chẩn đoán lỗi
- Thường dùng qua Serial console (cổng UART)



## Trình tự khởi động (Boot Sequence)
- Trong hệ thống Linux nhúng, quá trình khởi động không diễn ra trong một bước duy nhất mà được chia thành nhiều giai đoạn. Mỗi giai đoạn có nhiệm vụ riêng và chuẩn bị điều kiện cho giai đoạn tiếp theo.
- Trình tự đầy đủ:
Boot ROM
→ SPL (khởi tạo RAM)
→ U-Boot (nạp kernel)
→ Linux Kernel (chạy hệ điều hành)

# Khái niệm cốt lõi
- Khi thiết bị được cấp nguồn hoặc reset, CPU sẽ bắt đầu thực thi từ một vị trí cố định gọi là reset vector.
- Từ đây, hệ thống bắt đầu chuỗi khởi động gồm nhiều bước, thường gọi là multi-stage boot.
- Nguyên lý chung: Hệ thống sử dụng một chương trình rất nhỏ để khởi động chương trình lớn hơn, cho đến khi đủ khả năng chạy hệ điều hành.

# Các giai đoạn chính trong Boot Sequence
# Giai đoạn 1: Boot ROM
- Là chương trình được tích hợp sẵn trong chip (SoC).
- Không thể thay đổi.
- Nhiệm vụ:
    + Xác định thiết bị lưu trữ (NAND, eMMC, SD card…)
    + Tìm và nạp chương trình khởi động đầu tiên vào SRAM
- Đặc điểm:
    + Rất nhỏ
    + Chỉ làm nhiệm vụ tối thiểu

# Giai đoạn 2: SPL (Secondary Program Loader)
- Được nạp vào SRAM (bộ nhớ nhỏ trong chip).
- Là phiên bản rút gọn của bootloader.
- Nhiệm vụ chính:
    + Khởi tạo DRAM (RAM chính)
    + Chuẩn bị môi trường đủ để chạy bootloader đầy đủ
    + Nạp U-Boot (full) vào DRAM
- Đặc điểm:
    + Bị giới hạn dung lượng
    + Chỉ chứa các chức năng thiết yếu

# Giai đoạn 3: U-Boot (Bootloader đầy đủ)
- Chạy trong DRAM, nên có nhiều tài nguyên hơn.
- Đây là bootloader chính trong nhiều hệ thống Linux nhúng.
- Nhiệm vụ:
    + Nạp Linux kernel
    + Nạp Device Tree (FDT)
    + Truyền kernel command line
    + Cung cấp giao diện dòng lệnh (CLI) qua serial
- Ngoài ra:
    + Cho phép debug
    + Cập nhật firmware
    + Tùy chỉnh quá trình boot

# Giai đoạn 4: Linux Kernel
- Được nạp vào RAM và bắt đầu thực thi.
- Nhiệm vụ:
    + Khởi tạo toàn bộ phần cứng
    + Mount filesystem
    + Chạy hệ điều hành và ứng dụng
- Sau bước này:
    + Bootloader không còn vai trò
    + Quyền điều khiển thuộc hoàn toàn về kernel