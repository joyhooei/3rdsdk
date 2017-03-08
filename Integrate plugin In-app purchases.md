#**Hướng dẫn cái đặt plugin in-app purchases sử dụng SDKBOX**

**1. Chạy câu lệnh bằng cmd:**

    sdkbox import iap
![](https://lh3.googleusercontent.com/QovSDqsnIO4Vhf4bCNIxvLmhThC9VXGAwIuIaCMFiUtf4Z7-3FAFro4Hg5mG-bfJIvd4LlxP4hhG-CODpPG9rjtP0LrOt94m=w2400-h1350-rw-no)
    
**2. Sửa file** ./Resource/sdkbox_config.json

    Điền key google play vào android->iap->id'
    type có 2 loại: "non_consumable" thể hiện loại hàng này chỉ cần mua 1 lần. Ví dụ: Mở khóa nhân vật, level pack,..
                    "comsumable" ngược lại, có thể mua nhiều lần. Ví dụ: Vàng, nâng cấp,..
![](https://lh3.googleusercontent.com/yKC2N3pja96vNdAcNMtmghitPonmJP55gSwmZMbODgIzI8A81kI1nklnVP5iaj3k6NGyyfmY1rfWm1PXtVV1axlU4yooGGkV=w2400-h1350-rw-no)

**3. Sử dụng**

Khởi tạo:

    #include "PluginIAP/PluginIAP.h"
    ...
    AppDelegate::applicationDidFinishLaunching()
    {
      sdkbox::IAP::init();
    }
![](https://lh3.googleusercontent.com/GwHhwyJOGHWHtzXKrBmiMpShui72cyKk4XgBFVUxn-3mauiVQlnrjMw_m0PUWgCzYY2rK3L3c8OGBGjs5dxUFwDOpkyWcpUh=w2400-h1350-rw-no)

Kế thừa lớp listener:

    #include "PluginIAP/PluginIAP.h"
    ...
    class HelloWorld : public cocos2d::Layer, public sdkbox::IAPListener
    {
    ...
    }
        
Khi kế thừa cần Override lại các hàm:


    virtual void onInitialized(bool ok) override;
    virtual void onSuccess(sdkbox::Product const& p) override;
    virtual void onFailure(sdkbox::Product const& p, const std::string &msg) override;
    virtual void onCanceled(sdkbox::Product const& p) override;
    virtual void onRestored(sdkbox::Product const& p) override;
    virtual void onProductRequestSuccess(std::vector<sdkbox::Product> const &products) override;
    virtual void onProductRequestFailure(const std::string &msg) override;
    void onRestoreComplete(bool ok, const std::string &msg);
    
Đặt listener cho Scene:

    sdkbox::IAP::setListener(this);
    
- Các API:

Bật/tắt debug

    static void setDebug(bool debug);
Lấy các sản phẩm

    static std::vector <Product> getProducts();
Gửi yêu cầu mua

    static void purchase(const std::string & name);
    name ở đây là "coin_package", "remove_ads"... config trong file json. Không phải tên id của gói hàng.
    onSuccess được gọi khi mua hàng thành công
    onFailure được gọi khi mua hàng thất bại
    onCanceled được gọi khi hủy hàng
Cập nhật thông tin mua hàng (title, price, description)

    static void refresh();
    onProductRequestSuccess sẽ được gọi nếu request thành công
    onProductRequestFailure sẽ được gọi nếu xảy ra ngoại lệ

Khôi phục mua hàng

    static void restore();
    Khi restore thàh công, hàm onRestored đc gọi. Gọi hàm này khi muốn phục hồi cái item non-consumable đã mua của user. Ví dụ:
    void HelloWorld::onRestored(const sdkbox::Product& p)
    {
        if(p.name == "YOUR_IAP_NAME")
        {
            UserDefault::getInstance()->setBoolForKey("YOUR_STRING_HERE", true);
            UserDefault::getInstance()->flush();
        // do something here
    }
    }
+ Listeners:

Hàm được gọi khi đã đăng nhập/ủy quyền

    void onInitialized(bool success);
Khi mua hàng thành công

    void onSuccess(const Product & p);
Khi mua hàng thất bại

    void onFailure(const Product & p, const std::string & msg);
    
Khi restore

    void onRestored(const Product & p);
Khi hủy mua hàng

    void onCanceled(const Product & p);
*[Chi tiết bài viết](http://docs.sdkbox.com/en/plugins/iap/v3-cpp/)*

*[Project mẫu](https://github.com/sdkbox/sdkbox-sample-iap/tree/master/cpp/Classes)*
