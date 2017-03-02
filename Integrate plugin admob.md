**Cài đặt plugin Admob sử dụng SDKBOX**

**1. Cài đặt SDKBOX: (Xem ở bài cài đặt GoogleAnalytics)**
**2. Chạy câu lệnh bằng command line:**

 ** sdkbox import admob**
  ![](https://lh3.googleusercontent.com/Ke2u4Ii1U639Vw9Lx0NQu69mr7kLnKA8dI7AfJcaj2YIqKzJaIbbEhEkd5X-L2hcTbqT1CrJtWYIWYgjlNsd5-iNinVE0cOI7g=w2400-h1350-rw-no) 

**3.  Sửa file sdkbox config**

Mở file ./Resource/sdkbox\_config.json và sửa các thông số:

- Thay đổi id của quảng cáo ở key gameover và key home (key gameover chạy quảng cáo full screen, key home chạy quảng cáo banner
- Ở key home, sửa lại giá trị của key width và height cho phù hợp (khuyên dùng 0 và 0 có nghĩa là smart banner, fit screen của user).
- Key alignment: vị trí của banner (top, bottom, left, right, center, …)

 ![](https://lh3.googleusercontent.com/VAULoZEgqHawqN4COqmHAc0hKES-ti4BOxrt93ph9DTNQ_y6dYmvBxwQZJzWcWjGMTsvvKRh2qY4DqM91z7oT-06XlJ10Eajdw=w2400-h1350-rw-no) 

**4.  Sử dụng**

- Trước tiên là include vào project:

*#include &quot;PluginAdMob/PluginAdMob.h&quot;

- Khởi tạo plugin admob

*#ifdef SDKBOX\_ENABLED

        sdkbox::PluginAdMob::init();

sdkbox::PluginAdMob::cache(&quot;home&quot;);

        sdkbox::PluginAdMob::cache(&quot;gameover&quot;);

*#endif

\*\* Hàm cache dùng để lưu sẵn quảng cáo vào bộ nhớ cache.

- Các hàm để sử dụng quảng cáo:

sdkbox::PluginAdMob::show(&quot;home&quot;);

sdkbox::PluginAdmob::show(&quot;gameover&quot;); ![](https://lh3.googleusercontent.com/q4VyXyBBruinIEYSua9bcDCPOVQSgrmvTaGsKMWduicj468_9ZpZ5Pxj5q4rMGANN9l0r00dNMhOjZsGWIwea1p96BoQZVGXwQ=w2400-h1350-rw-no)

        &lt;Hiển thị quảng cáo banner và fullscr&gt;

 sdkbox::PluginAdmob::hide(&quot;home&quot;);

        &lt;Tắt quảng cáo banner&gt;



_Chi tiết: http://docs.sdkbox.com/en/plugins/admob/v3-cpp/_
