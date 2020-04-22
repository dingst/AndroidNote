
1、阻塞问题：Address is already used

LocalServerSocket accept方法阻塞，导致调用close方法没有作用，再次创建LocalServerSocket时，报错
Address is already used错误。

解决办法是
LocalSocket localSocket = new LocalSocket();
            localSocket.connect(serverSocket.getLocalSocketAddress());
            localSocket.close();

参考链接：
https://stackoverflow.com/questions/8007982/java-serversocket-and-android-localserversocket


