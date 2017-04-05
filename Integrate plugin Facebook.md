#**Hướng dẫn cài đặt plugin Facebook sử dụng SDKBOX**

**1. Cài đặt SDKBOX (Xem ở bài cài plugin GoogleAnalytics)**
**2. Chạy câu lệnh bằng command line:**

  **sdkbox import facebook**
  ![](
https://lh3.googleusercontent.com/TFiABDutk4ewHXBUziWkEnh4vL96N_SrHFUuepch8tnseO1K1Pym5sQsr2wfxt0z3MXVGJfze1Q_TaUYwhMirbSRQzGeAQnC=w2400-h1350-rw-no)
**3. Setup Platform**
**Đối với Android**
- Sử dụng java &gt;= 1.7
- Truy cập [https://developers.facebook.com/quickstarts/?platform=android](https://developers.facebook.com/quickstarts/?platform=android)

 =&gt; skip and create App id =&gt; điền thông tin và lấy ID ứng dụng

- Tới đường dẫn ./proj.android/res/values/ và sửa file strings.xml:

 thay thế &quot;facebook\_app\_id&quot; thành ID ứng dụng vừa khởi tạo

 ![](https://lh3.googleusercontent.com/VwLg-RsUhFgCMbNMXD2LinELVIIcQ71gE5JHyjv2PN_SXsk1xxjahSZBTh7GUm7UEYtf-AhmxwDrOD4Cvj3I3youM8ABqxfU=w2400-h1350-rw-no)

- Tới ./proj.android/ và sửa file AndroidManifest.xml: thay thế \_replace\_with\_your\_app\_id\_ thành ID vừa tạo
![](https://lh3.googleusercontent.com/5Dm87a99Lswc-mx30IRdLKYik8UWIWBQe2lG5XhPyP_CYc0l6Li7o_88CR_osMACvci4RnoG2QoHPreXHRn92-QmQ01d10Ft=w2400-h1350-rw-no)



- Tới ./proj.android/ và sửa file project.properties thành target=android-15(hoặc cao hơn) 
![](https://lh3.googleusercontent.com/iBmNoVfAV78b38JO8Khi9GBlhwNkSwo2Ya4rBm-FlzrkprAKm6wWR0WhHq1n_F-lyyPm_8HPLJMd2j06lnWcwgTgOq3LB02a=w2400-h1350-rw-no)

**Đối với iOS**
- Sửa file &lt;project_root&gt;/proj.ios_mac/ios/info.plist: Thêm các dòng sau vào giữa <dict> ... </dict>

*lưu ý thay đổi AppID Facebook của ứng dụng*

    <key>CFBundleURLTypes</key>
    <array>
      <dict>
      <key>CFBundleURLSchemes</key>
      <array>
        <string>fb131512794031280</string>
      </array>
      </dict>
    </array>
    <key>FacebookAppID</key>
    <string>131512794031280</string>
    <key>FacebookDisplayName</key>
    <string>DragonHunting</string>
và

    <key>LSApplicationQueriesSchemes</key>
    <array>
      <string>fbapi</string>
      <string>fb-messenger-api</string>
      <string>fbauth2</string>
      <string>fbshareextension</string>
    </array>

  Sửa file &lt;project_root&gt;/proj.ios_mac/ios/AppController.mm


    Trong hàm
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      // ...

      //
      // **************************
      // !! IMPORTANT !!
      // **************************
      //
      // gọi hàm [[FBSDKApplicationDelegate sharedInstance] application:didFinishLaunchingWithOptions
      // trước app->run()

      BOOL ret = [[FBSDKApplicationDelegate sharedInstance] application:application
                                          didFinishLaunchingWithOptions:launchOptions];
      app->run();
      return ret;
    }

    - (BOOL)application:(UIApplication *)application
                openURL:(NSURL *)url
      sourceApplication:(NSString *)sourceApplication
             annotation:(id)annotation {
      return [[FBSDKApplicationDelegate sharedInstance] application:application
                                                             openURL:url
                                                   sourceApplication:sourceApplication
                                                          annotation:annotation];
    }

    - (void)applicationDidBecomeActive:(UIApplication *)application {
      [FBSDKAppEvents activateApp];
    }
    
 *Lưu ý: Thêm các framework trong &lt;project_root&gt;/proj.mac_ios vào project Xcode*   

**4. Sửa file json của sdkbox**

- Tới ./Resource, mở file sdkbox\_config.json và thay đổi giá trị của key Facebook:
        &quot;Facebook&quot;: {
            &quot;debug&quot;: true,
           &quot;app\_id&quot;: &quot;&lt;Facebook\_app\_id&gt;&quot;
        }

 ![](https://lh3.googleusercontent.com/NFBMEwWt0UbNkOD9VfpFLx3YIzUmFPHbd6mS_SIr5oYhPxP4iJdH2kcmS4daMz63QN8cXAlxGwbzIFQ7NqDoTBmoETEyvKpw=w2400-h1350-rw-no)

Lưu ý: Cấu hình Facebook app đúng để được cung cấp các dịch vụ của SDKBOX cần làm thêm các thao tác:

- Đi tới Trang cài đặt của facebook app vừa tạo, điền vào mục email.
- Bật chế độ công khai cho facebook app

 ![](https://lh3.googleusercontent.com/MPeAJQipCsV6aROcbod0VhZWD-9lmvnwqgeUGrpyWOWUt7f4DSioOm1kFcs-TZarE1wWrJCSC0pz9u19iG3ZiMmkgCgYyIAU=w2400-h1350-rw-no)

-
 Hình tròn màu xanh bên Tên ứng dụng sẽ sáng lên 
 ![](https://lh3.googleusercontent.com/Hizf2h7BREEgBNw0HSpVX6NPc_M4k78BpowYkldz3U9hNPGbMlq7Q5uMdFzZSMcuR-P6LrGyrGFL8PRe2CPDiRhDE5VSrJTX=w2400-h1350-rw-no)

_[Chi tiết](http://blog.cocos2d-x.org/2016/02/setting-up-facebook-app-for-sdkbox-services/)_









**5. Sử dụng**

- Trước tiên, include vào project:

*#include &quot;PluginFacebook/PluginFacebook.h&quot;

- Khởi tạo facebook trong hàm AppDelegate::applicationDidFinishLaunching():
AppDelegate::applicationDidFinishLaunching()
{

*#ifdef SDKBOX\_ENABLED
                     sdkbox::PluginFacebook::init();

*#endif
        }

- Các hàm để sử dụng facebook:

        sdkbox::PluginFacebook::login();

        &lt;Đăng nhập vào facebook để sử dụng&gt;

        sdkbox::PluginFacebook::logout();

        &lt;Đăng xuất&gt;

        sdkbox::PluginFacebook::isLoggedIn();

        &lt;Kiểm tra xem user đã log in hay chưa&gt;

- Quyền truy cập:

        Facebook có 2 loại quyền truy cập là publish và read _(Chi tiết_ [_https://developers.facebook.com/docs/facebook-login/permissions/v2.3#reference_](https://developers.facebook.com/docs/facebook-login/permissions/v2.3#reference)_)_

        Để yêu cầu quyền truy cập đến facebook cá nhân của người dùng cần gọi hàm:

sdkbox::PluginFacebook::requestReadPermissions();
sdkbox::PluginFacebook::requestPublishPermissions();

Các tham số truyền vào thường dùng:

FB\_PERM\_READ\_PUBLIC\_PROFILE

FB\_PERM\_READ\_EMAIL

FB\_PERM\_READ\_USER\_FRIENDS

FB\_PERM\_PUBLISH\_POST

Ví dụ: sdkbox::PluginFacebook::requestReadPermissions({sdkbox::FB\_PERM\_READ\_PUBLIC\_PROFILE, sdkbox::FB\_PERM\_READ\_USER\_FRIENDS});
sdkbox::PluginFacebook::requestPublishPermissions({sdkbox::FB\_PERM\_PUBLISH\_POST}); 
![](https://lh3.googleusercontent.com/pPtS-OTNEWNUm_QUo8sT9KUJo-tAYv_x48tBU-bB_xCwBHqWsPlrKtqbEzt-7iMdtB52-swFoWTQMUSOLlDKtr2fOwKCsM70=w2400-h1350-rw-no)







- Chia sẻ ảnh:

sdkbox::FBShareInfo info;
info.type  = sdkbox::FB\_PHOTO;
info.title = &quot;New Dragon Hunting game&quot;;
info.image = &quot;icon.png&quot;;
sdkbox::PluginFacebook::share(info);

 Thay hàm share  thành sdkbox::PluginFacebook::dialog(info); để hiện lên mục edit nội dung bài viết user muốn share

Chụp ảnh màn hình trong cocos2d: http://www.cocos2d-x.org/wiki/Take\_a\_Screenshot

 ![](https://lh3.googleusercontent.com/_aQVgKXXKVc0fqFsEWmlWzsU7MFgESOW0NL_oAPHW8EDkA1O6V1e20-Uuig399jW6XKjfPy5lAK_BTybgLsLQEX4FcMWvRcJ=w2400-h1350-rw-no)

- Chia sẻ đường dẫn:

sdkbox::FBShareInfo info;
info.type  = sdkbox::FB\_LINK;
info.link  = &quot;http://www.cocos2d-x.org&quot;;
info.title = &quot;cocos2d-x&quot;;
info.text  = &quot;Best Game Engine&quot;;
info.image = &quot;http://cocos2d-x.org/images/logo.png&quot;;
sdkbox::PluginFacebook::share(info);

Tương tự, thay share thành dialog(info) để user viết nội dung.

- Một số API khác:

static std::string getUserID ( ) ;

static void logout ( ) ;

static void fetchFriends() ;



_[Chi tiết bài viết](http://docs.sdkbox.com/en/plugins/facebook/v3-cpp/)_
