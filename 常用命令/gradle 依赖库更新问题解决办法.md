gradle 依赖库更新，但是没有升级版本，本地更新问题解决办法：

windows：
1、删除.gradle -> caches -> modules-2 -> files-2.1 -> 找到依赖库，然后删除，

2、然后clean build android studio项目，这个时候就能更新到新的代码了。


linux/mac：

1、删除.gradle -> caches -> modules-2 -> files-2.1 -> 找到依赖库，然后删除，

2、 在android studio 中 执行 ./gradlew build --refresh-dependencies 


3、 然后clean build android studio项目，这个时候就能更新到新的代码了。如果还不行，就 invalidate caches/restart.
