# sign-ipa
怎样签名ipa
由于ios的企业证书有期限，到期限后就会导致App不能使用，故需要重新进行签名，先整理一份自己在mac上手动替换签名的方法和步骤。
原理：
IOS安装包企业签名ipa包里面包含的是payload文件夹，文件夹中包含了资源文件和_CodeSignature签名文件夹以及embedded.mobileprovision证书配置文件，而其中关于ios授权签名的就是_CodeSignature签名文件夹和embedded.mobileprovision证书配置文件 只要替换这两个文件就搞定。
_CodeSignature签名文件夹 需要用 *.plist 授权文件去自动生成。
*.plist文件配置如下：
#signature.plist
其中 ${application-identifier} 和 ${com.apple.developer.team-identifier} 在 企业证书文件中 *.mobileprovision 中拷贝
1.    **实施步骤：**
* 你的有原始的ios安装包，*.ipa
* 你得有在有效期内的的企业证书，如下文件：
    * *.mobileprovision
    * distribution.cer
    * distribution.p12
* 将cer证书输入密码安装在当前的mac机器上
* 命名*.mobileprovision 为embedded.mobileprovision
* 解压ipa包
    * unzip *.ipa
* 
* 删除_CodeSignature签名文件夹
    * rm -rf Payload/*.app/_CodeSignature
* 
* 替换*app中的embedded.mobileprovision
    * cp embedded.mobileprovision Payload/*.app/ 
* 
* 创建*.plist文件并用该文件签名
    * /usr/bin/codesign -f -s "iPhone Distribution: ${cerName}" --entitlements *.plist Payload/*.app
* 
其中 ${cerName}是证书名称，可以在钥匙串证书中看到
* 修改Payload/*.app中Info(info).plist中bundleIdentifier的值为你自己的bundleIdentifier实际值
* 打包ipa
    * zip -r xxx.ipa Payload
* 
1.    拿到xxx.ipa进行验证去吧。
