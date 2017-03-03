#**Hướng dẫn cái đặt plugin In-app purchases sử dụng SDKBOX**
**1. Chạy câu lệnh bằng cmd:**
    sdkbox import iap
**2. Sửa file** ./Resource/sdkbox_config.json
    Điền key google play vào android->iap->key
**3. Sử dụng**
    .#include "PluginIAP/PluginIAP.h"
    ...
    AppDelegate::applicationDidFinishLaunching()
    {
      sdkbox::IAP::init();
    }
