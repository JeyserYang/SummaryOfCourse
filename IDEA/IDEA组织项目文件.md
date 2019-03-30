1. 创建一个空项目，在src同级目录下增加以下文件夹：classes、config、db、lib、plugin、test；添加以下文件：start.bat、build.xml。如图所示：
![](https://upload-images.jianshu.io/upload_images/7024242-efb4275f73e4a8c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 选择swingdemo按F4显示工程配置，如图所示：
![](https://upload-images.jianshu.io/upload_images/7024242-8a53f35d312288a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将output path选择项目路径下的classes文件夹，将test output path选择为项目路径下test文件夹，便于ant打包，完成后点击确定。

![](https://upload-images.jianshu.io/upload_images/7024242-68401a801493ffda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 创建GUI FORM
![](https://upload-images.jianshu.io/upload_images/7024242-81c34447ab0db320.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 输入form名称，选择Intellij的GridLayoutManager布局方式，点击确定。Create bound class】必须勾选，IDEA会依名称建立.java与.form的档案，前者是Java原始档，后者存放图形元件。
![](https://upload-images.jianshu.io/upload_images/7024242-2aeb843ce9644095.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 进入GUI Designer画面，左上方是表单的元件清单，其下方是元件特性设定区，中间是图形画面，右方是所有元件的调色盘，在调色板按了某个元件后，将光标移入中间就能将之放入，放入时要决定对齐的方式。GUI Form默认会新增一个JPanel容器元件。

6. 在选择好组件，每个组件必须设置Field Name后，点选任意一个元件后按〔F4〕（Jump to source）跳至Java原始码该元件宣告处。将光标移到最后面按〔Alt+Insert〕以自动产生程式码，此时使用【Form main()】来产生显示此GUI表单的main()。

7. 最后的java文件应该像下面这样
```java
package unit22;

import javax.swing.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;

/**
 * @author jeyseryang
 * @version 1.0 2019-02-16-9:52
 */

public class GUIFormTest {
    private JPanel wrapPanel;
    private JTextField textField1;
    private JLabel yangweijie;

    public GUIFormTest() {
        yangweijie.addMouseListener(new MouseAdapter() {
            @Override
            public void mousePressed(MouseEvent e) {
                super.mousePressed(e);
            }
        });
    }

    public static void main(String[] args) {
        JFrame frame = new JFrame("GUIFormTest");
        frame.setContentPane(new GUIFormTest().wrapPanel);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.pack();
        frame.setVisible(true);
    }

}
```

### 出现错误：
- 如果我们勾选了`Custom Create`即代表你想要自己写代码来组织构件，如果此时你什么都没有做，就会出现`java.lang.NullPointerException`,
> Code from inputGui.java
> ```
> private void createUIComponents() {
>   // TODO: place custom component creation code here
> }
> ```
> You selected option "Custom Create" on some component in UI designer. You should create that component by yourself, otherwise it will fail. Uncheck that option and everything should be fine.