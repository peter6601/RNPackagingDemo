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


## 安裝react & react native
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
## 安裝 cocoapods 
```
$ sudo gem install cocoapods
```
## 自己專案建立Podfile
```
```

## 在Podfile加入要安裝的套件
主是要為RN的套件，官方指示需加入的Code
```

  pod 'React', :path => 'node_modules/react-native', :subspecs => [
    'Core',
    'CxxBridge', # Include this for RN >= 0.47
    'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
    'RCTText',
    'RCTNetwork',
    'RCTWebSocket', # Needed for debugging
    'RCTAnimation', # Needed for FlatList and animations running on native UI thread
    # Add any other subspecs you want to use in your project
  ]
  # Explicitly include Yoga if you are using RN >= 0.42.0
  pod 'yoga', :path => 'node_modules/react-native/ReactCommon/yoga'
  # Third party deps podspec link
  pod 'DoubleConversion', :podspec => 'node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
  pod 'glog', :podspec => 'node_modules/react-native/third-party-podspecs/glog.podspec'
  pod 'Folly', :podspec => 'node_modules/react-native/third-party-podspecs/Folly.podspec'
```

## 還需加入issue修正的內容在Podfile

以下官方尚未提到，但在build project會有error
要fix issue需加入以下的code
```

def change_lines_in_file(file_path, &change)
    print "Fixing #{file_path}...\n"
    
    contents = []
    
    file = File.open(file_path, 'r')
    file.each_line do | line |
        contents << line
    end
    file.close
    
    File.open(file_path, 'w') do |f|
        f.puts(change.call(contents))
    end
end

post_install do |installer|
    # https://github.com/facebook/yoga/issues/711#issuecomment-381098373
    change_lines_in_file('./Pods/Target Support Files/yoga/yoga-umbrella.h') do |lines|
        lines.reject do | line |
            [
            '#import "Utils.h"',
            '#import "YGLayout.h"',
            '#import "YGNode.h"',
            '#import "YGNodePrint.h"',
            '#import "YGStyle.h"',
            '#import "Yoga-internal.h"',
            ].include?(line.strip)
        end
    end
    
    # https://github.com/facebook/yoga/issues/711#issuecomment-374605785
    change_lines_in_file('./node_modules/react-native/React/Base/Surface/SurfaceHostingView/RCTSurfaceSizeMeasureMode.h') do |lines|
        unless lines[27].include?("#ifdef __cplusplus")
            lines.insert(27, "#ifdef __cplusplus")
            lines.insert(34, "#endif")
        end
        lines
    end
    
    # https://github.com/facebook/react-native/issues/13198
    change_lines_in_file('./node_modules/react-native/Libraries/NativeAnimation/RCTNativeAnimatedNodesManager.h') do |lines|
        lines.map { |line| line.include?("#import <RCTAnimation/RCTValueAnimatedNode.h>") ? '#import "RCTValueAnimatedNode.h"' : line }
    end
    
    # https://github.com/facebook/react-native/issues/16039
    change_lines_in_file('./node_modules/react-native/Libraries/WebSocket/RCTReconnectingWebSocket.m') do |lines|
        lines.map { |line| line.include?("#import <fishhook/fishhook.h>") ? '#import "fishhook.h"' : line }
    end
end
```
## 安裝套件
```
Pod install
```

## 在需要地方寫串接method
   ```
  let jsCodeLocation = Bundle.main.url(forResource: "wizard_mobile", withExtension: "bundle")
  let rootView = RCTRootView(bundleURL: jsCodeLocation, moduleName: "WizardMobile", initialProperties: nil, launchOptions: nil)
  self.view.addSubview(rootView!)
   ```
## 建立file檔(需同名）
  - 
