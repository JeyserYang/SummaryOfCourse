1. 打开IDEA的file -> Project Structure，进入项目配置页面。如下图：
![](https://upload-images.jianshu.io/upload_images/7024242-1a2d91ada40122c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 点击Artifacts，进入Artifacts配置页面，点击 + ，选择如下图的选项
![](https://upload-images.jianshu.io/upload_images/7024242-e3d59851bbdda965.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 进入Create JAR from Modules页面，按照如下图配置
![](https://upload-images.jianshu.io/upload_images/7024242-4a10aa20e17e0a1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 第一步选择Main函数执行的类。
第二步选择如图的选项，目的是对第三方Jar包打包时做额外的配置，如果不做额外的配置可不选这个选项（但不保证打包成功）
第三步需要在src/main目录下，新建一个resources目录，将MANIFEST.MF文件保存在这里面，因为如果用默认缺省值的话，在IDEA12版本下会有bug。
点击OK之后，出现如下图界面，右键点击<output root>，点击Create Directory,创建一个libs，将所有的第三方JAR放进libs目录下。
![](https://upload-images.jianshu.io/upload_images/7024242-d750cfb67087a662.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 成功之后，如下图所示：
![](https://upload-images.jianshu.io/upload_images/7024242-5e33d64507353520.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6. 放入之后，点击我们要打成的jar的名字，这里面是kafka-cps.jar,选择classpath进行配置。
![](https://upload-images.jianshu.io/upload_images/7024242-02285b093f265f39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7.编辑的结果如下
 ![](https://upload-images.jianshu.io/upload_images/7024242-d00e9f8d1af216dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8. 这里将所有的jar都写在libs/里面。点击OK，回到配置页面。
同时还注意在配置页面，勾选build on make
![](https://upload-images.jianshu.io/upload_images/7024242-51d9f546f9a2176a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

9. 最后点击配置页面的OK，完成配置。回到IDEA,点击Build->Build Artifacts，选择build
![](https://upload-images.jianshu.io/upload_images/7024242-fcb727d972d347b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

10. 就会生成我们需要的jar包。其位置在项目目录的out目录下/out/artifacts/kafka_cps_jar。
下面放一个正确配置的清单文件内容
![](https://upload-images.jianshu.io/upload_images/7024242-0d4b989d183ba997.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)