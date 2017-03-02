**Hướng dẫn cài đặt plugin Vungle sử dụng SDKBOX**



Vì sdkbox không còn hỗ trợ plugin này nữa nên cần phải cài đặt thủ công:

**1. Download tại đây:**

  [http://download.sdkbox.com/installer/v1/sdkbox-vungle\_v2.3.2.0.tar.gz](http://download.sdkbox.com/installer/v1/sdkbox-vungle_v2.3.2.0.tar.gz)

**2 Giải nén file vừa tải về.**

**3. Copy các file jar** từ plugin/android/libs sang thư mục cocos2d/cocos/platform/android/java/libs của project

**4 Copy toàn bộ thư mục** trong plugin/android/jni đến &lt;project\_root&gt;/jni/

**5 Sửa file AndroidManifest.xml**

 Chèn vào file các dòng sau:

&lt;uses-permission android:name=&quot;android.permission.INTERNET&quot; /&gt;
&lt;uses-permission android:name=&quot;android.permission.WRITE\_EXTERNAL\_STORAGE&quot; /&gt;
&lt;uses-permission android:name=&quot;android.permission.ACCESS\_NETWORK\_STATE&quot; /&gt;

Trước thẻ đóng của thẻ application chèn dòng sau:

&lt;activity
  android:name=&quot;com.vungle.publisher.FullScreenAdActivity&quot;
  android:configChanges=&quot;keyboardHidden|orientation&quot;
  android:theme=&quot;@android:style/Theme.NoTitleBar.Fullscreen&quot;/&gt; 
  ![](https://lh3.googleusercontent.com/7hW1KiKuF7qFABFwLW7KtzmZ8dtKJ1J7zIZ1jW9HB3f9Au3FhBaA9y9mC8Q_JuOzfsAyN2hK-yX7qkODdHNeccxsQiYYcYr6=w2400-h1350-rw-no)

**6. Sửa file**android/jni/Android.mk

        Thêm vào **LOCAL\_WHOLE\_STATIC\_LIBRARIES** như sau: PluginVungle \ 
![](https://lh3.googleusercontent.com/Zo2j9TNyAFt6qSKQc2r3Qj7p_J-dqLUHV3tqT0NwEWoVmQEh9J0ah1wAxdhd8r1kZCkDLIMmRJ_8mqiKGOjRz1YcyexVvMMn=w2400-h1350-rw-no)

Và thêm vào call import-module:

$( **call**** import **-** module**, ./sdkbox)
$( **call**** import **-** module**, ./pluginvungle) 
![](https://lh3.googleusercontent.com/dyGHDTJgvWl-k_rm_LdkqH3MgkTj1iwztI2C3M2ZFnPAxKB_nZqquaC7GI2b9jOeaEWiSbcPI0FA_yL-pEqp1lTDRUdp7Zrf=w2400-h1350-rw-no)



**7. Sửa file**../../cocos2d-x/cocos/platform/android/java/src/org/cocos2dx/ lib/Cocos2dxActivity.java

        thêm các import:

                import android.content.Intent;
                import com.sdkbox.plugin.SDKBox;

Sau lời gọi hàm        onLoadNativeLibraries(); thêm hàm sau: SDKBox.init( **this** ); 
![](https://lh3.googleusercontent.com/1m8v84nFYyOzQu4b4438_LaRWxTbbls8ZluPwkJkpacDNZ9Ykah2nkZgq4HJbGclxDvXEiVDpAyY9R4_50t64PGmfKyEVYZn=w2400-h1350-rw-no)

Cuối cùng là sửa các hàm, nếu như đã từng cài plugin bằng command line (sdkbox import admob hay gì đó) thì **không** cần làm bước này:

Nếu như các hàm chưa đc định nghĩa thì thêm vào, còn nếu đã đc định nghĩa thì thêm các lời gọi hàm của sdkbox như dưới:

@Override
     **protected**** void ****onActivityResult** ( **int** requestCode, **int** resultCode, Intent data) {
           **if** (!SDKBox.onActivityResult(requestCode, resultCode, data)) {
             **super**.onActivityResult(requestCode, resultCode, data);
          }
    }
    @Override
     **protected**** void ****onStart** () {
           **super**.onStart();
          SDKBox.onStart();
    }
    @Override
     **protected**** void ****onStop** () {
           **super**.onStop();
          SDKBox.onStop();
    }
    @Override
     **protected**** void ****onResume** () {
           **super**.onResume();
          SDKBox.onResume();
    }
    @Override
     **protected**** void ****onPause** () {
           **super**.onPause();
          SDKBox.onPause();
    }
    @Override
     **public**** void ****onBackPressed** () {
           **if** (!SDKBox.onBackPressed()) {
             **super**.onBackPressed();
          }
    }



**8. Sửa** Resource/sdkbox\_config.json

 Thêm vào key android: lưu ý thay đổi vungleid

&quot;Vungle&quot; :
{
    &quot;id&quot;:&quot;&lt;vungle id&gt;&quot;,
    &quot;ads&quot;:{
        &quot;video&quot;:{
            &quot;sound&quot; : true,
            &quot;backbutton&quot; : true
        },
        &quot;reward&quot;:{
            &quot;sound&quot; : false,
            &quot;backbutton&quot; : false,
            &quot;incentivized&quot; : true
        }
    }
}

**9. Sửa code:**

 *#include &quot;PluginVungle/PluginVungle.h&quot;

...

 bool AppDelegate::applicationDidFinishLaunching() {

*#ifdef SDKBOX\_ENABLED

 sdkbox::PluginVungle::init();

*#endif

- Các API và cách sử dụng:

Trước tiên, cho lớp sử dụng thừa kế sdkbox::VungleListener và định nghĩa lại các hàm listener: 
![](https://lh3.googleusercontent.com/vbwYvDGqOSLzUGHQvzl1fKe4oY5MDHFSm4Gna0nsMdVOMO606f7OGqfHWRdIWDQGmsmD3ySl4DAcJcaMNL5Tf0ku9buLj1Pp=w2400-h1350-rw-no)

 Hai hàm sử dụng để hiện quảng cáo là

sdkbox::PluginVungle::show(&quot;video&quot;);

và

sdkbox::PluginVungle::show(&quot;reward&quot;);

Sự nhau nhau của 2 giá trị là khi gọi hàm show video, user sẽ không được reward và có thể tắt quảng cáo bất chứ lúc nào. Còn hàm show reward cùng với listener onVungleAdReward() bắt sự kiện khi user đã xem hết quảng cáo để trao quà.

 ![](https://lh3.googleusercontent.com/Za6Awf7oDGyB0GFD8i1AkELlF1ct_RuS8_RcfvY-d5YOilCKsXQzX1nZQCBj9tpCt3Z8Yjf9LKbNSCkpSjpCH7gDL1UcqQwu=w2400-h1350-rw-no)

_Chi tiết bài viết: http://docs.sdkbox.com/en/plugins/vungle/v3-cpp/#manual-integration-for-android_
