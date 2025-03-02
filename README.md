# **Capacitor說明**


:::info
Capacitor文件說明
- Capacitor版本：v5
- Android Studio版本：Android Studio Flamingo | 2022.2.1 Patch 2
- Xcode版本：15.2

截至2024/2/22關於V5升級V6的官方指引請參考[Capacitor 6 Beta Now Available](https://ionic.io/blog/capacitor-6-beta-now-available)、[Updating from Capacitor 5 to Capacitor 6](https://capacitorjs.com/docs/next/updating/6-0?__hstc=13779304.20d8e3253411143e44e6011d1af1b8ed.1708572366963.1708572366963.1708572366963.1&__hssc=13779304.1.1708572366963&__hsfp=2034566964&_gl=1*nokjyy*_ga*OTk1NjQyMjI1LjE3MDcyOTkwNjA.*_ga_REH9TJF6KF*MTcwODU3MjM1OC4yLjEuMTcwODU3Mjc1My4wLjAuMA..)
:::


Capacitor只是使用附加在原生專案上提供原生api功能。Capacitor不另外提供app打包工具，減少多了一層工具設定檔的繁雜設定。

開發android、ios都一樣有各自的原生專案，需要改原始碼直接進專案原始碼修改，capacitor端不需要其他對應設定。

**==Capacitor、Cordova差異==**

**Capactitor是Cordova的後繼大改動版本**。由於改動的部分過大，且已經有許多舊專案是基於Cordova進行開發的，如果以Cordova的方式直接升級將會造成升級難以進行。因此另起了一個名為Capacitor的專案

Capacitor在專案的整體管理、原生套件的引用模式都做出了更動，整體上來說提升了開發的DX，減少了專案開發原始碼容易因為原生系統改版後的不相容問題。

**==2種運作模式說明==**

- 前端資源打包進APP檔
  官方標準用法。後續更新服務可使用官方的[AppFlow](https://ionic.io/appflow)或是Google的[Fastlane](https://fastlane.tools/)
- Webview載入目標網頁網址 <font color="#E84855">(本篇說明使用的方式)</font>
  非官方推薦方式，[由社群討論出來的使用方式](https://github.com/ionic-team/capacitor/discussions/4080)。更新服務直接更新對應網站即可，無須額外服務。
  
**==Capacitor各平台支援度列表==**
 - [Capacitor各平台支援度列表](https://capacitorjs.com/docs/main/reference/support-policy)

# **環境建置**

- 請參照[Capacitor官方指引](https://capacitorjs.com/docs/getting-started/environment-setup)來安裝所需環境
- 安裝所需Node版本(參照package.json)，並使用`npm i`安裝npm所需套件
- cd進apps/app-template
- 初始化: 
  使用`npm run ready`來初始化並同步ios、android原生設置
- 驗證app開啟: 
  分別使用`npm run open-adnroid`、`npm run open-ios`來開啟淵生app編輯器介面，並使用模擬器開啟app

:::info
開啟android studio如有出現紅字Java JDK版本不相容問題:

則請至以下連結下載 jdk 17.0.8 並安裝
https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html

- 下載jdk安裝檔
- 開啟android studio > SDK manager>Build, Execution, Deployment > Build Tools > Gradle > Gradle SDK，設置為前項我們自己安裝好的JDK版本路徑
:::

:::info
使用npm run ready後出現錯誤訊息: xcode-select: error: tool xcodebuild requires Xcode

先確認當前xcode-select路徑: 
```
xcode-select -p
# /Applications/Xcode.app/Contents/Developer
```

如非以上路徑，則執行以下:
`sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer/`
:::

# **開發流程**

**==開新專案4步驟==**

* 建立新app專案
    * 執行`npm run add-app`，並輸入app名稱
      這個步驟會：
        * 複製app專案樣板(apps/app-template)，並重新命名為app名稱
        * 設定workspace：跟目錄package.json及app package.json
* 進入新app目錄，修改專案App變數 set-config.yaml，並執行`npm run ready`
* `npm run android` 開啟android 模擬器顯示app畫面

**==新專案其他設定==**

* 設定推播：更新google-services.json檔案
* 產生上架用圖檔：將logo.png放進`assets`資料夾，`npm run appicon`
* 同步cpacitor設定：當capacitor config有修改時，須執行`npm run sync`同步資料進原生專案
* 打包參照原生專案打包方式(Android studio、Xcode)

**==App簽章、商店素材、備份==**

- [公司Google帳號雲端硬碟](https://drive.google.com/drive/folders/1gEvEox8tgfHR7zlsSWtCn4Ix9OfJ9w80)
    - 資料夾/檔案說明
        - 檔案
            - App開發專案備份檔
            - [nodejs firebase推播發送測試程式](https://drive.google.com/drive/folders/14wF98_hYuWZatcsy--nEn_yLT1NtM7wI)
        - Android/keystore/通用/sign.jks
            - 公司Android開發簽署app用的keystore
            - 全部app共用此簽章
        - Apps/專案名稱/App Store：存放Apple App Store共用素材
        - Apps/專案名稱/Google Play：存放Google Play共用素材
        - Apps/專案名稱/Apple：存放此專案apple開發相關簽署檔案

**==專案結構說明==**

- 專案資料夾
    - apps：包含各app專案
    - src：前端UI src資料夾。只有當app採用將前端一併打包入app才會用到此資料夾。目前公司採用webview的方式，前端UI獨立於app打包檔
- 個別app資料夾
    - www
        - 這裡的檔案會被capacitor放進原生專案供取用
            - android會被丟進android\app\src\main\assets\public
            - ios會被丟進ios\App\App\public
        - 可以使用capacitor.config.json的webDir修改成其他資料夾
    - assets
        - 這裡存放@capacitor/assets套件會使用的資源，用以產生app所需要的圖檔、icon、splash screen圖檔
    - icons
        - @capacitor/assets產生出來的圖檔會輸出至此資料夾
    - capacitor-bridge
        - 打包前端呼叫原生api需要的函示庫原始檔，輸出的js檔會在output資料夾
- 檔案
    - set-config.yaml
        - 專案總設定檔
    - www/offline.html
        - 作為app offline時顯示的替代頁面
    - capacitor-bridge/output/capacitor-bridge.umd.js
        - 前端呼叫原生api需要的函示庫
    - vite.bridge-config.js
        - 用來打包前端呼叫原生api所需要的js函式庫
:::info
capacitor-bridge只有在網頁端時需要載入。App端capacitor會透過原生程式加載bridge。
因此需多判斷只有網頁環境再加載bridge script，不要直接將script直接在index.html預設引入，避免app額外的流量負擔
:::

**==使用原生功能說明==**

**原生Api掛載位置**

```javascript=
window.Capacitor.Plugins
```

**判斷當下的使用者環境**

```javascript=
window.Capacitor.platform // web | ios | android
window.Capacitor.isNative // true | false
```

**原生套件範例：取得App相關資訊**

```javascript=
const info = await window.Capacitor.Plugins.App.getInfo()
console.log(`App名稱: ${info.name}`)
console.log(`App ID: ${info.id}`)
console.log(`App build: ${info.build}`)
console.log(`App version: ${info.version}`)
```

**原生套件範例：取得App 操作**

```javascript=

//關閉app
await window.Capacitor.Plugins.App.exitApp()

//將App退到背景
await window.Capacitor.Plugins.App.minimizeApp()

//監聽App狀態變化()
await window.Capacitor.Plugins.App.addListener('appStateChange', listenerFunc(isActive){
 console.log(`App isActive: ${isActive}`)       
})

//監聽App Pause
await window.Capacitor.Plugins.App.addListener('pause', listenerFunc(){
  console.log('App Pause')
})

//監聽App 虛擬返回鍵(僅Android)
await window.Capacitor.Plugins.App.addListener('backButton', listenerFunc(){
  console.log('虛擬返回鍵被點擊!')
})

//監聽App Resume
await window.Capacitor.Plugins.App.addListener('resume', listenerFunc(){
  console.log('App Resume')
})
```
:::warning
監聽backButton事件會停止預設的虛擬返回鍵行為。因此需額外設定當返回鍵被點擊的行為，例如:返回上一頁、關閉App。
:::

**原生套件範例：取得裝置唯一碼、語系**

```javascript=
const idObj = await window.Capacitor.Plugins.Device.getId()
console.log(`裝置唯一ID: ${idObj.identifier}`)

// 回傳IETF BCP 47格式字串
const langTagObj = await window.Capacitor.Plugins.Device.getLanguageTag()
console.log(`語言: ${langTagObj.value}`) // zh-TW
```

**原生套件範例：提示視窗**

```javascript=
window.Capacitor.Plugins.Dialog.prompt({
　title: '訊息',
　message: '確定刪除訊息?',
　okButtonTitle: '確定',
　cancelButtonTitle: '取消'
})
```

**原生套件範例：監聽原生鍵盤事件**

```javascript=
window.Capacitor.Plugins.Keyboard.addListener('keyboardDidShow', info => {
  window.Capacitor.Plugins.Dialog.alert({
    title: '訊息',
    message: `鍵盤已顯示，鍵盤高度為: ${info.keyboardHeight}`,
  })
})
```
:::info
目前Webview App在原生鍵盤出現後會是Footer被往上推的呈現方式。

![20240220](https://hackmd.io/_uploads/H1pSf5W36.png)

關於Android、Ios的鍵盤出現後，頁面layout顯示方式的問題討論：
- [Stop pushing the page to top of the screen when opens the keyboard](https://forum.ionicframework.com/t/stop-pushing-the-page-to-top-of-the-screen-when-opens-the-keyboard/183601)
- [Difference between adjustResize and adjustPan in android?](https://stackoverflow.com/questions/17410499/difference-between-adjustresize-and-adjustpan-in-android/17410528#17410528)
- [秒懂Android开发之 android:windowSoftInputMode 属性详解](https://blog.csdn.net/ShuSheng0007/article/details/104232176)
- [Android中windowSoftInputMode的配置与软键盘状态](https://juejin.cn/post/6844903907391455240)
:::

**原生套件範例：操作cookie**

```javascript=
// 設定Cookie
window.Capacitor.Plugins.CapacitorCookies.setCookie({
    url: 'http://example.com',
    key: 'language',
    value: 'en',
    expires: 'Wed, 31 Oct 2012 08:50:17 UTC'
});

//刪除Cookie
window.Capacitor.Plugins.CapacitorCookies.deleteCookie({
    url: 'https://example.com',
    key: 'language',
});

// 刪除某個網站的所有Cookie
window.Capacitor.Plugins.CapacitorCookies.clearCookies({
    url: 'https://example.com',
});
```

**原生套件範例：分享**

```javascript=
window.Capacitor.Plugins.Share.share({
　text: '分享Ａｐｐ給好友',
　url: 'http://ionicframework.com/',
})
```

**原生套件範例：打開FB App**

```javascript=
window.Capacitor.Plugins.AppLauncher.openUrl({ 
 url: 'fb://facewebmodal/f?href=https://www.facebook.com/' 
})
```

:::warning
從Android 11開始，要打開其他App需要在AndroidManifest.xml加上以下設定:
```xml=
<queries>
  <package android:name="對方App的Package Name" />
</queries>
```
:::

**原生套件範例：拍照**

```javascript=
const image = await window.Capacitor.Plugins.Camera.getPhoto({
    quality: 90,
    allowEditing: true,
    resultType: CameraResultType.Uri
})

// image.webPath will contain a path that can be set as an image src.
// You can access the original file using image.path, which can be
// passed to the Filesystem API to read the raw data of the image,
// if desired (or pass resultType: CameraResultType.Base64 to getPhoto)
var imageUrl = image.webPath
```

**原生套件範例：當前位置**

```javascript=
const coordinates = await window.Capacitor.Plugins.Geolocation.getCurrentPosition()
window.Capacitor.Plugins.Dialog.alert({
    title: '訊息',
    message: `lat: ${coordinates.coords.latitude}, 
    lng: ${coordinates.coords.longitude}`,
})
```

**原生套件範例：震動提示**

```javascript=
window.Capacitor.Plugins.Haptics.vibrate()
```

**原生套件範例：鎖定垂直/水平畫面**

```javascript=
window.Capacitor.Plugins.ScreenOrientation.lock({
    orientation: 'landscape'
})
```

**原生套件範例：開啟Splash Screen**

```javascript=
window.Capacitor.Plugins.SplashScreen.show({
    showDuration: 2000,
    autoHide: true,
})
```

**原生套件範例：隱藏/顯示狀態欄**

```javascript=
window.Capacitor.Plugins.StatusBar.hide()
window.Capacitor.Plugins.StatusBar.show()
```

**原生套件範例：下載檔案、讀取檔案**
```javascript=
// url、path、directory為必填
const res = await Capacitor.Plugins.Filesystem.downloadFile({
    url: 'https://mozilla.github.io/pdf.js/web/compressed.tracemonkey-pldi-09.pdf',
    path: 'tracemonkey-pldi-09.pdf',
    directory: 'DOCUMENTS',
})
// console.log(res)
// {
//   path: /storage/emulated/0/Documents/tracemonkey-pldi-09.pdf
// }

// 上面下載後的檔案，可直接以下面方式讀取
// path直接放檔名即可、data使用base64Data較不會出錯
const res = await Capacitor.Plugins.Filesystem.readFile({
    path: 'tracemonkey-pldi-09.pdf',
    directory: 'DOCUMENTS',
    encoding: 'utf8',
    data: base64Data
})
// console.log('file read', res)
// {
//   data: "....."
// }
```

**原生套件範例：儲存檔案**
```javascript=
const status = window.Capacitor.Plugins.Filesystem.requestPermissions()

Dialog.alert({
    title: '訊息',
    message: `請開啟檔案存取權限!: ${status.publicStorage}`,
})

if ( status.publicStorage === 'granted' ) {
  window.Capacitor.Plugins.Filesystem.writeFile({
     directory: Directory.Documents,
     encoding: Encoding.UTF8,
     data: base64Data
  })
} else {
  window.Capacitor.Plugins.Dialog.alert({
    title: '訊息',
    message: '請開啟檔案存取權限!',
  })
}
```

**原生套件範例：讓用戶選擇檔案**
```javascript=
const res = await window.Capacitor.Plugins.FilePicker.pickFiles({ readData: true }).catch(err => {
    console.log('pickFiles error', err)
})
const base64Data = res?.files?.[0]?.data
```

**原生套件範例：顯示檔案**
```javascript=
// 要開啟顯示檔案前，檔案需已存在裝置的檔案系統並取得uri
// 因此這邊需先將base64寫入後才能開啟
const res = Capacitor.Plugins.Filesystem.writeFile({
    path: 'test.pdf',
    data: base64Data,
    directory: 'CACHE',
})
.catch(err => {
    console.log('writeFile error', err)
})

const uri = res?.uri

if(uri){
    Capacitor.Plugins.FileOpener.open({
      filePath: res.uri,
    }).catch(err => {
      console.log('open error', err)
    })
}

```
:::info
- 關於使用Capacitor套件處理檔案的指引：
[The File Handling Guide for Capacitor](https://capawesome.io/blog/the-file-handling-guide-for-capacitor/)
:::
:::danger
檔案操作注意：
- 儲存檔案前先取得狀態確認該路徑下是沒有同名稱的檔案，避免出現錯誤。
- 取得檔案在裝置裡的路徑後，要先使用**Capacitor.convertFileSrc**轉換path才能丟給webview座使用(ex:照片要吃的src)
- Filesystem.writeFile的data欄位使用base64格式比較保險不會出錯
:::
**原生套件範例：開啟瀏覽器**

```javascript=
window.Capacitor.Plugins.Browser.open({ 
    url: 'http://capacitorjs.com/',
    presentationStyle: 'popover'
})
```

**原生套件範例：小提示視窗**

```javascript=
window.Capacitor.Plugins.Toast.show({
    text: '測試Toast!',
    position: 'center'
})
```

**原生套件範例：發送本地推播**

```javascript=
window.Capacitor.Plugins.LocalNotifications.schedule({
    notifications: [
      {
        title: '訊息',
        body: '測試1秒後app本地推播',
        id: 1,
        // 必須要是大於當前時間
        schedule: { at: new Date(Date.now() + 1000) },
        sound: '',
        attachments: [],
        actionTypeId: '',
        extra: '',
      }
    ]
})
```


**原生套件範例：啟用第三方推播**

:::info
- 關於使用Capacitor設定推播的指引：
    - [The Push Notifications Guide for Capacitor](https://capawesome.io/blog/the-push-notifications-guide-for-capacitor/)
- 註冊推播，預設當app在前景時不會跳出來，需至capacitor.config.json加上presentationOptions設定。
請參考[@capacitor/push-notifications文件](https://capacitorjs.com/docs/apis/push-notifications#push-notifications-appearance-in-foreground)
- google-services.json要放在android/app資料夾下
- google-services.json的package_name要跟set-config.yaml的PACKAGE_NAME相同(但不用跟firebase>專案設定>一般設定>套件名稱相同)
- 要提供給後端的文件：firebase>專案設定>服務帳戶>產生新的私密金鑰
- 使用nodejs測試推播發送：[點擊下載js程式](https://drive.google.com/file/d/12wzjYkSHGTAiD1SCH5iMrcLvVXmmQ_KH/view?usp=sharing)
:::

```javascript=
  
  const PushNotifications = window.Capacitor.Plugins.PushNotifications

  // 請求權限
  // iOS 會提示使用者，並且會讓使用者決定是否同意
  // Android 會直接通過且不會提示使用者
  PushNotifications.requestPermissions().then(result => {
    console.log('requestPermissions!')
    if (result.receive === 'granted') {
      // Register with Apple / Google to receive push via APNS/FCM
      PushNotifications.register()
    } else {
      // Show some error
    }
  })
  
  // 成功啟用，可以開始接收推播
  PushNotifications.addListener('registration',
    (token) => {
      window.CapacitorBridge.pushNotificatonToken = token.value
      alert('Push registration success, token: ' + token.value)
    }
  )
  
  // 啟用推播失敗
  PushNotifications.addListener('registrationError',
    (error) => {
      alert('Error on registration: ' + JSON.stringify(error))
    }
  )
  
  // 監聽收到推播
  PushNotifications.addListener('pushNotificationReceived',
    (notification) => {
      alert('Push received: ' + JSON.stringify(notification))
    }
  )
  
  // 監聽推播被點擊
  PushNotifications.addListener('pushNotificationActionPerformed',
    (notification) => {
      alert('Push action performed: ' + JSON.stringify(notification))
    }
  )
```

**==社群套件==**

**Navigation Bar**
:::info
Navigation Bar的操作尚無官方套件。可使用[@hugotomazi/capacitor-navigation-bar](https://github.com/hugotomazi/navigation-bar)、[fagundes/cordova-plugin-navigationbar](https://github.com/capacitor-community/proposals/issues/51#issuecomment-705749035)。詳細討論參考[Navigation Bar](https://github.com/capacitor-community/proposals/issues/51)
:::

**==自訂Capacitor Plugin==**

自訂Android Capacitor套件只需要透過Android Studio新增1隻檔案、MainActivity.java註冊套件即可完成，往後不用再新增，可把相關功能都直接集中在此套件暴露給JS端即可
- [Custom Native Code](https://capacitorjs.com/docs/android/custom-code)

**==App運行期間閃退log蒐集==**

[使用FirebaseCrashlytics ](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%9C%A8-ios-app-%E5%8A%A0%E5%85%A5%E6%94%B6%E9%9B%86%E9%96%83%E9%80%80-log-%E7%9A%84-firebase-crashlytics-b884b9790527)

**==權限說明==**

* [Android權限狀態說明](https://capacitorjs.com/docs/plugins/web#aliases)

**==Android ADB==**

* 使用adb來操作連線的裝置/模擬器
* [使用keytool取得apk的sha256](https://capacitorjs.com/docs/v2/guides/deep-links)
* [adb安裝apk](https://stackoverflow.com/a/7076679)
    * adb -s `裝置名稱` install `Apk路徑`

**==在Mac開發Aandroid App問題==**

* java not found on path and JAVA_HOME not set. Please set JAVA_HOME to the root of your Java installation
    * 參考[Capacitor版本更新的處理](https://hackmd.io/OtvAhiUeThKIzaYsnWSXDQ?both#Capacitor%E7%89%88%E6%9C%AC%E6%9B%B4%E6%96%B0%E7%9A%84%E8%99%95%E7%90%86)區塊嘗試解決

**==常見問題==**

* 不要使用LocalStorage、IndexedDB
    * 假如app空間不夠使用，作業系統會從Web Views拿走儲存空間，可能使得資料消失
    * 小量資料可使用：[@capacitor/preferences](https://capacitorjs.com/docs/apis/preferences)
    * 大量資料可用：[@capacitor-community/sqlite](https://github.com/capacitor-community/sqlite)
* 打包時出現No matching client found for package name錯誤。
    * 如果有使用firebase推播，請檢查google-services.json的package_name要跟set-config.yaml的PACKAGE_NAME相同

# **Capacitor版本更新的處理**

- 建議先檢查@capacitor/android、@capacitor/ios、@capactior/cli都更新到latest版本。
- 更新完後執行npm run ready後再使用android studio(Android)、Xcode(IOS)分別開啟模擬器測試看看


# **上架流程**

**==上架前檢查==**
- [ ] Deep Link設置的host須為以下格式(sports-app.tesla168.xyz)。不含http、https、結尾的/。對應設置為set-config.yaml的WEB_SERVER_HOST
- [ ] 看打包的aab api level是否符合目標API級別規定([最新規定](https://developer.android.com/google/play/requirements/target-sdk?hl=zh-tw))
- [ ] 打包的aab版本號否正確
      檢查目前的原始碼版本號是否高於當前線上版本
- [ ] [選擇App標記](https://support.google.com/googleplay/android-developer/answer/9859673?visit_id=638288050749802347-3193154011&rd=1#zippy=%2C%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F)
    - 標記的目的是介紹應用程式的內容和功能。**標記可能會影響應用程式在 Google Play 上的顯示位置**
    - 最多可選**5**個
- [ ] [設定隱私權政策](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E7%94%A2%E7%94%9F-app-%E4%B8%8A%E6%9E%B6%E9%9C%80%E8%A6%81%E7%9A%84%E9%9A%B1%E7%A7%81%E6%AC%8A%E6%94%BF%E7%AD%96-url-7bc4746cf75d)
- [ ] 應用程式存取權
      [請勿在應用程式啟動時要求所有權限。只在他們需要存取特定功能時才要求權限。](https://developer.android.com/training/permissions/usage-notes?hl=zh-tw)
- [ ] 準備對應圖檔素材/App文案
    - **應用程式名稱**
      - 最多30字元
      - 輸入的文字必須是純文字，無法設置樣式(如顏色、字型)
      - AppStore 名稱不可重複
          應用程式名稱在 Google Play 是允許重複的，但是 AppStore 是不允許重複的。因此 Google Play 應用程式名稱可能需要與 AppStore 同時考慮
      - 下載後可以有不同名稱
          應用程式名稱，是顯示在 Google Play 上的名稱，但是下載下來後，APP 可以有不同的名稱。且 APP 名稱過長時，會被截掉出現 ... 結尾。
    - 簡短說明
      - 最多80字元
      - 輸入的文字必須是純文字，無法設置樣式(如顏色、字型)
      - 建議寫一句簡短介紹或標語(slogan)
      - 在「應用程式」頁面，只會顯示「簡短說明」。
      - 在「關於應用程式」頁面，會同時顯示「簡短說明」與「完整說明」。
      - 建議**完整說明的第一句不要把「簡短說明」複製過去**，否則會重複顯示。
    - 完整說明
      - 最多4000字元
      - 輸入的連結文字，在手機檢視 Google Play 時，會變成可以點擊的連結
      - 可支援部分HTML標記，如下圖。[完整說明可支援的格式說明](https://stackoverflow.com/questions/11071127/google-play-app-description-formatting)[](![](https://hackmd.io/_uploads/BklASFYT2.png)
)
        ![](https://hackmd.io/_uploads/HJmrrYY6n.png)
    - **應用程式圖示**
        - 一般來說就是將 App icon 也上傳上去
        - (PNG 或 JPEG 檔案，大小不得超過 1 MB，尺寸應為 512 x 512 px)
    - **主題圖片** [官方說明](https://support.google.com/googleplay/android-developer/answer/9866151?hl=zh-Hant#zippy=%2C%E4%B8%BB%E9%A1%8C%E5%9C%96%E7%89%87)
        - 一般情形下不會顯示，當滿足特定條件時，Google Play 才會顯示主題圖片
        - PNG 或 JPEG 檔案，大小不得超過 15 MB，尺寸應為 1,024 x 500 px
    - **手機螢幕截圖** [使用Android Studio 模擬器可截圖](https://developer.android.com/studio/debug/am-screenshot?hl=zh-tw)
        - PNG 或 JPEG 檔案，2 到 8 張手機螢幕截圖
        - [作法1: 將APP截圖上傳](https://play.google.com/store/apps/details?id=com.teslamotors.tesla&hl=zh_TW&gl=US)
        - [作法2: 畫面中有手機，另外也有其他提示文字](https://play.google.com/store/apps/details?id=com.google.android.apps.photos&hl=en&gl=US)

      
**==其他設定準備==**

- [ ] [本次發佈的版本詳細資訊內容](https://support.google.com/googleplay/android-developer/answer/9859348?hl=zh-HK&ref_topic=7072031&sjid=8500401862475328496-AP#zippy=%2C%E6%87%89%E7%94%A8%E7%A8%8B%E5%BC%8F%E5%AE%8C%E6%95%B4%E6%80%A7%2Capp-bundle%2C%E7%99%BC%E4%BD%88%E7%89%88%E6%9C%AC%E5%90%8D%E7%A8%B1%2C%E9%80%99%E5%80%8B%E7%99%BC%E4%BD%88%E7%89%88%E6%9C%AC%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B0%E5%8A%9F%E8%83%BD)

**==Google Play商店頁語系==**

- 在第一次建立完「主要商店資訊」後，Google有提供「自訂商店資訊」，可以用來針對不同語系/國家個別設定要呈現的文字
- Googple Play Console > 商店發佈 > 自訂商店資訊 > 網路論壇 > 選擇主要商店資訊群組 > 點選「在這個群組中建立商店資訊」。即可開始輸入另一個語系的資料

**==上架前測試==**

- 內部測試：使用play console選擇內部測試，依序照步驟設定，可獲取下載連結發送給要測試的人員
- 正式發布前測試報告：檢查是否有事項需進行調整

**==正式發佈==**

- 使用play console選擇正式版，依序照步驟設定後送審等待結果。
- 審查作業通常會在 7 天內完成，但也可能需要更長的時間

**==上架後更新==**

- [提示使用者更新至最新版應用程式](https://support.google.com/googleplay/android-developer/answer/13812041?hl=zh-Hant)

**==常見問題==**

- [沒有與這個App Bundle相關聯的去模糊化檔案](https://developer.android.com/build/shrink-code#decoding)
  根據官方文件，打開gradle設定的minifyEnabled、shrinkResources
- **版本代碼 XX 已經使用過了，請使用其他版本代碼：**
  應用程式套件探索工具>右上角"刪除app bundle" 刪除原本的版本後才能上傳新版本(因為版本號相同衝突了)
  :::info
  要先在發布/測試頁把裡面的app bundle刪除並儲存，然後”應用程式套件探索工具”頁的刪除按鈕才會出現
  :::
- **您的應用程式指定 Android 13 (API 級別 33) 以上版本為目標。您必須在 Play 管理中心聲明使用廣告 ID：**
  根據提示前往填寫設定即可完成
- [有使用到第三方平台導致App下載後於使用者端出現功能無法正常使用](https://vocus.cc/article/62e1204dfd897800017acdff)
  這方面的問題由於牽涉到不同平台與設定，需特別留意
- [Api Level需要更新的官方說明](https://android-developers.googleblog.com/2022/04/expanding-plays-target-level-api-requirements-to-strengthen-user-security.html)
  
:::warning
Google Play商店的描述文案與App的名稱、Icon需特別注意不要有誤導性內容。
請參考Google文件[中繼資料](https://support.google.com/googleplay/android-developer/answer/9898842#zippy=%2C%E5%B8%B8%E8%A6%8B%E7%9A%84%E9%81%95%E8%A6%8F%E8%A1%8C%E7%82%BA%E7%A4%BA%E4%BE%8B)
:::
  
# **Goole Play 商店**
- 多語系
    - [Google Play 商店多語系說明](https://support.google.com/googleplay/android-developer/answer/9844778?hl=zh-Hant#zippy=%2C%E6%96%B0%E5%A2%9E%E6%82%A8%E8%87%AA%E8%A1%8C%E6%8F%90%E4%BE%9B%E7%9A%84%E6%96%87%E5%AD%97%E7%BF%BB%E8%AD%AF%E8%88%87%E6%9C%AC%E5%9C%B0%E5%8C%96%E5%9C%96%E5%83%8F%E8%B3%87%E6%BA%90%2C%E4%BD%BF%E7%94%A8-play-%E7%AE%A1%E7%90%86%E4%B8%AD%E5%BF%83%E7%9A%84%E5%85%8D%E8%B2%BB%E6%A9%9F%E5%99%A8%E7%BF%BB%E8%AD%AF%E6%9C%8D%E5%8B%99%2C%E5%A6%82%E6%9E%9C%E6%82%A8%E4%B8%8D%E6%83%B3%E6%96%B0%E5%A2%9E%E7%BF%BB%E8%AD%AF%E7%89%88%E6%9C%AC%E6%88%96%E8%B3%BC%E8%B2%B7%E7%BF%BB%E8%AD%AF%E6%9C%8D%E5%8B%99)
    - 新增不同語系：拓展>主要商店資訊>右方語言下拉>選取語言>新增
- App介紹詳細頁：
    - 手機google play app版：
        - 會在「關於應用程式」標題下方顯示「簡短說明」
        - 會在「關於應用程式內頁」內分別有上至下顯示「簡短說明」、「完整說明」
    - 網頁版：
        - 會在「關於應用程式」標題下方顯示「簡短說明」
        - 在「關於應用程式內頁」內只顯示「完整說明 」
- 簡短說明：
- 完整說明：如果開發者未提供使用者使用的對應語系，則會自動翻譯成使用者所在地區的語言
    - 在play console的對應設定為：主要商店資訊>完整說明
- 關於此應用程式：顯示此版本的相關訊息，可依照不同語系做新增

:::warning
上架後的App在Google Play搜尋不到的問題：
- [不是熱門App可能會遇到這奘狀況(有時甚至不會出現在搜尋結果)](https://www.quora.com/How-can-I-fix-it-if-apps-are-not-showing-up-on-the-Google-Play-Store)
- [如果App是剛推出的，可能需要等待幾天的時間(之後通常用關鍵字就能搜尋到App)](https://support.google.com/googleplay/thread/15162444?hl=en&msgid=104038345)

解決方式:
- 可以直接使用「套件名稱」進行搜尋
:::

# **資源**
* [capacitor官方套件庫](https://capacitorjs.com/docs/apis)
* [awesome capacitor資源整理](https://github.com/riderx/awesome-capacitor)
* [capawesome capacitor第三方套件庫](https://capawesome.io/)
* [app icon](https://icons8.com/icon/set/food/color)
* [免費splash screen](https://unsplash.com/s/photos/splash-screen?license=free)
* [隱私權政策產生器](https://app-privacy-policy-generator.firebaseapp.com/#)
* [使用google工具產生網站關聯檔](https://developers.google.com/digital-asset-links/tools/generator?hl=zh-cn)

# **名詞**

* Device Id
    * [IOS取得裝置唯一值有3種方式](https://medium.com/appworks-school/device-check-35556ab74d)。而capacitor/device套件使用的是[identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)
    * Android使用的是結合signing key、user、device來得到的key
* 網站關聯檔：讓應用程式或網站公開對於其他應用程式或網站公開的可驗證聲明
* ProGuard：為Android提供編譯代碼加密
* Fastlane：google支持的一款app自動化發佈工具
* 應用程式套件探索工具：版本記錄器
* Android API Level(sdk version)
    * [Android api level對應 android 版本](https://developer.android.com/guide/topics/manifest/uses-sdk-element?hl=zh-cn)
* [自訂商店資訊](https://support.google.com/googleplay/android-developer/answer/9867158?visit_id=638428748705748768-670212887&rd=1#zippy=%2C%E6%9F%A5%E7%9C%8B%E8%87%AA%E8%A8%82%E5%95%86%E5%BA%97%E8%B3%87%E8%A8%8A%E7%9A%84%E4%BD%BF%E7%94%A8%E7%AF%84%E4%BE%8B)
* cloudinary

# **參考**

* [使用vite打包capacitor網頁端用的 js bridge library](https://andrewwalpole.com/blog/use-vite-for-javascript-libraries/)
* [webview中存取capacitor api](https://github.com/ionic-team/capacitor/discussions/4150)
* [webview中存取capacitor api的capacitor.config.json需要做的設定](https://github.com/ionic-team/capacitor/discussions/4080)
* [capacitor專案中操作webview的範例1](https://stackoverflow.com/questions/68216690/capacitorjs-app-with-android-webview-disable-zoom)
* [capacitor專案中操作webview的範例2](https://github.com/ionic-team/capacitor/issues/2645)
* [app中跳轉至另一個app](https://medium.com/@moriliu/%E5%9C%A8ios-app%E4%B8%AD%E9%80%8F%E9%81%8Eurl-scheme%E8%B7%B3%E8%BD%89fb-app%E5%92%8Cmessenger%E6%8C%87%E5%AE%9A%E8%81%AF%E7%B5%A1%E4%BA%BA-ab1fea2129c6)
* [設定app status bar的方式](https://www.geeksforgeeks.org/how-to-change-the-color-of-status-bar-in-an-android-app/)
* [capacitor官方說明正確使用方式為把資源打包進app，如果要載入外部網站，則offline的部分需自行處理](https://github.com/ionic-team/capacitor/issues/2899)
* [google play 免安裝體驗概覽](https://developer.android.com/topic/google-play-instant/overview?hl=zh-cn)
* [IOS開發環境設置須注意Xcode版本要求](https://capacitorjs.com/docs/getting-started/environment-setup#ios-development)
* [android上架官方指引](https://support.google.com/googleplay/android-developer/answer/9859152?hl=zh-HK)
* [符合 Google Play 的目標 API 級別規定](https://developer.android.com/google/play/requirements/target-sdk?hl=zh-tw)
* [產生上架所需的icon、splash screen](https://capacitorjs.com/docs/guides/splash-screens-and-icons)
* [google play 上架 費用？開發者必看的完整指南！](https://www.pintech.com.tw/blog_list/234/google-play-deployment-cost-guide)
* [最新Google应用上架流程！Googleplay 上架流程(2023版)](https://juejin.cn/post/7237320435632488504)
* [app上架圖全攻略](https://medium.com/goons/%E4%B8%8A%E6%9E%B6%E5%9C%96%E5%85%A8%E6%94%BB%E7%95%A5-%E6%80%A6%E7%84%B6%E5%BF%83%E5%8B%95%E7%9A%84%E4%B8%80%E7%9C%BC%E7%9E%AC%E9%96%93-ce1ed46908a8#25eb)
* [Google Play上架/更新被拒的原因及解決方法匯總](https://seoasoorm.com/zh-tw/google-play-store-rejection/)
* [google play送審前檢查清單](https://play.google.com/console/about/guides/releasewithconfidence/)
* [Android應用程式權限最佳做法](https://developer.android.com/training/permissions/usage-notes?hl=zh-tw)
* [Android 發布至 Google play 填寫資訊教學](https://www.wanwaninfo.com/blog/android-release-fill-information)
* [How to listen for a WebView finishing loading a URL?](https://stackoverflow.com/questions/3149216/how-to-listen-for-a-webview-finishing-loading-a-url)
* [Android App Bundle](https://juejin.cn/post/7197246543207022629)
* [如何讓您的Android App上架成功？
LA](https://www.pintech.com.tw/blog_list/190/android-app-publishing-success)
* [APP上線全記錄 - 4種APP上線會遇到的問題](https://vocus.cc/article/62e1204dfd897800017acdff)
* [APP誕生全紀錄](https://vocus.cc/article/62c52081fd897800014e8b56)
* [Storage quotas and eviction criteria](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria)
* [Capacitor monorepo](https://forum.ionicframework.com/t/using-capacitor-with-nx-monorepo/191003)
