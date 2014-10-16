dd_command_in_linux
===================

Mục lục:

1. Mở đầu và Khuyến nghị
2. Khái niệm và ứng dụng của câu lệnh dd
3. Cú pháp và các trường tùy chọn
4. Các ví dụ

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
|bs=Bytes |Quá trình đọc (ghi) bao nhiêu byte một lần đọc (ghi) |
|cbs=Bytes|Chuyển đổi bao nhiêu byte một lần |
|count=Blocks | thực hiện bao nhiêu Block trong quá trình thực thi câu lệnh |
|if | Chỉ dường dẫn đọc đầu vào |
|of | Chỉ đường dẫn ghi đâu ra|
|ibs=bytes | Chỉ ra số byte một lần đọc |
|obs=bytes | Chỉ ra số byte một lần ghi |
|skip=bloc | Bỏ qua bao nhiêu block đầu vào |
|conv=Convs | Chỉ ra tác vụ cụ thể của câu lệnh, các tùy chọn được ghi dưới bảng sau đây |

**Các tùy chọn của conv**

|Tùy chọn | Tác dụng  |
|-----------|----------|
|ascii | Chuyển đôi từ mã EBCDIC sáng ASCII |
|ebcdic | Chuyển đổi từ mã ASCII sang EBCDIC |
|lcase | Chuyển đổi từ chữ thường lên hết thành chữ in hoa |
|ucase | Chuyển đổi từ chữ in hoa sang chữ thường |
|nocreat | Không tạo ra file đầu ra |
|noerror | tiếp tục sao chép dữ liệu khi đầu vào bị lỗi |
|sync | Đồng bộ dữ liệu với ô đang sao chép sang |


*Lưu ý:* Khi bạn định dạng số lượng byte mỗi lần đọc. Mặc định nó được tính theo đơn vị là kb. Bạn có thể thêm một số trường sau để báo định dạng khác:

- c = 1 byte
- w = 2 byte
- b = 512 byte
- kB = 1000 byte 
- K = 1024 byte 
- MB = 1000000 byte
- M = (1024 * 1024) byte
- GB = (1000 * 1000 * 1000) byte
- G = (1024 * 1024 * 1024) byte

#### 4. Các ví dụ:
###### a. Sao lưu - phục hồi toàn bộ ổ cứng hoặc phân vùng trong ô cứng.
- Sao lưu toàn bộ dữ liệu ổ cứng sao ổ cứng khác:
```
#dd if=/dev/sda of=/dev/sdb conv=noerror,sync
```
Câu lệnh này dùng dể sao lưu toàn bộ dữ liệu của ổ sda sang ổ sdb với tùy chọn trong trường conv=noerrom.sync với ý ngĩa vẫn tiếp tục sao lưu nếu dữ liệu đầu vào bị lỗi và tự động đồng bộ với dữ liệu sdb

- Tạo một file image cho ổ sda1. Các này sẽ nhanh hơn là viêc chuyển dữ liệu sao ổ khác
```
dd if=/dev/sda1 of=/root/sda1.img 
```
- Nếu muốn nén ảnh file anh vào bạn có thể sử dụng command sau
```
dd if=/dev/sda1 | grip > /root/sda1.img.gz
```
-Sao lưu dữ liệu từ một phân vùng này đến một phân vùng khác
```
dd if=/dev/sda2 of=/dev/sdb2 bs=512 conv=noerror,sync
```
*Đối với câu lệnh này bs=512 có ý nghĩa mỗi lần đọc ghi nó đọc và ghi 512 byte*
- Phục hồi dữ liệu 
```
dd if=/root/sda1.img of=/dev/sda1
```
- Sao lưu từ CDroom
```
dd if=/dev/cdrom of=/root/cdrom.img conv=noerror
```

###### b.Sao lưu phục hồi MBR
Việc sao lưu lại mbr là việc làm cần thiết đối với hệ thống linux. nó đề phòng cho việc khi virut có thể nhảy được hẳn vào vùng mbr. Lúc bày bất kì một phần mềm diệt virut nào cũng không diệt được con virut này. Cách hay nhất là cài đặt lại mbr và lúc đó việc sao chép mbr lúc trước khi nhiễm sẽ phát huy tác dụng:
- Sao chép mbr
```
dd if=/dev/sda1 of=/root/mbr.txt bs=512 count=1
```
- Phục hồi lại mbr
```
dd if=/root/mbr.txt of=/dev/sda1
```
###### c. Chuyển đổi chữ thường thành chữ in hoa
- Chuyển chữ thường thành chứ in hoa
```
dd if=/root/test.doc of=/root/test1.doc conv=ucase
```
<img class="image__pic js-image-pic" src="http://i.imgur.com/ihXZb4z.png" alt="" id="screenshot-image">

- Chuyển chứ hoa thành chứ thường
```
dd if=/root/test1.doc of=/test2.doc conv=scase,sycn
```
###### d. Tạo một file có dung lượng cố định 
Tạo ra một file có kích thước 100M
```
dd if=/dev/zero of=/root/file1 bs=100M count=1
```

#### 5. Các tình huống áp dụng trong thực tế

Các vi dụ tôi vừa nêu trên đều sử dụng nhiều trong thực tế. Ngoài ra còn kết hợp với một số câu lệnh để làm thêm tác vụ khác

- VD1: Kết hợp với câu lệnh mkswap để tạo phân vùng swap cho máy 
    - Sử dụng câu lênh dd để tạo một phân vùng trống có kích cỡ 1G:
```
dd if=/dev/zero of=/root/swap bs=1024M count=1
```

<img class="image__pic js-image-pic" src="http://i.imgur.com/ULIQkPh.png" alt="" id="screenshot-image">

  - Gán quyền cho nó chỉ root mới vào xem được
```
chmod 600 /root/swap
```
<img class="image__pic js-image-pic" src="http://i.imgur.com/nllxHrl.png" alt="" id="screenshot-image">

- Chỉ cho đến vùng swap
```
mkswap /root/swap
```
```
swapon /root/swap 
```
<img class="image__pic js-image-pic" src="http://i.imgur.com/f6taOEY.png" alt="" id="screenshot-image">

Oki nào bây giờ kiểm tra lại xem thành công chưa. Sử dụng lệnh
```
swapon -s
```



