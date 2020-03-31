
1、查看手机在局域网的ip地址：

adb shell ip -f inet addr show wlan0 或者 adb shell ip -f inet addr show

2、手机cpu类型，是32位还是64位
adb shell getprop ro.product.cpu.abi 

