---
title: github-hackmd-串流、多媒體播放

---


## HLS

iOS < 17.1 的 Safari on iPhone不支援MediaSource API但具備 HLS support

- https://github.com/video-dev/hls.js/issues/4354
- https://github.com/video-dev/hls.js/pull/5542
- [可升級到IOS17的裝置清單](https://support.apple.com/en-is/guide/iphone/iphe3fa5df43/ios)

> 注意:
> 當瀏覽器不支援MediaSource 且原生沒支援HLS，則該瀏覽器無法播放hls
> When a platform has neither MediaSource nor native HLS support, the browser cannot play HLS.

ios17.1新增的managedMediaSource終於可以讓Hls.js運作:
- [但需要手動開啟該功能](https://github.com/video-dev/hls.js/pull/5542#issuecomment-1603370874)
- [iOS 17 beta 5 有一個bug讓HLS.js會誤判，而判斷不支援](https://github.com/video-dev/hls.js/pull/5542#issuecomment-1603370874)

> 注意
> [使用Xcode模擬器即便IOS > 17.1，仍會有HLS.isSupported()返回不支援的狀況(apple的bug)，請直接在真實裝置測試](https://github.com/video-dev/hls.js/issues/6161)
> 實機實測(IOS >= 17.1) : HLS.isSupported() 為true
> 模擬機實測(IOS >= 17.1) : HLS.isSupported() 為falseHLS

