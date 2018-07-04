# RNPackagingDemo

參考文件：https://facebook.github.io/react-native/docs/getting-started
（不過很多錯）

## 安裝套件管理
  - 安裝brew
  - https://brew.sh/index_zh-tw


## 用brew安裝node
    brew install node


## 安裝npm
  - 在root底下放入  package.json
  - 裡面放入以下內容
  - 可用vi  package.json  建立
  - 
  ```
    {
      "name": "MyReactNativeApp",
      "version": "0.0.1",
      "private": true,
      "scripts": {
        "start": "node node_modules/react-native/local-cli/cli.js start"
      }
    }
   ```


## 安裝react & react native##
  - 安裝npm,  react 和 react native 
    npm install
    npm install react react-native
  
## 用SDK project產生jsbundle
  - 去GitHub 下載project
  - 在此project去下指令
  ```
    react-native bundle --platform ios--dev false--entry-file wizard_mobile.js--bundle-output ios/wizard_mobile.bundle--
   assets-dest ios/
  ```
  - 
  
## 在需要地方寫串接method
   ```
  let jsCodeLocation = Bundle.main.url(forResource: "wizard_mobile", withExtension: "bundle")
  let rootView = RCTRootView(bundleURL: jsCodeLocation, moduleName: "WizardMobile", initialProperties: nil, launchOptions: nil)
  self.view.addSubview(rootView!)
   ```
## 建立file檔(需同名）
  - 
