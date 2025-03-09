---
title: Github-React Native說明

---

## 資源

- [Interactive playground](https://snack.expo.dev/)
- [App showcase](https://reactnative.dev/showcase)
- [RN cli commands](https://github.com/react-native-community/cli/blob/main/docs/commands.md)
- [檢查react打包的bundle](https://github.com/segmentio/analytics-react-native#adding-plugins)
```
"https://github.com/IjzerenHein/react-native-bundle-visualizer.git#fix-remove-minify-false" --platform android
```

## 資料夾結構

- [Best Folder Structure for React Native Project](https://www.google.com/url?q=https://learn.habilelabs.io/best-folder-structure-for-react-native-project-a46405bdba7&sa=D&source=docs&ust=1741533604841179&usg=AOvVaw1iPvgXcoXc5R7hfrwVrWy0)
- [React Native Project Structure: A Best Practices Guide](https://www.waldo.com/blog/react-native-project-structure)
- [Ultimate Folder Structure F](https://dev.to/rushitjivani/ultimate-folder-structure-for-your-react-native-project-1k27)

## RN打包比較

**使用Expo**
- 使用Expo可以加速開發流程
- 使用Expo完全不需要會原生程式，使用純RN多少會需要碰到一點點
- 有了Expo後，開發上完全不需要Android Studio、Xcode
- 使用Expo可以完全不需要使用App Store來辦到熱更新(OTA Update)

**不使用Expo**
- 保留最多控制
- 要支援Web需要自行設定支援

## Android開發

- Android 13 (Tiramisu) SDK
- [設定系統 ANDROID_HOME 環境變數](https://reactnative.dev/docs/environment-setup?guide=native)，新增platform-tools至系統Path環境變數
- npx react-native@latest init [專案名稱]
:::info
或使用third-party CLI 例如Ignite CLI、create-expo-app
:::

## IOS開發

**環境需求**

- Xcode(and iOS Simulator)
- Ruby
- CocoaPods
- Testflight
- Apple Developer App
- homebrew
```
// 查看版本
brew -v

// 用于檢查 HomeBrew 的還境是否已經就緒
brew doctor

// 檢查套件版本
brew list --versions 套件名稱

// 檢查套件線上最新資料
brew info 套件名稱
```
- watchman
用於偵測檔案變動
```
// 版本
watchman -v
```
- rbenv
ruby版本管理工具
```
// 版本
ruby - -version
```

**打包問題排除**

- [The linked framework 'Pods_Runner.framework' is missing one or more architectures required by this target: x86_64](https://stackoverflow.com/questions/70447462/the-linked-framework-pods-runner-framework-is-missing-one-or-more-architecture/72750060#72750060)
- [Updated to Xcode 13, get unique error code when trying to run SwiftUI app on iOS but not macOS](https://stackoverflow.com/questions/69350634/updated-to-xcode-13-get-unique-error-code-when-trying-to-run-swiftui-app-on-ios)

**TestFlight**
- [使用 TestFlight 邀請 internal tester 測試 App](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E4%BD%BF%E7%94%A8-testflight-%E9%82%80%E8%AB%8B-internal-tester-%E6%B8%AC%E8%A9%A6-app-7c762aa1cb82)

## APP功能

**推播**

- [react-native-onesignal](https://github.com/OneSignal/react-native-onesignal)
經測試可正常收到第三方推播
- expo推播
經測試可正常收到第三方推播

**App自身訊息推播**

- [notifee](https://notifee.app/)
經測試app本身可設定推播訊息

**保持app背景service運作**

- [expo-background-fetch](https://docs.expo.dev/versions/latest/sdk/background-fetch/)
- react-native-background-timer

**Deep Link(Android)/Universal Link(IOS)**

- [測試Deep link](https://halgatewood.com/deeplink/)
- Android
    - Deep links
        - 將使用者直接導向app
        - 如果沒安裝app，預設沒有fallback，需自行處理fallback邏輯
        - 只需要在AndroidManifest.xml加上intent-filter即可啟用
        - 打開deep link會需要使用a href或function 去觸發，不能直接用瀏覽器輸入網址
    - Web links
        - 一樣是deep link但使用http或https
        - 內容都會直接使用瀏覽器瀏覽
    - Android App Links
        - 可當作是deep links的進化版
        - Web link且也是使用http或https
        - 會將app打開、app沒安裝則會打開該網
        - 要啟用會需要驗證該網址是我方擁有的(需上傳一個json file到網站上)
- IOS
    - Custom URL Schemes
    - Universal Links

**輪播**
- [react-native-swiper-flatlist](https://github.com/gusgard/react-native-swiper-flatlist)

**Tab**
- [React Native Tab View](https://reactnavigation.org/docs/tab-view/)

**Action Sheet**
- [react-native-action-sheet](https://github.com/expo/react-native-action-sheet)

**Icon**
- [React Native Vector Icons](https://github.com/oblador/react-native-vector-icons#icon-component)

**SVG**
- [react-native-svg](https://github.com/software-mansion/react-native-svg)
- [react-native-svg-transformer](https://github.com/kristerkari/react-native-svg-transformer)

****

## 發布更新-使用Microsoft/react-native-codepush

**建置步驟**

- [CodePush](https://learn.microsoft.com/en-us/appcenter/distribution/codepush/)

**官方使用範例**

- [App Center Sample App for React Native](https://github.com/microsoft/appcenter-sampleapp-react-native)

**費用**

- [無限app數量、無限推送更新次數、無限codepush使用](https://learn.microsoft.com/zh-tw/appcenter/general/pricing)
- [所有人都可以使用 Visual Studio App Center](https://visualstudio.microsoft.com/zh-hant/app-center/pricing/)

**工作流程**

- [更新發佈步驟](https://learn.microsoft.com/en-us/appcenter/distribution/codepush/cli#releasing-updates-react-native)
- [Multi-Deployment Testing](https://github.com/microsoft/react-native-code-push#multi-deployment-testing)

**App Center Cli文件**

- [Releasing CodePush updates using the App Center CLI](https://learn.microsoft.com/en-us/appcenter/distribution/codepush/cli)

**常用指令**

```
// 列出所有app
appcenter apps list

// 查看某個app的所有deployment list
ppcenter codepush deployment list -a <ownerName>/<appName>

// 查看app某個deployment更新紀錄
appcenter codepush deployment history -a <ownerName>/<appName> <deploymentName>


//發布react-native app deployment
appcenter codepush release-react -a <ownerName>/<appName> -d <deploymentName> -t <targetBinaryVersion>

// 修改已經relase出去的更新的metadata
appcenter codepush patch -a <ownerName>/<appName> <deploymentName> <existing-release-label>

// 取消某次更新推送(實際上是再發布一版前個版本的更新)
appcenter codepush rollback -a  <ownerName>/<appName> <deploymentName>
```

**功能簡介**

- Email通知功能
推送更新後、編譯後(成功或失敗)、當機事件
- Webhook支援
推送更新後、編譯後(成功或失敗)、當機事件
- 線上後台錯誤回報列表
Diagnostics/Issues，可用於查看當機/錯誤報告
- [分析功能](https://learn.microsoft.com/zh-tw/appcenter/analytics/)
    - 主要用於了解應用程式使用者和使用應用程式時的行為
    - Analytics裡面的分析包含任何安裝本app的裝置(不管是否有安裝過推送的更新)

**Codepush常見問題**

- [Frequently Asked Questions](https://microsoft.github.io/code-push/faq/index.html)

**Codepush注意事項**

- 測試更新不能使用debug mode
> CodePush updates should be tested in modes other than Debug mode. In Debug mode, React Native app always downloads JS bundle generated by packager, so JS bundle downloaded by CodePush does not apply

- 預設codepus會在app每次starup時檢查更新
> By default, CodePush will check for updates on every app start
(如果更新是madatory，該更新會立即被安裝)

- 官方建議推送更新流程
> We recommend that all users take advantage of the automatically created Staging and Production environments, and do all releases directly to Staging, and then promote from Staging to Production after the appropriate testing

- 推送上去的更新無法刪除，有出錯必須使用rollback
- 不要跟In App Update搞混，In App Update是Android的另一種更新機制
- 不建議在app原始碼(appcenter-config.json)放置app secret
- [App可增加設置user id，用於錯誤回報時可得知哪位使用者](https://learn.microsoft.com/en-us/appcenter/sdk/other-apis/react-native)
- 要同時支援ios、android，官方建議要建立分開的codepush application
> Note, if you are targeting both platforms it is recommended to create separate CodePush applications for each platform


**限制**

- [codepush有支援的RN版本限制，須按裝對應版本](https://github.com/microsoft/react-native-code-push/tree/master#supported-react-native-platforms)
    - [請直接看relase選支援的版本](https://github.com/microsoft/react-native-code-push/releases)
```
// 安裝指定版本
npx react-native@0.71.12 init npx_react_native2 --version 0.71.12
```

## 免安裝測試App

- [Google Play Instant](https://developer.android.com/topic/google-play-instant?hl=zh-tw)

## Debug

- [DevTool](https://reactnative.dev/docs/react-native-devtools)

## 問題排除

- 出現..Error: EPERM: operation not permitted...
    - [React Native - Constant errors like "Error: EPERM: operation not permitted, lstat ..."](https://stackoverflow.com/questions/63508278/react-native-constant-errors-like-error-eperm-operation-not-permitted-lsta/72596995#72596995)
    - [Error: EPERM: operation not permitted...](https://github.com/oblador/react-native-vector-icons/issues/354#issuecomment-1139691331)源

- [Interactive playground](https://snack.expo.dev/)
- [App showcase](https://reactnative.dev/showcase)
- [RN cli commands](https://github.com/react-native-community/cli/blob/main/docs/commands.md)
- [檢查react打包的bundle](https://github.com/segmentio/analytics-react-native#adding-plugins)
```
"https://github.com/IjzerenHein/react-native-bundle-visualizer.git#fix-remove-minify-false" --platform android
```

## RN打包比較

**使用Expo**
- 使用Expo可以加速開發流程
- 使用Expo完全不需要會原生程式，使用純RN多少會需要碰到一點點
- 有了Expo後，開發上完全不需要Android Studio、Xcode
- 使用Expo可以完全不需要使用App Store來辦到熱更新(OTA Update)

**不使用Expo**
- 保留最多控制
- 要支援Web需要自行設定支援

## Android開發

- Android 13 (Tiramisu) SDK
- [設定系統 ANDROID_HOME 環境變數](https://reactnative.dev/docs/environment-setup?guide=native)