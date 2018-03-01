---
layout: post
title:  "Cơ chế hoạt động của https"
date:   2018-03-01 16:22:43 +0530
categories: networking
author: "vnc0d9r"
---

Nếu bạn là người để ý thì khi truy cập internet trên trình duyệt, trên phần địa chỉ web bao giờ cũng bắt đầu là `http` hoặc `https`. HTTPS là giao thức truy cập web bảo mật do được tích hợp giao thức SSL hoặc TSL mã hóa gói tin khi truyền đi thay vì dạng plant-text như của http. Dạng plant-text nghĩa là khi bạn đăng nhập vào một trang web như facebook chẳng hạn thì username và password của bạn được truyền đi theo dạng mà ai cũng đọc được, do đó nếu hacker có thể  lấy được dữ liệu bạn gửi đi từ trình duyệt thì tất nhiên sẽ biết được tài khoản của bạn. Việc này cũng như bạn gửi thư vậy, http là bạn gửi thư đi luôn còn https là bạn cho thư vào hộp, khóa lại rồi mới gửi đi.

![Hình 1 - tài khoản (v4L-justfortryv4L) đã bị lộ)](https://www.hacking-tutorial.com/pics/blog/network-sniffing-use-wireshark/wireshark7.jpg)

Ngược lại, https mã hóa tài khoản của bạn sau đó mới gửi đi, do đó kể  cả hacker có lấy được dữ liệu mà không có chìa khóa thì cũng không thể làm gì được. Nhưng bầy giờ có một vấn đề phát sinh là làm sao mình gửi cái chìa khóa đó đi được, vì dữ liệu đã mã hóa thì muốn đọc được thì phải giải mã. Gửi đi làm sao để hacker không biết được hoặc biết cũng không làm gì được, à lúc đó chúng ta phải mã hóa cái key đó!. Vậy phải mã hóa làm sao cho hợp lí? Muốn mã hóa chúng ta lại phải tạo key, bây giờ muốn bảo vệ cái key này chẳng nhẽ chúng ta lại tạo thêm một cái key nữa sao, cư như thế thì tạo đến bao giờ?, nên nguời ta nghĩ ra một cách mã hóa gọi là mã hóa bất đối xứng. Ở loại mã hóa này, ta sẽ có 2 loại key là private key (chỉ mình người nhận biết) và publish key (công khai, ai cũng biết). Ta dùng publish key để mã hóa còn người nhận dùng private key để giải mã. Như vậy ai cũng có thể mã hóa thông điệp để gửi cho nguời nhận nhưng chỉ mình anh ta mới có thể đọc được nội dung của các thông điệp.
Như vậy, gần như mọi vấn đề đã được giải quyết để việc truyền tin được an toàn, tuy nhiên ta vẫn thấy rằng vẫn còn kẽ hở ở đây. Đó là làm sao biết được là cái publish key kia là của đúng nguời nhận, nhỡ nó là "hàng nhái" thì sao?. Cái này cũng giống như khi bạn mua hàng ngoài chợ, nếu bạn không phải "người mua hàng thông minh" thì bạn dễ mua phải hàng giả vì luôn có cách làm nó rất giống hàng thật. Để giải quyết vấn đề này, người ta nghĩ ra một cái gọi là "chữ kí số" có vai trò như là con dấu hoặc tem chính hãng ngoài đời thật. Chữ kí số bao gồm một số thuật toán sau:

1. Thuật toán chọn khóa
2. Thuật toán tạo chữ ký 
3. Thuật toán kiểm tra chứ ký 

Ta có một ví dụ sau (lấy từ wikipedia.org):
Bob muốn gửi thông tin cho Alice và muốn Alice biết thông tin đó thực sự do chính Bob gửi. Bob gửi cho Alice bản tin kèm với chữ ký số. Chữ ký này được tạo ra với khóa bí mật (private key) của Bob. Khi nhận được bản tin, Alice kiểm tra sự thống nhất giữa bản tin và chữ ký bằng thuật toán kiểm tra sử dụng khóa công cộng của Bob. Bản chất của thuật toán tạo chữ ký đảm bảo nếu chỉ cho trước bản tin, rất khó (gần như không thể) tạo ra được chữ ký của Bob nếu không biết khóa bí mật của Bob. Nếu phép thử cho kết quả đúng thì Alice có thể tin tưởng rằng bản tin thực sự do Bob gửi.

## Chuyện gì sẽ diễn ra khi bạn truy cập vào một site https ##

Câu chuyện này có thể được mô tả theo hình dưới dây (theo nguồn stackoverflow.com):

![Hình 2 - From browser to HTTPS site](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/WRNrD.png_q1sk0vahes)

Các quá trình diễn ra:

1. Browser truy cập một trang web https mà ở đây là https://payment.com 
2. Server của payment.com sẽ trả về certificate (cái này có vai trò như chứng minh thư của chúng ta, xác định một trang web có đúng là nó hay giả mạo) của trang này. Trong certificate chứa publish key của trang dùng để mã hóa K ở bước 4. K đây chính là khóa để mã hóa dữ liệu trao đổi giữa browser và server của payment.com
3. Browser sẽ kiểm chứng certificate (bằng cách chạy thuật toán kiểm tra chữ ký). Quá trình này giúp browser xác định https://payment.com là thật hay giả.
4. Sau khi kiểm chứng được certificate, browser sẽ tự sinh ra 1 khóa K. Khóa K sẽ được dùng để mã hóa tất cả các dữ liệu giữa browser và payment.com sau này. Sau đó khóa K sẽ được mã hóa để gửi cho payment.com để bên này giải mã dữ liệu bằng mã hóa bất đối xứng.
5. Sau khi nhận được khóa K, payment,com giải mã bằng private key của mình.
6. Payment.com và browser dùng khóa K để mã hóa toàn bộ dữ liệu liên lạc sau này

Certificate là một khối dữ liệu bao gồm rất nhiều thông tin về payment.com. Các thông tin này bao gồm:

* Tên domain
* Tên công ty sở hữu
* Thời gian certificate được cấp
* Thời hạn certificate
* Public key

## Làm thế nào browser kiểm chứng được certificate ở bước 3 ##

Bất cứ người nào đứng giữa browser và payment.com đều có thể làm giả certificate và public key. Bằng cách làm giả certificate, người đứng giữa có thể giả dạng payment.com. Bằng cách giả public key, người đứng giữa có thể dùng khóa private của mình để xem thông tin truyển tải giữa 2 bên. Vậy làm thế nào để ngăn chặn cách tấn công này. Cách giải quyết là: CA (Certificate Authority).
CA là gì? Làm gì?

Trong thực tế, để chứng minh một ai đó trình độ đại học, trường đại học nơi người đó học sẽ cấp cho họ một tấm bằng. Do tấm bằng đó có thể bị làm giả, ta cần một đơn vị chứng minh đó là bằng thật. CA chính là người chứng minh certificate mà payment.com cung cấp là thật! CA bán dịch vụ chứng thực đó bằng cách ký chứng minh rằng certificate của payment.com là thật

Certificate chứng thực cho payment.com sẽ được CA ký bằng khóa bí mật của CA. Khóa này chỉ có CA biết và do vậy việc chữ ký là an toàn. payment.com sẽ gửi cho user certificate đã được ký bởi CA cùng với khóa công khai PK2 của khóa bí mật CA dùng để ký certificate. Browser sẽ tiến hành kiểm tra certificate này như bình thường dùng khóa công khai PK2 của CA.

Đến đây vấn đề chưa thực sự được giải quyết vì, khóa công khai của CA cũng hoàn toàn có thể bị làm giả và do vậy certificate hoàn toàn có thể là giả! Giống như thực tế người chứng minh cho tấm bằng đại học cũng có thể bị làm giả. Để chắc chắn việc này, ta có thể ký chứng thực certificate do CA ký là thật. Cách làm có thể hoàn toàn tương tự là dùng khóa bí mật nào đó và ký tiếp và đính kèm khóa công khai PK3 với certificate sau khi được ký. Cứ thế ta có một dãy các certificate và khóa công khai mà certificate sau chứng thực cho certificate trước. Do bản chất đệ quy, ta cần điểm dừng là một certificate mà ta hoàn toàn tin tưởng. Đến đây ta có khái niệm root certificate.

### Root certificate ###
 
Root certificate là certificate mà ta hoàn toàn tin tưởng. Khi có certificate này, ta có thể tin tưởng những certificate mà được chứng thực bởi certificate này là hoàn toàn hợp lệ (giống trong thực tế là cơ quan công chứng!). Mỗi OS và browser có một danh sách các certificate mà OS và browser đó tin tưởng.

Firefox tin tưởng các certificate có danh sách tại: https://wiki.mozilla.org/CA:IncludedCAs

Do root certificate là certificate cuối cùng dùng để chứng thực các certificate trong chuỗi, khóa bí mật của certificate này cần được bảo vệ nghiêm ngặt. Bất cứ công ty cung cấp dịch vụ CA nào bị tấn công và bị mất khóa bí mật của root certificate đều rất nguy hiểm, bởi vì hackers có thể dùng khóa đó để ký certificate ở bước cuối cùng. Do browser và OS tin tưởng certificate này nên tất cả certificate, không nhất thiết do CA bị hack cung cấp, đều có thể bị làm giả. Các CA do vậy dùng rất nhiều công sức để bảo vệ thật kín đáo khóa bí mật này. Kinh doanh chứng thực CA là kinh doanh về mặt lòng tin, CA hứa sẽ đảm bảo tốt nhất khóa bí mật của họ và ta trả tiền để họ dùng khóa bí mật của họ ký.

Theo đồn đại của giang hồ, [verisign dùng cửa lock dày và các tay súng chuyên nghiệp cùng hệ thống truy cập phức tạp để bảo vệ khóa bí mật này](https://security.stackexchange.com/questions/24896/how-do-certification-authorities-store-their-private-root-keys). 

### Certificate trong thực tế ###

Ta có thể xem certificate của bất cứ trang nào sử dụng HTTPS bằng trình duyệt như sau:

![Hình 3 - Lấy thông tin certificate](https://uphinhnhanh.com/images/2018/03/01/sslFacebook.gif)

Facebook certificate được ký bởi DigiCert High Assurance CA-3. Bản thân CA-3 được chứng thực bởi EV RootCA.

## References ##

1. [Kipalog](https://kipalog.com/posts/https-hoat-dong-nhu-the-nao)
2. [Wiki][https://vi.wikipedia.org/wiki/Ch%E1%BB%AF_k%C3%BD_s%E1%BB%91]
3. [Stackoverflow][https://stackoverflow.com/questions/470523/how-does-ssl-really-work]
