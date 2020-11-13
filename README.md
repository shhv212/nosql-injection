# NoSQL Injection

## Mục lục   

[I. Giới thiệu tổng quan về NoSQL injection](#introduction)  

[II. Thực hành demo khai thác](#demo)  

[III. Cách phòng chống NoSQL injection trên thực tế](#prevent)  


=========================================

<a name="introduction"></a>  
## I. Giới thiệu tổng quan về NoSQL injection  

Đây là slide về những phần kiến thức căn bản về hệ cơ sở dữ liệu NoSQL, các lổ hổng có thể gặp của CSDL NoSQL và các cách khai thác để tấn công NoSQL injection.  

[Slide_NoSQL_injeciton](https://docs.google.com/presentation/d/1sp9oXrkQiTK3BM69hdF3PgqnNy0uKkO7ILkYgMBKHDE/edit?usp=sharing)  

<a name="demo"></a>  
## II. Thực hành demo khai thác  



<a name="prevent"></a>  
## III. Cách phòng chống NoSQL injection trên thực tế  

Sau đây là phần trình bày về việc set up một web login form sử dụng CSQL MongoDB. Ta sẽ tìm hiểu về cách hoạt động của nó như thế nào để chống lại tấn công NoSQL injection.  

Đầu tiên, bạn cần tham khảo trên mạng về cách cài đặt MongoDB và các packages cần thiết nếu có:  

<img src="https://i.imgur.com/cSU6aan.png">  

Tiếp sau đó ta khởi động MongoDB lên:  

<img src="https://i.imgur.com/01vvaIV.png">  

<img src="https://i.imgur.com/NBVWhw3.png">  

Đây là phần source code của login form này được viết bằng JavaScript và sử dụng cơ sở dữ liệu là MongoDB.

<img src="https://i.imgur.com/7fOuvzA.png">  

Chúng ta cần cấu hình file `database.js` trong thư mục `config` trỏ đến database `uit` là chúng ta đã tạo trước đó.

<img src="https://i.imgur.com/9XSDyD7.png">  

<img src="https://i.imgur.com/0Uusyyu.png">  

Trong db `uit` trước đó ta đã tạo trước 2 users là:  
- username: sang và password: sang123456  
- username: ketoan và password: ketoan123456  

Ta có thể kiếm tra các user bằng lệnh trong MongoDB (phần password đã được mã hóa).

<img src="https://i.imgur.com/Y5ygCAB.png">  

Cài đặt `npm` là công cụ tạo và quản lý các thư viện lập trình JS trong NodeJS. Sau đó là khởi chạy project:  

<img src="https://i.imgur.com/dXeFIrz.png">  

Ta thấy được project đã được kết nối tới database.  

<img src="https://i.imgur.com/dXeFIrz.png">  

Mở browser lên và truy cập vào `localhost` với port 3000 để thấy kết quả:  

<img src="https://i.imgur.com/WwrLdtP.png">  

Ta thực hiện log in vào form này với user đã được tạo trước đó:  

<img src="https://i.imgur.com/pNDip6Q.png">  

Thành công !!!

<img src="https://i.imgur.com/YzCloxZ.png">  

Bây giờ là lúc chúng ta đóng vai 1 attacker và thử tấn công NoSQL injection vào hệ thống. Ta dùng Burp Suite để bắt các gói request và response để phân tích chúng:  

<img src="https://i.imgur.com/ExEuOrV.png">  

Tuy nhiên chúng ta thử rất nhiều cách tấn công NoSQL injection khác mà vẫn thấy không có kết quả. Ta đoán rằng có thể source code này đã fix được lỗi bypass login.  

<img src="https://i.imgur.com/9P2SARs.png">  

Vào trong code để tìm hiểu, ta thấy rằng khi `username` và `password` được gửi tới thì chỉ có `username` được đưa vào câu truy vấn lấy dữ liệu từ database. Sau khi kiểm tra có một object có `username` trùng với `username` mình nhập vào thì nó mới lấy `password` của user lên đó để so sánh với `password` của mình nhập vào. Do đó các câu truy vấn có chứa NoSQL injection không trực tiếp vào database nên không thể thực hiện injection.  

<img src="https://i.imgur.com/TRUV7CE.png">  

