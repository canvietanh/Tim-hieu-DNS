#DOMAIN NAME SYSTEM

##I. Tìm hiểu lý thuyết

###1. Định nghĩa tên miền:
Mỗi máy tính, thiết bị mạng tham gia vào mạng Internet đều giao tiếp với nhau bằng địa chỉ IP (Internet Protocol) . Để thuận tiện cho việc sử dụng và dễ nhớ ta dùng tên (domain name) để xác định thiết bị đó. Hệ thống tên miền DNS (Domain Name System) được sử dụng để ánh xạ tên miền thành địa chỉ IP. Vì vậy, khi muốn liên hệ tới các máy, chúng chỉ cần sử dụng chuỗi ký tự dễ nhớ (domain name) như: http://www.microsoft.com, http://www.ibm.com…, thay vì sử dụng địa chỉ IP là một dãy số dài khó nhớ. 

###2. Mục đích chính của DNS là: 
  - Phân giải tên máy thành địa chỉ ip và ngược lại
  - Phân giải tên miền domain

###3. Cấu trúc hệ thống tên miền
####a. Cấu trúc về cơ sở dữ liệu

 <img src="http://i.imgur.com/I7dXlMW.png">

  Cơ sở dữ liệu của hệ thống DNS là hệ thống cơ sở dữ liệu phân tán và phân cấp theo hình cây.

  Trên đỉnh là Root server sau đó đến các miền domain được phân nhánh xuống dần và phân quyền quản lý. Khi có 1 yêu cầu truy vấn, yêu cầu sẽ được gửi đến root rồi sau đó phân cấp xuống dần xử lý.

  Quyền cấp phát các tên miền ở mức cao nhất gọi là Top-Level-Domain

  Hệ thống tên miền (DNS) cho phép phân chia tên miền để quản lý, nó chia hệ thống thành các zone, trong zone sẽ quản lý tên miền được phân chia. Các zone này chứa các thông tin về các miền cấp thấp hơn có khả năng chia thành các zone cấp thấp hơn và chia cho các DNS server khác quản lý.

  Ví dụ: zone ".vn" do DNS server quản lý chứa các thông tin có bản ghi là đuôi ".vn" và có thể chuyển xuống các zone cấp thấp hơn để quản lý như ".ftp.vn"

####b. Cấu trúc tên miền
Tên miền được phân thành nhiều cấp như:

- Gốc (Domain root): Nó là đỉnh của nhánh cây của tên mền. Nó xác định kết thúc của domain. Nó thể diễn đơn giản chỉ là dấu chấm “.”

- Tên miền cấp 1 (Top-Level-domain): Là gồm 1 vài ký tự xác định 1 nước, 1 tổ chức hay 1 khu vực
  
- Tên miền cấp 2 (Second-level-domain): Rất đa dạng, có thể là tên cty hay tổ chức hay cá nhân
  Tên miền cấp nhỏ hơn (Subdomain): Chia tên miền cấp 2 thành tên nhỏ hơn như chi, nhánh, phòng, ban hay chủ thể nào đó

###4. Máy chủ quản lý tên miền (dns-Domain name server)
 Máy chủ quản lý tên miền theo từng khi vực, theo từng cấp như 1 tổ chức, 1 cty hay 1 vùng lãnh thổ. Máy chủ đó chứa thông tin dữ liệu về địa chỉ và tên miền trong khu vực, trong cấp mà nó quản lý để chuyển giữa tên miền và địa chỉ IP đồng thời nó cũng có khả năng hỏi các máy chủ quản lý tên miền khác hoặc cấp cao hơn để có thể trả lời đc các truy vấn về tên miền không thuộc quản lý của nó

 Máy chủ cấp cao nhất là root Server quản lý toàn bộ hệ thống tên miền. Nó không chứa các thông tin về cấu trúc hệ thống DNS mà nó chuyển quyền quản lý xuống các server cấp thấp hơn.

###5. Phân loại DNS server
- Primary server: Nguồn xác thực thông tin chính mà nó quản lý. Thông tin tên miền đc lưu trữ hchinhs tại đây
- Secondary server: dùng để lưu trữ dự phòng cho primary server
- Caching Name server: Không chứa thông tin mà sẽ lấy thông tin trong cache trả về cho máy

###6. Đồng bộ dữ liệu giữa các DNS Server (Zone transfer)
Có 2 cách để đồng bộ dữ liệu:

- ĐỒng bộ toàn phần(all zone transfer)
- Đồng bộ phần thay đổi (incremental zone transfer)

###7. Cơ sở dữ liệu DNS Server
Một máy chủ DNS là 1 máy chủ lưu trữ các bản ghi tên miền giúp đáp ứng những truy vấn đối với cơ sở dữ liệu của nó. Các loại bản ghi thường gặp là:

####a. Bản ghi SOA (Start of Authority): 
Trong mỗi tập tin CSDL phải có và chỉ có 1 bản ghi SOA chỉ ra máy chủ nameserver là nơi cung cấp thông tin tin cây từ dữ liệu có trong zone

Cú pháp như sau:

         [domain name] IN SOA [dns-server][email-address]
    (
    [serial number]; 
    [refresh number]; 
    [retry number]; 
    [expire number];
    [time-to-live number];
    )

 domain name: tên domain DNS quản lý.

 dns-server: tên server quản lý miền.

 email-address: địa chỉ email của admin.

 serial number: số lần thay đổi dữ liệu trong zone, khi máy chủ secondary liên lạc với máy chủ primary nó sẽ dùng số này để so sánh nếu lệch nhau thì sẽ có sẽ cập nhật thông tin.

 refresh number: khoảng thời gian máy chủ secondary kiểm tra primary nếu cần.

 retry number: sau thời gian refresh nếu ko thể kết nối với máy chủ thì định kỳ theo thời gian retry nó sẽ thử kết nối lại (thường nhỏ hon giá trị refresh).

 expire number: nếu sau khoảng thời gian này ko liên lạc đc với máy chủ primary thì dữ liệu zone trên máy secondary sẽ bị quá hạn và nó sẽ ko trả lời truy vấn về zone này nữa (thường lớn hơn giá trị retry và refresh)

 TTL (time to live): chỉ ra thời gian mà các máy chủ name server đc cache lại thông tin giúp làm giảm lưu lượng truy cập DNS trong mạng
 
####b. Bản ghi A(Address) và CNAME (Canonical Name)
- Bản ghi A : ánh xạ tên vào địa chỉ

Cú pháp:
 
     [Domain] IN A [địa chỉ ip máy]


- Bản ghi CNAME: tạo alias trỏ vào tên ở bản ghi A. Cho phép 1 máy tính có thể có nhiều tên, nói cách khác thì bản ghi này cho phép nhiều tên cùng trỏ vào 1 địa chỉ Ip cho trước. Để khai báo bản ghi CNAME thì bắt buộc phải có bản ghi A để khai bảo tên máy. Tên miền ở bản ghi A trỏ đến địa chỉ ip của máy được gọi là tên miên chính (canoncial domain). Các tên miền khác muốn trỏ đến máy tính này phải được khai báo là bí danh của tên máy (alias domain).

Cú pháp:
      
     [alias-domain] IN CNAME [canonical domain]

####c. Bản ghi MX (mail exchange)
 DNS dùng bản ghi MX trong việc chuyển mail trong mạng Internet. Khi nhận đc maul trình chuyển mail dựa vào bản ghi MX để quyết định đường đi của mail. 

Cú pháp:
 
    [domain name] IN MX [độ ưu tiên] [tên mail server]
     
 domain name :là tên miền được khai báo để sử dụng như địa chỉ thư điện tử.

 tên mail server : là tên trạm chuyển tiếp thư điện tử (tên máy dùng làm máy trạm chuyển tiếp thư điện tử).

 độ ưu tiên : là số từ 1-255 nếu giá trị ưu tiên này càng nhỏ thì trạm chuyển tiếp thư điện tử được khai báo sau đó sẽ là trạm chuyển tiếp thư điện tử đc chuyên đến đầu tiên.

####d. Bản ghi NS:
  Bản ghi dùng để khai báo máy chủ tên miền cho 1 tên miền.

 Cú pháp: 

     [domain name] IN NS [DNS Server]

  domain name : là tên miền

  DNS Server : tên máy chủ tên miền

####e. Bản ghi PTR
 Là bản ghi dùng để ánh xạ địa chỉ IP thành tên máy.

Cú pháp:

    [IP] IN PTR [host-name]

###8. Cơ chế phân giải tên:
####a. Phân giải tên thành ip

 Khi có 1 truy vấn đến thì server dns cục bộ sẽ nhận đầu tiên xem tên miền đó có do nó quản lý ko, nếu có thì nó sẽ trả về địa chỉ ip còn nếu không thì nó sẽ truy vấn đến root server gần nhất mà nó biết. Root Server sẽ trả về ip của server quản lý tên miền dưới. Lần lượt tên miền sẽ được phân giải như vậy cho đới khi tên miền được trả về ip cụ thể

####b. Phân giải ip thành tên máy.
 Ánh xạ địa chỉ ip thành teenmays được dùng để diễn dịch các tập tin log cho dễ đọc hơn. Nó còn được dùng trong 1 số trường hợp chứng thực trên hệ thống UNIX. Để phân giải được tên máy của 1 địa chỉ IP, trong không gian tên miền ta bổ sung thêm 1 tên nhánh tên miền mà đc lập chỉ mục theo địa chỉ IP. Phần không gian này có tên miền là in-addr.arpa

##II. Tiến hành lab DNS trên Ubuntu-server
###Bước 1: Thực hiện update cho ubuntu bằng câu lệnh:

    apt-get update

###Bước 2: Cài đặt gói bind9 (dùng để cấu hình dịch vụ DNS trên Linux):

    apt-get install bind9

###Bước 3: Tiến hành cấu hình dịch vụ:
- Các bạn vào đường dẫn /etc/bind/name.conf.local
- Chúng ta tiến hành thêm vào đây 2 zone 

1 zone để phân giải tên miền thành ip

    zone "canvietanh.com" {
    type master;
    file "/etc/bind/db.vdc.com";
    };

1 zone dùng để phân giải ip về tên miền

     zone "11.168.192.in-addr.arpa" {
     type master;
     file "/etc/bind/db.192";
     };


Ví dụ:

- Mình đặt với tên miền mới muốn tạo là "canvietanh.com"
- Type có 3 loại:

        Master: server có bản copy chính CSDL
        Slave: server lưu một bản sao CSDL từ master
        Stub: tương tự như slave nhưng chỉ sao chép record NS từ Master chứ không phải toàn bộ dữ liệu

- File: Chỉ ra đường dẫn về cơ sở dữ liệu, nơi chứa các bản ghi

###Bước 4: Cấu hình đến cơ sở dữ liệu
####Với zone phân giải tên miền thành ip:

Tạo 1 file db.vdc.com trong đường dẫn /etc/bind/ với nội dung như sau:


    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     canvietanh.com.      canvietanh.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
    ;  
    @       IN      NS      canvietanh.com.
    @       IN      A       192.168.11.200



- Các thông số theo bản ghi SOA đã được đề cập ở phần lý thuyết.

- Bản ghi A để phân giải tên miền thành ip


####Với zone phân ip thành tên miền:
Tạo 1 file db.192 trong đường dẫn /etc/bind/ với nội dung như sau:


    ;
    ; BIND reverse data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     canvietanh.com.   root.canvietanh.com. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      canvietanh.com.
    200     IN      PTR     canvietanh.com.



###Bước 5: Kiểm tra
- Trên 1 máy ubuntu khác ta sửa trong file /etc/resolv.conf 
- Sửa name server về địa chỉ ip của máy ubuntu ta vừa cấu hình DNS
- Sau đó dùng lệnh nslookup để kiểm tra kết quả vừa cáu hình:

<img src="http://i.imgur.com/ldB4iL1.png">
