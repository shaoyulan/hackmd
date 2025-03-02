## 環境
- 編輯器: Vscode
- 版控: Sourcetree/git
- 框架: Nuxt、Vue(Vite)
- 本地伺服器: Nginx
- 套件管理: npm
- Node管理: [nvm-windows](https://github.com/coreybutler/nvm-windows/releases)
- 樣式: SCSS
- Chrome DevTool
    - Vue
    - Page Ruler
    - Cocos: Cocos Devtool
- Chrome Extention
    - Proxy: Omega
    - VPN: Surfsark

## 規範

### 共用資料夾架構

- 開發目錄(根目錄或src目錄)
    - .env檔案
      框架基本上都預設依不同mode(develop、release)加載不同.env。
    - config資料夾
      存放專案config，會依不同mode回傳對應config。
      ```typescript=
      // 使用:
      import config from '@/config'
      const baseUrl = config.appBaseURL
      ```
    - assets資料夾
        - images資料夾
          依用途/元件來管理資料夾，例如:
          - logo資料夾
          - header資料夾
          - pages資料夾(存放不同頁面的圖檔)

### Vue元件規範

### GIT

### Vue(Vite)

### Nuxt
