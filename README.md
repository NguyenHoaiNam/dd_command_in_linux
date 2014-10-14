dd_command_in_linux
===================

Mục lục:

1. Mở đầu và Khuyến nghị
2. Khái niệm và ứng dụng của câu lệnh dd
3. Cú pháp và các trường tùy chọn
4. Lab

#### 1. Mở đầu và khuyến nghị

Xin chào các bạn. Hôm nay tôi sẽ giới thiệu một command dd trong hệ thống Linux. Để có thể hiểu hết được ý nghĩa của câu lệnh này và các tùy chọn của câu lệnh trước tiên bạn cần phải có kiến thức và cách tổ chức lưu trư dữ liệu trong ô cứng, hiều về các sector,tracks, Cylinders,.. các thuật ngũ liên quan đến ổ cứng, và kiến thức về MBR...

#### 2. Khái niệm và ứng dụng của câu lệnh
Câu lệnh `dd` trong linux là một trong những câu lệnh thường xuyên được sử dụng. Câu lệnh `dd` dùng để sử dụng trong các trường hợp sau:

- Sao lưu và phục hồi toàn bộ dữ liệu ổ cứng hoặc một partition 
- Chuyển đổi định dạng dữ liệu từ ASCII sang EBCDIC hoặc ngược lại
- Sao lưu lại MBR trong máy (MBR là một file dữ liệu rất quan trong nó chứa các lệnh để LILO hoặc GRUB nạp hệ điều hanh)
- Chuyển đổi chữ thường sang chữ hoa và ngược lại
- Tạo một file với kích cơ cô định
- Tạo một file ISO 

#### 3. Cú pháp và các trường tùy chọn
###### a. Cú pháp
```
#dd if=<source> of=<target> option
```
Trong đó:
- if=<soure> địa chỉ nguồn của dữ liệu nó sẽ bắt đầu đọc
- of=<targer> viết đầu ra của file
- option : các tùy chọn cho câu lệnh

###### b. Các tùy chọn
|Tùy chọn | Ý nghĩa |
|---------|---------|
|bs=bytes| đọc bao nhiêu byte cùng một lúc |
|count=Blocks| Số lượng đọc hoặc ghi theo blocks |
|ibs| tương tự như tùy chọn bs, nếu không có Bytes thì mặc định là 512 |
|obs| viết bao nhiêu byte cùng một lúc |

