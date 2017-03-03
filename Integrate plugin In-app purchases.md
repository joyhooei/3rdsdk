#**Hướng dẫn cái đặt plugin in-app purchases sử dụng SDKBOX**

**1. Chạy câu lệnh bằng cmd:**

    sdkbox import iap
    
**2. Sửa file** ./Resource/sdkbox_config.json

    Điền key google play vào android->iap->key
    
**3. Sử dụng**

Khởi tạo:

    #include "PluginIAP/PluginIAP.h"
    ...
    AppDelegate::applicationDidFinishLaunching()
    {
      sdkbox::IAP::init();
    }

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
Cập nhật thông tin mua hàng (title, price, description)

    static void refresh();
Khôi phục mua hàng

    static void restore();
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
