---
layout: post
title:  "Biên dịch chương trình trên c"
date:   2018-02-01 23:19:43 +0530
categories: programming
author: "vnc0d9r"
---

Nếu là một lập trình viên, ai cũng nghe qua về trình biên dịch cũng như cách biên dịch một chương trình nhưng chắc cũng ít bạn iết cụ thể thì biên dịch là quá trình diễn ra như thế nào vì đơn giản chúng ta chỉ cần ấn nút chạy hoặc gõ một dòng lệnh đơn giản. Hôm nay mình sẽ có một bài viết cụ thể xem biên dịch một đoạn mã ngôn ngữ c thành chương trình chạy trải qua những bước gì.

Để bắt đầu, ta cùng xem một đoạn mã đơn giản và quen thuộc như sau:
{% highlight c %}
#include <stdio.h>

int main()
{
    printf("hello, world\n");
}
{% endhighlight %}
chương trình trên in ra một đoãn text "hello, world" ra ngoài mà hình. Đoạn code này được tôi lưu trên một file tên `hello.c`

## 1.1 Information is Bits + Context ##

Chương trình `hello` của chúng ta bắt đầu vòng đời của nó như là một `source program hay source file` (ở đây chính là file `hello.c`). Source program này là một chuỗi các `bit` 0 và 1 được tổ chức thành các nhóm 8 bit một được gọi là `byte`, mỗi byte biểu diễn một kí tự trong chương trình. Các hệ thống bây giờ biểu diễ n các kí tự thông qua chuẩn `ASCII`. Chương trình trên có thể được biểu diễn dười dạng mã ASCII như sau:

![Hình 1](http://sv1.upsieutoc.com/2018/02/02/Screenshot-from-2018-02-02-22-29-56.png)

Như chúng ta thầy mỗi byte có một giá trị nguyên, tương ứng với một ký tự được lưu trữ trên file. Loại file này được gọi là `text file` vì nó chỉ chứa các kí tự đơn thuần, một số loại file được viết đến với tên gọi `binary files`. Việc biểu diễn file `hello.c` minh họa một ý tưởng cơ bản là mọi thông tin có trong hệ thống bao gồm file trên đĩa, chương trình được lưu trữ trên bộ nhớ, dữ liệu người dùng lưu trữ trên bộ nhớ, dữ liệu được luân chuyển thông qua mạng đều được biểu diễn thông qua `bunch of bits` mà như trường hợp trên là một `byte`, chỉ duy nhất sự khác biệt là tùy vào ngữ cảnh của dữ liệu được hiển thị ra cho chúng ta, ví dụ như cùng một chuối byte nhưng trong trường hợp này là số nguyên, trong khi trường hợp khác là số thập phân, kí tự hay chỉ lệnh. Chúng ta phải hiểu cách mà máy biểu diễn dữ liệu vì nó không giống với cách của con người.

## 1.2 Sự chuyển đổi của chương trình ##

Chương trình `hello` được bắt đầu như là một chương trình c bậc cao (high-level c program) bởi vì nó có thể  được đọc và hiểu bởi con người (đây là dạng đầu tiên). Tuy nhiên để có thể chạy chương trình này trên hệ thống nó cần được chuyển đổi sang một dạng bậc thấp gọi là `machine-language` hay chính là mã máy bao gồm một tập các lệnh. Gọi là bậc thấp vì nó khó có thể đọc và hiểu bởi con người nhưng lại rất dễ dàng đối với máy. Tập lệnh này sau đó được đóng gói duới dạng `executable object program` và lưu trữ dưới dạng `binary file ` trên đĩa cứng. Việc chuyển đổi này thông qua một chương trình gọi là trình biên dịch, trên các hệ thống linux ta có thể biên dịch một chương trình c thông qua lệnh sau:

{% highlight shell %}
linux> gcc -o hello hello.c
{% endhighlight %}
![Hình 2](http://sv1.upsieutoc.com/2018/02/02/imagecbfcf4ee122224b9.png)

