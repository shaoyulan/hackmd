源

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