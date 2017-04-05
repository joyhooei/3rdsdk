**Hướng dẫn cài đặt plugin Google Play Service sử dụng SDKBOX**

**1. Cài đặt SDKBOX:** (Xem bài google analytics).

**2. Chạy câu lệnh trên command prompt:**

    sdkbox import gpg

**3. Sửa file** android/res/values/string.xml điền app id google.

        3.1. Lấy ID của Leaderboard

        3.2. Tạo và lấy ID Achievement

**4. Khởi tạo plugin** trong AppDelegate::ApplicationDidFinishLaunching()

  sdkbox::PluginGPG::init();
  
  *Đối với nền tảng iOS, trước khi sử dụng cần mở Xcode -> cài đặt project -> add thêm các framework trong thư mục /project_ios_mac/ vào project*

**5. Khởi tạo biến game service:       **

 std::unique\_ptr&lt;gpg::GameServices&gt; gameServices = gpg::GameServices::Builder()

  .SetOnAuthActionFinished([](gpg::AuthOperation op, gpg::AuthStatus status){

 })

  .EnableSnapshots()

  .SetDefaultOnLog(gpg::LogLevel::VERBOSE)

  .Create(\*CreatePlatformConfiguration().get());

**6. Đăng nhập tài khoản:**

  gameServices-&gt;StartAuthorizationUI();

**7. Một số API:**

Unlock Achievement:

if (gameServices-&gt;IsAuthorized()) {

                gameServices-&gt;Achievements().Unlock(achievementId);

}

Show Achievement:

 if (gameServices-&gt;IsAuthorized()) {

gameServices-&gt;Achievements().ShowAllUI([](gpg::UIStatus const &amp;status) {});

}

Submit high score:

 if (gameServices-&gt;IsAuthorized()) {

        gameServices-&gt;Leaderboards().SubmitScore(leaderboardId, score);

}

Show high score:

 if (gameServices-&gt;IsAuthorized()) {

gameServices-&gt;Leaderboards().ShowUI(leaderboardId, [](gpg::UIStatus const &amp;status) {});

}

Sign out:

gameServices-&gt;SignOut();



_Chi tiết:_

_https://developers.google.com/games/services/cpp/GettingStartedNativeClient_

_Project mẫu: https://github.com/sdkbox/sdkbox-sample-gpg/blob/master/cpp/Classes/StateManager.cpp_
