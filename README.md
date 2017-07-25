## **先cd到项目的根目录**
# 第一步：归档生成.xcarchive
> ### 方法一：使用自动签名功能进行归档 
`xcodebuild -project  <XXX.xcodeproj所在路径>  -archivePath  <XXX.xcarchive所在路径>  -scheme <Scheme名字>  -configuration  <Debug或Release>  -sdk iphoneos archive DEVELOPMENT_TEAM="<TeamID>"`

_-project   指向xcode工程文件的路径_ <br>
_-archivePath  导出的归档的文件路径_ <br>
_-scheme  项目的scheme名字_ <br>
_-configuration  通常设置Debug或者Release_ <br>
_-DEVELOPMENT_TEAM  苹果开发者团队TEAMID_ 
<br><br>

> ### 方法二：使用手动添加证书、描述文件进行归档
`xcodebuild -project <XXX.xcodeproj所在路径>  -archivePath  <XXX.xcarchive所在路径> -scheme <Scheme名字> -configuration <Debug或Release> -sdk iphoneos archive DEVELOPMENT_TEAM="<TeamID>" CODE_SIGN_IDENTITY="<证书名字>" PROVISIONING_PROFILE_SPECIFIER="<描述文件名字>"`

_-project   指向xcode工程文件的路径_ <br>
_-archivePath  导出的归档的文件路径（包含文件夹和文件名）_ <br>
_-scheme  项目的scheme名字（xcode -> product -> scheme）_ <br>
_-configuration  通常设置Debug或者Release_ <br>
_-DEVELOPMENT_TEAM  苹果开发者团队TEAMID(在苹果开发者网站上查看teamid)_ <br>
_-CODE_SIGN_IDENTITY  证书名字（一定要去钥匙串里复制粘贴正确的证书名，注意包含前缀如iPhone Distribution: ,iPhone Developer:，全名如：iPhone Distribution: Jiangsu Qianmi Network Technology Co., Ltd.）_<br>
_-PROVISIONING_PROFILE_SPECIFIER  描述文件的名字（苹果开发者网站，Provisioning Profiles对应的名称）_

# 第二步：通过.xcarchive生成.ipa
`xcodebuild -exportArchive -archivePath <XXX.xcarchive所在路径> -exportPath <导出.ipa的路径>  -exportOptionsPlist <XXX.plist所在路径>`

_-archivePath  导出的归档的文件路径_ <br>
_-exportPath  想要导出.ipa文件的路径_ <br>
_-exportOptionsPlist  plist文件所在的位置（不是info.plist，这是xcode8.3后不支持-exportFormat命令后，改成用plist文件来支持了）_ <br>

> ##### 测试环境plist文件
`<?xml version="1.0" encoding="UTF-8"?> ` <br>
`<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">`<br>
`<plist version="1.0">`<br>
   &nbsp;&nbsp;&nbsp;&nbsp;`<dict>`<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<key>method</key>`<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<string>development<string>`<br>
   &nbsp;&nbsp;&nbsp;&nbsp;`</dict>`<br>
`</plist>` 

> ##### 正式环境plist文件
`<?xml version="1.0" encoding="UTF-8"?> ` <br>
`<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">`<br>
`<plist version="1.0">`<br>
   &nbsp;&nbsp;&nbsp;&nbsp;`<dict>`<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<key>method</key>`<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<string>app-store<string>`<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<key>teamID</key>`<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`<string>你的苹果开发者团队ID<string>`<br>
   &nbsp;&nbsp;&nbsp;&nbsp;`</dict>`<br>
`</plist>` 
<br><br>
[参考文章看这里](http://123.57.28.121/index.php/2016/10/28/code-sign-in-xcode8/)


#####examble########

生成归档文件  xxx.xcarchive
xcodebuild -project /Users/yclxiao/Project/elifehome/ios/elifehome.xcodeproj -archivePath /Users/yclxiao/Project/elifehome/ios/elifehome.xcarchive -scheme elifehome -configuration Release -sdk iphoneos archive DEVELOPMENT_TEAM="SFSFH6NHYB" CODE_SIGN_IDENTITY="iPhone Distribution: Jiangsu Qianmi Network Technology Co., Ltd." PROVISIONING_PROFILE_SPECIFIER=“ehome"



生成ipa文件   xxx.ipa
xcodebuild -exportArchive -archivePath /Users/yclxiao/Project/elifehome/ios/elifehome.xcarchive -exportPath /Users/yclxiao/Project/elifehome/ios/elifehome.ipa -exportOptionsPlist /Users/yclxiao/Project/elifehome/ios/export.plist

