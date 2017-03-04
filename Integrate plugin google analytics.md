**Hướng dẫn cái đặt Plugin Google Analytics**

**sử dụng SDKBOX**



**1. Cài đặt SDKBox**

Cài đặt Python
Chạy câu lệnh sau bằng command line:

**python -c &quot;import urllib; s = urllib.urlopen(&#39;https://raw.githubusercontent.com/sdkbox-doc/en/master/install/install.py****&#39;).read(); exec s&quot;**

 ![](https://lh3.googleusercontent.com/tyizLCh8rJeFsER0R5TPPwzOS8nXz951mlL9TavaZAd8mXhwsRJG2IajhZZTXhyfDbZo9VTquVG6YQomqOU5nytqBmhu33mwXg=w2400-h1350-rw-no)

**2. Tạo track ID để theo dõi app thông qua googleanalytics**

Đi tới analytics.google.com và đăng nhập
Tạo Track ID để google trả về kết quả theo dõi app

![](https://lh3.googleusercontent.com/QLGALZoxy4EFqf8OjNunt95emGT1vnhUGAk52sXO-qdPUPMCol4BoMMWxJiM_8QzORi0qK2Qqq6nIEyPnbZIVycb_puWtgWeSA=w2400-h1350-rw-no)

**3. Tải plugin googleanalytics**

Chuyển đến thư mục của project và chạy câu lệnh bằng command line:

sdkbox import googleanalytics

 ![](https://lh3.googleusercontent.com/BADaOGiRLKu0ULEcOyD7Jl9OcsXQJU3gnQ7QsjnHWlmVek5a3dtE40PXdZzAXwu35XrWOfyiHXYWqAEm35CMAqlWNgjigHIisw=w2400-h1350-rw-no)

Sau khi tải xong plugin, chuyển tới thư mục ./Resource và mở file &quot;sdkbox\_config.json&quot;
Sửa đổi giá trị của Track ID. Ví dụ:

    &quot;GoogleAnalytics&quot;: {
            &quot;trackingCode&quot;: &quot;UA-91468317-1&quot;
        } 
 ![](https://lh3.googleusercontent.com/X33qe-qpfr7xZynrnpkOCI7zgoFj1IxKLw-PJ0haJha7kA7WVuTngEqN17LCv6jIbJBFUuH0aPy__w0Fp_g8d7NWumw_FvZZfg=w2400-h1350-rw-no)

**4. Thêm code trong project**

Trước tiên, include vào source:

    #include &quot;PluginGoogleAnalytics/PluginGoogleAnalytics.h&quot;
![](https://lh3.googleusercontent.com/zp0Y2nKKV-4WELCYOzrC1mPTiWUftz8snSl5jnQrIlANsdCms4tmM6s2p_QxI_lz8PyxgFp-B_74CIlmqY7F_RdEh82dC9zAFA=w2400-h1350-rw-no)
Sau đó là khởi tạo plugin, nên khởi tạo trong hàm AppDelegate::applicationDidFinishLaunching() như sau:

    AppDelegate::applicationDidFinishLaunching()
    {
    #ifdef SDKBOX\_ENABLED
      sdkbox::PluginGoogleAnalytics::init();
      sdkbox::PluginGoogleAnalytics::startSession();
      …
    #endif
    }

 ![](https://lh3.googleusercontent.com/rrb0Ld7uL0mxWqz_h06BfvPbrLdGsjT3QNGh9OAWFMOJwGk3CnLuZyCBoQaPsZMKwRwSqtk_sUHH9iajr4o1doYBq1a5_5Jr2A=w2400-h1350-rw-no)

** sdkbox chỉ sử dụng đc khi build nền tảng android nên cần thêm#ifdef SDKBOX\_ENABLED để chạy được nên windows**



-  Các hàm để tùy chỉnh các event trong googleanalytics plugin:

( Nhớ thêm #ifdef SDKBOX\_ENABLED và #endif )

    static void setUser ( const std::string &amp; userID )
&lt; Đặt userID cho phiên làm việc từ thiết bị &gt;

     static void logEvent (const std::string &amp; eventCategory ,
                       const std::string &amp; eventAction ,
                       const std::string &amp; eventLabel ,
                       int value);
&lt; Ghi lại sự kiện, Ví dụ:

    sdkbox::PluginGoogleAnalytics::logEvent(&quot;Achievement&quot;, &quot;Unlocked&quot;, &quot;Slayed 100 dragons&quot; , 1);&gt;

 ![](https://lh3.googleusercontent.com/VQM0IIhf6sqEyFfS7oTnLxjXWFVLoslwdBQVhlIAA92EKYNZFvA3iPEOia5DH1uFQjLxMzqaEQ7jJRIOSKAJUrouEx75LlRsag=w2400-h1350-rw-no)
 
        static void logException (const std::string &amp; exceptionDescription ,
                           bool isFatal);
&lt; Ghi lại lỗi nếu xảy ra một Exception &gt;


**5. Build Project**

Build Project và chạy, xem kết quả trên analytics.google.com (thường thì sẽ phải đợi 1 ngày google mới update).
![](https://lh3.googleusercontent.com/aKPwj_Bu2RA_cpFk6a8owTZqjwBFLyAJJvZyIN-39EROHMNgTULkonukWLYwmgFlVVDBJnmHue_sC4RGewAynT7zI_UnMhmpGw=w2400-h1350-rw-no)

_[Chi tiết bài viết](http://docs.sdkbox.com/en/plugins/googleanalytics/v3-cpp/)_
