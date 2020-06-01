
Android-APK签名工具-jarsigner和apksigner
参考链接：https://blog.csdn.net/qq_32115439/article/details/55520012


查看签名文件的签名信息方法：
keytool -list -v -keystore 签名文件


签名验证：
1.方法一(keytool,只支持V1签名校验)
    进入JDK/bin, 输入命令
    keytool -printcert -jarfile MyApp.apk (显示签名证书信息)

    参数:
        -printcert           打印证书内容
        -jarfile <filename>  已签名的jar文件 或apk文件   

2.方法二(apksigner,支持V1和V2签名校验)
    进入Android SDK/build-tools/SDK版本, 输入命令
    apksigner verify -v --print-certs xxx.apk

    参数:
        -v, --verbose 显示详情(显示是否使用V1和V2签名)
        --print-certs 显示签名证书信息

    例如:
        apksigner verify -v MyApp.apk

        Verifies
        Verified using v1 scheme (JAR signing): true
        Verified using v2 scheme (APK Signature Scheme v2): true
        Number of signers: 1
