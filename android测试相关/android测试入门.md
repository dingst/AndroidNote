android studio中如何写测试代码：



转自：Flywith24的博客地址：

https://juejin.im/user/57c7f6870a2b58006b1cfd6c



作为Android开发者我们知道在Android Studio的Android视图中有三部分代码：



- app的逻辑代码（main source set）
- androidTest代码
- local test代码



test代码知道所有的main source set中的代码，因此可以测试这些类。但是app代码不知道test中的代码，并且androidTest和test都不知道对方的存在。事实上，当你构建出apk并提交应用市场时，测试代码并没有包含在内。



/ 依赖引用  /



下面标记的依赖 使用了test的引用方式testImplementation和androidTestImplementation。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvaYoWk6QEfYZHanTNM3Q8DUgryYpGy6bP7icu7TedDDZtt7X5OovMTZA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



注意：这些test代码不会打包到最终的apk文件中。



testImplementation引用的JUnit依赖只能在test source set中使用，这种依赖范围的限制是Gradle实现的。简单总结下：



- 三种source sets：main，test，androidTest
- 测试代码能访问app代码
- app 代码 不能 访问测试代码
- 测试不会被打入到Apk中
- 依赖范围包括：testImplementation和androidTestImplementation



/  运行第一个test  /



我们打开test source set ，看到其中有一个ExampleUnitTest。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvNAM0XKbIFcAm5WRk4dIh7ZwHQ8lTPgNnAd2iac4vPOWTGCeO8F0YRpw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到其内部只有一个addition_isCorrect()方法。有两个要素使它成为一个test：



- 使用了@Test注解
- 它存在于两个test source set之一



有了这两个要素，这个方法就可以独立的作为test运行。本示例test测试的内容在第15行，它被称之为断言（assertion）。断言是test的核心内容，它检查你的代码或者app行为是否符合你的预期。本示例中，断言检查 4 是否等于 2 + 2。按照规定，您需要将你的预期结果传入到expected参数中，将实际结果传入到actual参数中。@Test注解和断言语句都是JUnit下的。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTv4swWxmBjZ1LXyL6ZzvxHRrWQWChEg14znIkWrw2BeaLiayu9IkugXUg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



关于JUnit的更详细的的信息，请移步官方文档：

> https://junit.org/junit4/



让我们开始运行一下这个test，右击该方法，点击Run。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvgNSQ8k5e4Uozp6We0x9vKdnJpYn0UM7N6QqB3WDDlUg5hYnqWTEapQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



紧接着，Run窗口会出现：



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTv09G84ZTia8AjXt2wbIUDmarXsOhsnlObpa1XF9v1OjPZzHvo60uEelg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到该窗口显示了test的信息，显示出test是否通过以及有多少test通过。下面我们尝试一个test不通过的情况，我们加入一个断言，如下所示：



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTv7pCcN5zkIicNwtHoLV5cFOuR8oOQiczCicianu6IHZS7iaP0Oqxx67yiahSg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



这次我们点击Run窗口的绿色按钮来运行test：



![img](https://mmbiz.qpic.cn/mmbiz_gif/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTviax0JkA8ucahthqYqrwtWMxMD63duwib2RSDNPoyo6KLDBwKMvvugEjg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)



我们可以看到，即使只有一个断言失败，整个test失败了。窗口指出了预期的结果为5，而实际的结果为4，并且下边标记处错误发生在第15行，可以看到这的确是个bug。解决好bug ，我们再次运行test 。这次我们使用一个不一样的方式



![img](https://mmbiz.qpic.cn/mmbiz_gif/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvHCjg1JMGz9ic3Gia66mULrtlF2Q86JVFy4t2TsqMhIMQsxJ1IcM9p03w/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)



下面介绍一些其他运行 test 的方式。



可以右击类名选择Run选项：



![img](https://mmbiz.qpic.cn/mmbiz_gif/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvaspvlymOzLo6GicicSRtoVhzbN3CbQaGlWDyux4BoKBMjqSPfH920kzw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)



也可以在左侧视图中右击test source set选择Run按钮，该方法会运行所有test。在顶部可以切换要运行的test ，点击绿色按钮可以运行。也可切换回 app。



/  Android Test VS Test  /



下面我们来对比一下androidTest和test：



| test              | androidTest              |
| ----------------- | ------------------------ |
| Local Tests       | Instrumented Tests       |
| Local machine JVM | Real or emulated devices |
| Faster            | Slower                   |



我们来运行一个androidTest，可以看到启动了模拟器：



![img](https://mmbiz.qpic.cn/mmbiz_gif/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvhZHic4dp1HsicuAPEibIfDg2AuXicWicHQtJbrFRnhDc5wIJpnfVybgGGuQ/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)



/  写一个test  /



首先我们针对一个功能来创建test，如图所示，存在一个getForkAndOriginRepoStats()方法用于获取fork仓库和原始仓库的数据并返回StatsResult，其中StatsResult第一个参数为fork项目的百分比，第二个参数为原始项目的百分比。我们调用Generate ，选中test选项，在弹出框中选择JUnit4，点击OK并选择存放在local test中。这样我们就创建了一个test。



![img](https://mmbiz.qpic.cn/mmbiz_gif/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvJ6jGEnAianfynHsASZ2DhrUXC9A3Ps2PL90scUFdEZVbJdmgW11MhdA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)



接下来我们编写test 。可以看到自动创建出的test路径与app code中的代码路径的包名是对应的。我们先测试项目列表只有一个item，并且没有fork ，然后计算fork项目的百分比和原始项目的百分比。理论上讲，fork项目的百分比为0 ，而原始项目的百分比为100%，代码如下图所示，我们编写完毕后点击运行。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvX8DMia5FNBrh8qaKVYkYiayazicD6IH80BvUtPX7ejVAJn61TCLrbPYRA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到测试通过这是一个正常流程，我们还需要测试异常的流程，比如repos为empty list或者repos变量本身为null。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvwNlOnvlicuoicWT9KGK8dn6dDCu0n7I2FBd4BNYNT8P5PmZtiaWCeHTSA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到我们的代码中没有针对list为empty或null做判断，所以导致了空指针，之后我们修改代码后即可通过测试。



事实上，我们上面的编码流程叫做Test Driven Development(TDD)。



/  让你的test更具可读性  /



与写普通代码一样，您需要让您的test代码更具可读性，可以从三个方向入手：



- 优秀的命名
- Given/When/Then
- 借助断言库

###  

### 优秀的命名



首先我们来谈谈命名，我们知道test方法使用@Test 注解标记，理论上方法名可以随意命名，但随意的命名会导致可读性的降低，因此需要一些特定的命名规范：测试模块_ 动作或输入_ 结果状态。



例如上面的例子我们的命名为：getForkAndOriginRepoStats_noForked_returnHundredZero。



第一部分显示我们要测试的是getForkAndOriginRepoStats() 方法，第二部分代表我们需要的是没有fork仓库的数据源，第三部分是结果的状态，0%。

###  

### Given/When/Then



说完了命名我们来谈谈Given/When/Then。测试的基本结构是Given X，When Y，Then Z。



还是上面的例子



- Given 为你的测试逻辑提供数据源
- When 是你的实际操作
- Then 检查 test 是否通过



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvSo7j9AJPrAIOq9ewzn4dpvPX92ttR38U9XHibSYib3OYfGvTwxEISz3Q/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



借助断言库



上面示例最后的断言代码让人看着很别扭，我们可以借助断言库来提高这部分的可读性。



```
// 之前
assertEquals(result.forkPercent, 0f)

// 之后
assertThat(result.forkPercent, `is`(0f))
```



下面的语句就像人类的一句话，翻译下来就是断言forkPercent是0f。这样的写法需要引入一个库Hamcrest。



```
testImplementation "org.hamcrest:hamcrest-all:1.3"
```



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTv62D9WO2yhIyfrXEXUgN4ywOGTGE6ia82goQ9tYcBibUfUUKKSic5oXemA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



/  测试范围  /



测试范围指一个test测试多少代码。例如自动化测试根据测试范围可以分为：



- Unit Tests（单元测试）
- Integration Tests（集成测试）
- End to end Tests（端到端测试）



您的测试策略需要覆盖到所有的类型。



### Unit Tests



上面的示例我们已经写过了Unit Tests。



- 范围是单个方法或类
- 帮助查明失败原因
- 应该运行的很快，通常是本地测试
- 低保真度



他们的范围是单个的方法或类。如果Unit Tests失败了，您知道您的代码在哪里出了问题。因为它聚焦于很小一段代码。Unit Tests也意味着可以快速运行，由于您频繁地修改代码会使得它会频繁的运行，因此需要速度。Unit Tests通常是本地测试。它们有较低的保真度，因为现实世界您的app要执行很多代码而不仅仅是一个方法或者类。Unit Tests就像检查一个链条的每个环节是否能够正常运行。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvVX6KjBTcUQW7h9TOT60EdIYJ5E98pO3hsdpBiaEibrjHEXTcO09MYzGg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



但它不检查这些环节组合在一起是否能够运行，为此您需要Integration Tests。



### Integration Tests



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvTwPL9eMnUuS5gBKboKBtxGnxBCzwpkrl3hSNxlXzlvGsW6jcvMmYYA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



Integration Tests拥有更大的范围：



- 范围是几个类或单个功能
- 确保几个类共同运行
- 可以使用本地测试或机器测试



就像Integration这个词一样，Integration Tests整合一些类确保他们组合起来的表现符合预期。构建Integration Tests的方式是让他们测试单个功能，就像获取指定用户的Github仓库。



与Unit Tests 相比，Integration Tests有着更大的范围，但他们仍运行的很快并且有着很好的保真度。根据具体情况来判断使用本地测试还是机器测试，例如如果您写的Integration Tests涉及到了UI组件，那么您需要使用真机来测试了。



### End to end Tests



第三种类型是End to end Tests，该测试将一些列功能组合起来一起运行。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvq1hdjq16Uto7oR31Kcy3HY4hfSTicSIKpyHdeJX6rMOHGC2wfWN10ow/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



- 范围是app的大部分
- 高保真度
- 将app作为整体来测试
- 接近真实地使用，应该使用设备测试



End to end Tests测试app的大部分，它十分接近真实地使用，因此速度上会比较慢，它有着最高的保真度并确保您的应用作为一个整体运行。这些测试应该使用设备测试。



/  测试比重  /



推荐的测试比例是70%的单元测试，20%的集成测试，以及10%的端到端测试。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvUfpXNicyxj1fVw3L6dgFRHMh0YjvnqMXcgmwr7Dvj7S8wuhuDW718IA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



您能否轻松地在各个部分测试您的app取决于您的app使用的结构。例如，您的应用将所有逻辑都放置在一个activity的大的方法中，您可能可以写出端到端测试，但单元测试和集成测试则写不出来。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvjEfbTM5LsuuhQpTq4Zia6ux4eiaflVa6xiczqfbb4Udob8RZCvGsygzvw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



一个更好的架构应该将应用的逻辑拆分为多个方法和类，这允许每部分可以独立的测试。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvtqOpQ2jLBCic9nZexXJUicjJ4a6wvCBWDVG57W6lRY8JlCXT9e8L8q2A/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



对于单元测试，您可以测试ViewModel，Repository以及DAO。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvQvRZqpk6NnTKwN2aIJaUWTkTOaicnbNMjO2bRfvaFFd1F2dtL8drc7w/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



对于集成测试，您可以组合测试fragment和ViewModel ，或者您可以测试整个数据库代码。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvnYEDYffscEcYwQtg5Yk684WCHgXBP2LABtIJXutV1dyjl3mVQsmodg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



端到端测试会测试整个应用。



![img](https://mmbiz.qpic.cn/mmbiz_jpg/v1LbPPWiaSt4u8ic7tdXo9W7VxCxDrkWTvQ6eTXES9IpwQEPl2yzz7YeZ8BhPWIACTx1nDCkAZiccgd4YsDVOIiauA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



关于测试的原理，可移步。官方文档：

> https://developer.android.com/training/testing/fundamentals



test的codelab：

> https://codelabs.developers.google.com/codelabs/advanced-android-kotlin-training-testing-basics/index.html?index=..%2F..index#0