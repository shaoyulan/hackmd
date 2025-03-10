---
title: '**Capacitor IOS**'

---

# **Capacitor IOS**

# **開發流程**

**==推播設置==**
* [Capacitor官方文件](https://capacitorjs.com/docs/guides/push-notifications-firebase#prerequisites)
* [Firebase、ios憑證設置相關詳細教學](https://medium.com/yanweilun/firebase-%E7%99%BC%E9%80%81%E7%B3%BB%E7%B5%B1%E9%80%9A%E7%9F%A5-fcm-%E8%87%B3ionic-vue-ios-app-1aae6106a37)
* [Sign In with Apple JS](https://dylan-blog.pages.dev/posts/2020-09-11_javascript-web-apple-sign-in/#google_vignette)

**==TestFlight設置==**

只能將版本 X.X.X 的其中一個建置版本提交 Beta 版 App 審查。待提交的建置版本通過核准後，即可提交其他建置版本。

- 在Test Flight可查看安裝本App的人員、裝置型號、OS版本(TestFlight>測試人員>全部)

**==Sign In with Apple==**

只要你的 App 中有包含任一第三方登入就必須支援 Sign in with Apple，要不然就無法通過審核上架。要啟用Sign In with Apple需要具備一個**Apple開發人員帳號**

- [[iOS] 跟 Sign in with Apple 的愛恨情仇](https://dylan-blog.pages.dev/posts/2020-09-11_javascript-web-apple-sign-in/)
- [整合 Sign in with Apple 到自己的 iOS App](https://medium.com/@tuzaiz/%E5%A6%82%E4%BD%95%E6%95%B4%E5%90%88-sign-in-with-apple-%E5%88%B0%E8%87%AA%E5%B7%B1%E7%9A%84-ios-app-%E4%B8%8A-ios-backend-e64d9de15410)
- [Implementing User Authentication with Sign in with Apple](https://developer.apple.com/documentation/authenticationservices/implementing_user_authentication_with_sign_in_with_apple)
- [Sign In with Apple(Apple 登入)](https://franksios.medium.com/ios-sign-in-with-apple-apple-%E7%99%BB%E5%85%A5-9b175596b69)
- [Configuring your webpage for Sign in with Apple](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js/configuring_your_webpage_for_sign_in_with_apple)

**==IOS App簽名機制==**
- [iOS App Distribute flow](https://hackmd.io/nKw3RWGURbil77eFIa6V-g?view#4-Run-your-app-on-a-device)

**==自訂Capacitor Plugin==**

自訂Ios Capacitor套件只需要透過Xcode新增2隻檔案即可完成，往後不用再新增，可把相關功能都直接集中在此套件暴露給JS端即可
- [Custom Native iOS Code](https://capacitorjs.com/docs/ios/custom-code#webview-accessible-native-code)

**==原生程式相關說明==**

**使用者裝置網路狀態偵測**

```swift=
import Network

// Create a Network Monitor.
let monitor = NWPathMonitor()

// Define a Handler for Network Changes.
monitor.pathUpdateHandler = { path in
    if path.status == .satisfied {
        print("Internet connection is available.")
        // Perform actions when internet is available
    } else {
        print("Internet connection is not available.")
        // Perform actions when internet is not available
    }
}

//Start the Network Monitor.
let queue = DispatchQueue(label: "NetworkMonitor")
monitor.start(queue: queue)
```

**啟用可Inspect模式**
```swift=
let webConfiguration = WKWebViewConfiguration()
let webView = WKWebView(frame: .zero, configuration: webConfiguration)
webView.isInspectable = true
```

**離線顯示頁面設定**
```swift=
// AppDelegate.swift

import UIKit
import ReachabilitySwift

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    var reachability: Reachability!

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        // Initialize Reachability
        self.reachability = Reachability()
        
        // Register for network reachability changes
        NotificationCenter.default.addObserver(self, selector: #selector(networkChanged(_:)), name: .reachabilityChanged, object: nil)
        
        // Start monitoring network reachability
        do {
            try self.reachability.startNotifier()
        } catch {
            print("Unable to start notifier")
        }
        
        // Check initial network status
        if reachability.connection == .none {
            // If there's no network, load the offline HTML file
            showOfflineHTML()
        }
        
        return true
    }
    
    @objc func networkChanged(_ notification: Notification) {
        guard let reachability = notification.object as? Reachability else { return }
        
        if reachability.connection == .none {
            // If network becomes unavailable, show the offline HTML file
            showOfflineHTML()
        } else {
            // If network becomes available, dismiss the offline HTML file if it's presented
            dismissOfflineHTML()
        }
    }
    
    func showOfflineHTML() {
        guard let offlineURL = Bundle.main.url(forResource: "offline", withExtension: "html") else {
            return
        }
        
        let offlineViewController = UIViewController()
        let webView = UIWebView(frame: UIScreen.main.bounds)
        webView.loadRequest(URLRequest(url: offlineURL))
        offlineViewController.view = webView
        
        // Present the offline HTML file on the window's root view controller
        self.window?.rootViewController?.present(offlineViewController, animated: true, completion: nil)
    }
    
    func dismissOfflineHTML() {
        if let presentedViewController = self.window?.rootViewController?.presentedViewController {
            presentedViewController.dismiss(animated: true, completion: nil)
        }
    }
}
```
**==啟用JavaScript Interface的設定==**

- [關於Ios啟用JavaScript Interface的說明](https://stackoverflow.com/questions/62319327/how-do-i-add-web-interface-functions-to-my-swift-webview)
- [Ios啟用JS Iinterface](https://stackoverflow.com/a/37373745)

**==Xcode開發設定==**

- [調整模擬器網速的 Network Link Conditioner](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E8%AA%BF%E6%95%B4%E6%A8%A1%E6%93%AC%E5%99%A8%E7%B6%B2%E9%80%9F%E7%9A%84-network-link-conditioner-e249e2875be6)

**==常見問題==**
- [**打包過程出現DT_TOOLCHAIN_DIR cannot be used to evaluate LIBRARY_SEARCH_PATHS, use TOOLCHAIN_DIR instead**](https://blog.csdn.net/shihuboke/article/details/133769390)
    - 全域搜尋替換DT_TOOLCHAIN_DIR
    - 替換@capacitor/ios套件版本再試試
- [多个app可以使用同一个ios证书吗？](https://www.applicationloader.net/doc/questions/multiple-apps-share-the-same-certificate.html)
    - 不建議的做法
- 在mac使用safari debug webview app
    - 開啟safari > 設定 > 進階 > 在選單中顯示「開發」選單
    - 開起Xcode並run app，開啟模擬器後
    - Safari > 開發 > 選取「模擬器--iPhone XX -- IOS XX (XXX)選項」
- [產生不重複讀一無二的bundle identifier](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%BB%BA%E7%AB%8B%E6%96%B9%E4%BE%BF%E5%A4%A7%E5%AE%B6%E5%AE%89%E8%A3%9D%E5%88%B0%E6%89%8B%E6%A9%9F%E7%9A%84-xcode-%E5%B0%88%E6%A1%88-%E6%90%AD%E9%85%8D-xcconfig-team-id-fb072ed08b2f)
- [IOS Deep Link驗證](https://developer.apple.com/documentation/technotes/tn3155-debugging-universal-links#Host-and-verify-your-AASA)
    - [驗證網站AASA檔案](https://branch.io/resources/aasa-validator/)
    - [使用iphone手機驗證](https://forums.developer.apple.com/forums/thread/731605)
- Error: java not found on path and JAVA_HOME not set. Please set JAVA_HOME to the root of your Java installation.
    - 請先確認已安裝android studio
        - [同capacitor文檔說明，安裝好android studio就不用自行安裝java了](https://capacitorjs.com/docs/getting-started/environment-setup)
    - 檢查@capacitor/android、@capacitor/ios、@capactior/cli都更新到latest版本

:::warning
- App名稱若要加上空白可將Yaml檔中的app名稱中的空白替換成\u2007。
:::



# **上架流程**
- [ ] **App名稱有空白的處理**
    - App名稱若要加上空白可將Yaml檔中的app名稱中的空白替換成`\u2007`。
- [ ] 檢查app所需要的原生套件都有在set-config.yaml裡的設定引入
- [ ] [設定隱私權政策](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E7%94%A2%E7%94%9F-app-%E4%B8%8A%E6%9E%B6%E9%9C%80%E8%A6%81%E7%9A%84%E9%9A%B1%E7%A7%81%E6%AC%8A%E6%94%BF%E7%AD%96-url-7bc4746cf75d)
    - [範例參考網址](https://sports-app.tesla168.xyz/?privacy#!/privacy-policy/)
- [ ] 設定支援連結([線上工具產生內容點我](https://www.contactuspagegenerator.top/))
    - [範例參考網址](https://sports-app.tesla168.xyz/?contact#!/contact/)
- [ ] 準備對應圖檔素材/App文案([iphone模擬器截圖方式教學](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%A6%82%E4%BD%95%E7%94%A2%E7%94%9F%E7%AC%A6%E5%90%88%E4%B8%8A%E6%9E%B6%E5%B0%BA%E5%AF%B8%E7%9A%84-screenshot-%E8%9E%A2%E5%B9%95%E6%88%AA%E5%9C%96-c6a265335802))
    - iOS 預覽和截圖
        - 可只上傳6.7吋(6.5吋裝置將共用此設定)、5.5吋顯示器的截圖　
        - 6.5吋：iPhone 15 Pro Max、5.5吋：iPhone 8 Plus
    - 銷宣傳文字(Promotional Text)：讓你不需提交更新要求，即可通知 App Store 訪客目前最新的 App 功能
        - iOS 11（或以上版本）及 macOS 10.13（或以上版本）的客戶，此文字將會顯示在 App Store 中「描述」的上方。
    - 描述(Description)：詳細介紹 App 的特色和功能
        - 不支援 HTML 格式
    - 關鍵字：讓 App Store 搜尋結果更加準確。可使用英文逗號、中文逗號或兩者混用來分隔不同的關鍵字
- [ ] 打包、驗證app(validate app)、上傳app(distribute app)
    - [打包前App預設語言確認](https://blog.csdn.net/adojayfan/article/details/135067272)
    - 此步驟完成後接下來的流程即可到App Store Connect後續操作
- [ ] TestFlight 測試(須審核，時間約1~2天)
    - 內部測試：需要經由開發者帳號的負責人發送邀請，只有取得邀請函的人可測試App，並且須擁有AppleID才能進入TestFlight做操作測試
    - 外部測試：簡單透過外部群組取得分享link，那被發連結邀請的人就能從TestFlight下載App來測試並查看
- 一般資訊>App審查資訊填寫
    - 手機: 格式須為+886開頭(ex: +886900180111)
- [ ] 上架前隱私權政策確認
    - [具備「管理」角色的使用者必須前往「App 隱私權」區段，並提供該 App 隱私權實施內容的相關資訊](https://developer.apple.com/cn/help/app-store-connect/manage-app-information/manage-app-privacy/)

:::warning
- 使用Xcode上傳(distribute)的App如果最後在TestFlight中消失，代表App有問題，請去信箱收信，並依照要求調整
- App在Distribute時建置版本會由apple線上系統自行遞增，所以即便在set-config.yaml裡將buildNumber設定成相同建制版本號是會在distribute後被apple忽略的
:::


**==常見問題==**
- [App 送審教戰守則](https://p5d12000.medium.com/app-%E9%80%81%E5%AF%A9%E6%95%99%E6%88%B0%E5%AE%88%E5%89%87-8dc46aef5afd)
- [蘋果送審被拒原因與處理](https://cloud.tencent.com/developer/article/1366045)
- [蘋果App審核被拒原因與解決方案【2023最新】](https://seoasoorm.com/zh-tw/apple-app-review/)
- [苹果开发上架常见问题汇总](https://www.applicationloader.net/doc/questions.html)
- [iOS App 上架流程图文教学](https://juejin.cn/post/7245183742264082489)
- [憑證產生](https://www.potatomedia.co/post/591b0c37-e7fa-4cb3-b5bd-ef2bd48a859e)
- 出口合規資訊-App 會使用哪種加密演算法
- [如何填寫SKU](https://stackoverflow.com/a/16225115)
    - 預設情況下Xcode 上傳(distrubute)的app SKU已經自動填寫好了
    - 除非是手動在App Store Connect新增app(建議透過Xcode直接上傳)，才需要填寫SKU
- [解决IOS上架App Store后显示语言为英文的问题](https://blog.csdn.net/adojayfan/article/details/135067272)

**==上架審核問題==**
- [苹果审核的沟通与申诉的一些见解](https://mp.weixin.qq.com/s?__biz=MzU4MjAzMjMxOA==&mid=2247484353&idx=1&sn=6139e6d2849093616aedaa17200fb6a7&chksm=fdbfcd10cac844069ded12be7f38b5a18b23674cbfa69155b9eaf3c73c930786b7437e89a9e0&token=654263058&lang=zh_CN#rd)
- 4.1.0 Design: Copycats
    - [4.1 Design-Copycats: What does that mean?](https://forums.developer.apple.com/forums/thread/129591)
    - 案例：[Sports Char](https://apps.apple.com/tw/app/sports-char/id6476840353?platform=iphone)
        - 解決方式：移除app內所有第三方logo

**==上架提醒==**
:::warning
年齡分級
- [你的 App 是否具有未加限制的網頁存取能力，例如含有嵌入式瀏覽器？](https://stackoverflow.com/questions/24462543/what-does-unrestricted-web-access-mean-in-itunes-connect#comment38644593_24462676)
    - 只要App內沒有提供如同瀏覽器的功能(使用者能在app內輸入網址並瀏覽網頁)，就可已選「**否，此 App 不具有未加限制的網頁存取能力**」
- [設定年齡分級](https://developer.apple.com/tw/help/app-store-connect/manage-app-information/set-an-app-age-rating)
:::

- [審核被拒後不要急著重新提交更新](https://mp.weixin.qq.com/s?__biz=MzU4MjAzMjMxOA==&mid=2247484353&idx=1&sn=6139e6d2849093616aedaa17200fb6a7&chksm=fdbfcd10cac844069ded12be7f38b5a18b23674cbfa69155b9eaf3c73c930786b7437e89a9e0&token=654263058&lang=zh_CN#rd)

:::info
回覆App審核團隊英文範本:

Dear App Review Team,

Thank you for bringing the concern related to Guideline 2.1 and Guideline 4.1 to our attention. 
We appreciate your diligence in ensuring the integrity of the App Store.

Guideline 2.1
1.What is the function of the jackpot seen in your app?
The function of the jackpot is decorative, intended to enhance the visual richness of the app interface.

2.Can users win any prize money in the app?
No, users cannot win any prize money in the app.


Guideline 4.1
Our app primarily serves as a convenient channel for Japanese users to access sport information. 

Thank you once again for your feedback. We are willing to provide any additional information required for your review to facilitate a smooth approval process.

Sincerely,
SHAO YU LAN
:::

# **審查流程**

- 準備提交
- 準備審查
- 正在等待審查
- 審查中
- 已拒絕、等待開發者發佈、被開發者拒絕
- 正在為 App Store 處理
    - 開發者手動發佈後須等待App Store處理的時間(時間很快)
- 已可發布
    - App Store處理中，此時更新尚未反映到App Store(時間很快)
- 銷售準備就緒
    - 已上架App Store
    - 已上架後仍可修改且不用送審的項目如下：
        - 行銷宣傳文字
        - 版權顯示文字
        - 路線規劃 App 地區範圍檔案
        - App 審查資訊區塊(登入資訊、聯絡人資訊、備註、附件)
        - App 資訊>內容版權選項
        - App 資訊>許可協議選項
        - 定價與供應狀況頁面
:::info
App商店網址格式為：https://apps.apple.com/{地區代號}/app/{app名稱}/id{AppleID}
- app名稱：App Store Connect > App資訊 > 名稱
- 地區代號：台灣為tw、日本為jp
- AppleID：App Store Connect > 一般資訊 > Apple ID
:::

# **Store 商店**
- 多語系
    - [App Store多語系設定](https://developer.apple.com/tw/help/app-store-connect/manage-app-information/localize-app-store-information)
    - [App Store商店多語系支援表格](https://developer.apple.com/tw/help/app-store-connect/reference/required-localizable-and-editable-properties)

> App Store多語系設定：如果你在 App Store Connect 中選取英文作為 App 的主要語言，且英文是你唯一提供的語言版本，那麼在所有 App Store 國家或地區，App 後設資料都會以英文呈現。如果你提供了法文版的 App 後設資料，將語言設定為法文的使用者，就會看到法文的本地化內容

- 行銷宣傳文字：
    - [行銷宣傳文字對應顯示位置](https://blog.rainvisitor.me/images/ios-app-deploy/img6-1.png)
    - 如果商店沒有設定使用者對應語系的行銷宣傳文字，則本內容不會顯示給使用者
    - 會顯示在 App Store 中「描述」的上方
    - 非必填項目
- 描述：詳細介紹 App 的特色和功能。
    - **會固定顯示**，無論使用者當前的語系
    - 會顯示在App Store 詳細介紹頁面(行銷宣傳文字下方)
    - 必填項目
- 在App Store Connect>定價與供應狀況>採用 Apple 晶片 Mac 上的 iPhone 與 iPad App中是否勾選「供應此 App」的差異
    - 有勾選
        - 商店將顯示顯示「請開啟 Mac App Store 以購買和下載 App。」
        - 且將額外顯示「專為 iPad 設計」或「專為 iPhone 設計」
![202402071](https://hackmd.io/_uploads/HyjWP3xjp.png)
    - 無勾選
        - 商店將顯示顯示「此 App 只能透過 iPhone 及 iPad 的 App Store 取得。」
![202402072](https://hackmd.io/_uploads/HJBLv2lop.png)
- 版本紀錄
    - App版本紀錄>1時才會在app store顯示版本紀錄連結


# **其他備註**
- 在Xcode裡的設定在app distribute到app store connect後，app store connect的設定會自動同步，不需要另外設置(因此可以在Xcode操作的步驟可以優先在Xcode操作就好)
- [apple-app-site-association: apple會快取這個資源最多7天，因此即便我們修改了這支檔案，設定值可能不會馬上看到變化](https://capacitorjs.com/docs/guides/autofill-credentials)

# **資源**
* [驗證網站AASA檔案](https://branch.io/resources/aasa-validator/)
* [Appuploader](https://www.applicationloader.net/)
* [Apple Downloads All](https://developer.apple.com/download/all/)

# **名詞**
* App Developer Program
    * [文檔](https://developer.apple.com/tw/help/app-store-connect/get-started/app-store-connect-homepage)
* APNs(Apple Push Notification service)
    * APN 驗證金鑰、 APN 憑證差異
* App store connect: Apple提供給開發者用來管理app資訊、提交app審核、查看app資料等統一的平台
* .cer凭证档: 告诉Apple 说这台电脑是开发者在使用的。(需手動新增)
* SKU(Stock Keeping Unit)
* Provisioning Profile: 告诉Apple 说，即将要被上传的App 资讯，包含凭证档(.cer) 和App ID 等 (如果是使用Xcode自動簽署，則本profile會自動產生)
* CSR檔(certSigningRequest): 用以到Apple Developer後台產生.cert檔
* [個人開發者帳號、公司開發者帳號、企業開發者帳號差異](https://www.applicationloader.net/doc/questions/the_difference_between_ios_developer_accounts.html)
    * 個人開發者帳號是無法將開發權分配出去的，因此只能使用自己的帳號開發。(app store connect中加入的團隊成員只具有app管理權限，無法參與開發)
    * 公司開發者帳號可以讓加入的團隊成員具有開發app的能力。(申請約需1周，且須先申請好邓白氏码)
* 輕巧APP
* iMessage App
* validate app:上傳前驗證app
* [嵌入式瀏覽器](https://stackoverflow.com/a/74855149)
* App 專用共享密鑰
    * App 專用共享密鑰為一組獨有的代碼，用於接收此 App 自動續訂型訂閱的收據。若要將 App 轉讓給其他開發者，或者不想公開主共享密鑰，建議使用 App 專用共享密鑰。
* 軟體版本管理慣例
* [automatic signing](https://help.apple.com/xcode/mac/current/#/dev8a2822e0b)
    * If you use automatic signing (recommended), Xcode creates signing certificates and provisioning profiles for you
    * since signing certificate private keys are stored in your keychain, you should back up the developer account
    * Downloading your provisioning profiles in Xcode won’t repair a missing private key. Instead, import your developer accounts from a backu
* Apple Push Notifications service Key (APNs):Establish connectivity between your notification server and the Apple Push Notification service. One key is used for all of your apps
* 沙箱測試帳號
* [帳單寬限期](https://developer.apple.com/help/app-store-connect/manage-subscriptions/enable-billing-grace-period-for-auto-renewable-subscriptions)
* [自訂產品頁面](https://developer.apple.com/cn/app-store/custom-product-pages/)
* App 內活動
* [產品頁面最佳化](https://developer.apple.com/app-store/product-page-optimization/)
* 重置 iOS 平均評分
* 促銷代碼
* MapKit JS
* WeatherKit
* [Device reset date](https://developer.apple.com/xcode-cloud/get-started/)
* Code-level support
    * Included with your membership are two Technical Support Incidents (TSIs), which can be used during your membership year to request code-level support for Apple frameworks, APIs, and tools from an Apple Developer Technical Support Engineer. You’ll receive two new TSIs when you renew your membership. Additional TSIs are available for purchase at any time.
* [Apple Push Notifications Console](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%BE%9E-apple-%E7%9A%84-push-notifications-console-%E6%B8%AC%E8%A9%A6%E6%8E%A8%E6%92%AD%E5%8A%9F%E8%83%BD-b1c9390e0f64)
    * [Testing notifications using the Push Notification Console](https://developer.apple.com/documentation/usernotifications/testing_notifications_using_the_push_notification_console#4181181)
* Ad Hoc
* 鄧白氏環球編碼
    * 申請時間約需1-2個星期
* Sign with Apple
    * 苹果登录按钮必须按照开发者官网上的两种设计方式做，既不能改变外观，也不能改变大小。
* IOS第三方應用程式(側載)
* [階段性發佈 App Store 自動更新](https://developer.apple.com/help/app-store-connect/update-your-app/release-a-version-update-in-phases)
* CloudKit Database
* Xcode Cloud
    * [可建立CICD工作流程](https://lihsinplayer.medium.com/xcode-cloud-%E7%9A%84cicd-56afbba26f0a)
    * 需要收費
* [Bundle Name vs Bundle Display Name](https://cg2010studio.com/2018/01/07/ios-bundle-name-%E5%92%8C-bundle-display-name/)
    * Bundle Name：會出現在像詢問權限的提示視窗
    * Bundle Display Name：顯示在桌面上App的名稱
* Apple 中介 Email
* [Sign In with Apple的Client Id](https://stackoverflow.com/a/56449429)
* [Entitlement](https://developer.apple.com/documentation/bundleresources/entitlements)：是由Xcode生成的檔案(例如開啟App的某個Capability時，Xcode會自動新增一個.entitlements檔案)。用來授權App能夠執行特定功能或是安全方面的權限
* SPM(Swift Package Manager)：新推出官方的IOS開發套件管理工具，用來取代cocoapods



# **待處理**
* Testflight客戶使用教學
    * [參考資源](https://fliphtml5.com/zh_tw/gfoff/ntap/basic)
* [Xcode配置多個config](https://www.appcoda.com.tw/xcconfig-guide/)
* 以預訂方式發佈你的 App
* [Xcode Cloud](https://developer.apple.com/xcode-cloud/get-started/)
* [Cloud Kit](https://icloud.developer.apple.com/dashboard/home/teams/X6TMYK35R3)
* Double  Click  Install 

# **參考**
* [Apple官方說明指南](https://developer.apple.com/tw/help/)
* [IOS 上架流程](https://www.joshmorony.com/deploying-capacitor-applications-to-ios-development-distribution/)
* [app上架流程-圖文簡述](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E6%95%99%E5%AE%A4/app%E4%B8%8A%E6%9E%B6%E6%B5%81%E7%A8%8B-%E5%9C%96%E6%96%87%E7%B0%A1%E8%BF%B0-241097887bae)
* [Publish IOS](https://hackmd.io/@XcodeIsComplicated/BJ7s-8-xO)
* [2023年苹果[IOS] App Store上架应用新流程-{原创图文} ](https://www.sohu.com/a/733533362_121779558)
* [申請iOS開發者帳號(Developer Account)&鄧白氏(DUNS)](https://medium.com/@AJIE-Impact-ON-Studio/2021%E5%AE%8C%E6%95%B4%E6%AD%A5%E9%A9%9F-%E7%94%B3%E8%AB%8Bios%E9%96%8B%E7%99%BC%E8%80%85%E5%B8%B3%E8%99%9F-developer-account-%E9%84%A7%E7%99%BD%E6%B0%8F-duns-%E4%B8%8A-b471fcf9d975)
* [App打包流程](https://www.joshmorony.com/deploying-capacitor-applications-to-ios-development-distribution/)
* [app上架-新增套件識別碼](https://alangprs1.medium.com/app%E4%B8%8A%E6%9E%B6-%E6%96%B0%E5%A2%9E%E5%A5%97%E4%BB%B6%E8%AD%98%E5%88%A5%E7%A2%BC-622226c37816)
* [將 App 上傳到 App Store Connect](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%B0%87-app-%E4%B8%8A%E5%82%B3%E5%88%B0-app-store-connect-2891ea8a758e)
* [將 iOS App 送審上架到 App Store](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%B0%87-ios-app-%E9%80%81%E5%AF%A9%E4%B8%8A%E6%9E%B6%E5%88%B0-app-store-2be449824ae2)
* [App Store Connect、Apple Developer網站的各身份存取權限說明](https://developer.apple.com/tw/help/app-store-connect/reference/role-permissions)
* [Apple App Developer新增成員說明](https://docs.google.com/document/d/1WDneYayqqRQddVUPBrb213HKu_zJS4h2jDRPkP3zZoE/edit)
* [Xcode 無法新增Capability可能原因](https://stackoverflow.com/a/58395558)
* [Signing & Capabilities workflow](https://developer.apple.com/documentation/xcode/configuring-an-associated-domain)
* [ios推播處理](https://capacitorjs.com/docs/guides/push-notifications-firebase#add-the-googleservice-infoplist-file-to-your-ios-app)
* [apple APNs設定](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/establishing_a_certificate-based_connection_to_apns)
* [Apple Developer Program新增團隊成員(只有企業帳號才有)](https://www.mobiloud.com/help-knowledge-base/how-to-invite-users-to-your-apple-developer-account)
    * 非企業帳號只能在App Store Connect管理成員
* [關於被邀請到個人開發者帳號作為團隊成員的限制](https://forums.developer.apple.com/forums/thread/117098?answerId=680965022#680965022)
* [apple app developer program個身份權限列表](https://developer.apple.com/cn/support/roles)
* [apple app資訊欄位概覽](https://developer.apple.com/tw/help/app-store-connect/reference/app-information/)
* [上架App store的整体流程于建议](https://sspai.com/post/55489)
* [上架App 注意事項](https://juejin.cn/post/7255557296951885885)
* [iOS 上架流程图文详解 (上)](https://developer.aliyun.com/article/1215480)
* [iOS不用上架就能發佈APP的5種方式](https://www.wanwaninfo.com/blog/iOS%E4%B8%8D%E7%94%A8%E4%B8%8A%E6%9E%B6%E5%B0%B1%E8%83%BD%E7%99%BC%E4%BD%88APP%E7%9A%845%E7%A8%AE%E6%96%B9%E5%BC%8F)
* [苹果商店上架流程_App上架苹果流程及注意事项](https://developer.aliyun.com/article/1203395)
* [ios新建APP SKU怎么填](https://blog.51cto.com/u_16213384/7591917)
* [iOS上架前的准备、上架技巧、常见上架问题及解决方案](https://developer.aliyun.com/article/853018)
* [上架流程商店資訊設定教學](https://www.potatomedia.co/post/759c38c7-d681-4aeb-ad5d-c3bdbc2c3097?s=bXiDs34D)
* [TestFlight 上傳教學](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E6%95%99%E5%AE%A4/testflight-%E4%B8%8A%E5%82%B3%E6%95%99%E5%AD%B8-99aabc6c91dd)
* [IPhone 尺寸列表](https://www.knowyourmobile.com/user-guides/iphone-size-comparison-chart/)
* [無需下架轉移APP將所有權轉讓給其他開發者帳號](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E6%95%99%E5%AE%A4/transfer-your-app-to-another-ios-developer-account-%E7%84%A1%E9%9C%80%E4%B8%8B%E6%9E%B6%E8%BD%89%E7%A7%BBapp%E5%B0%87%E6%89%80%E6%9C%89%E6%AC%8A%E8%BD%89%E8%AE%93%E7%B5%A6%E5%85%B6%E4%BB%96%E9%96%8B%E7%99%BC%E8%80%85%E5%B8%B3%E8%99%9F-97dd56dda415)
* [[iOS] Bundle name 和 Bundle display name](https://cg2010studio.com/2018/01/07/ios-bundle-name-%E5%92%8C-bundle-display-name/)Capacitor IOS**

# **開發流程**

# **上架流程**

# **名詞**
* App store connect: Apple提供給開發者用來管理app資訊、提交app審核、查看app資料等統一的平台