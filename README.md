# ios-Swizzling-quickstart

在Swift语言编写的iOS应用中进行运行时代码插桩通常涉及到动态修改程序的行为，这可以通过使用特定的框架如Frida来实现。以下是一个简单的例子，展示如何在一个Swift编写的iOS应用中插入"Hello, World"字符串，并在应用运行时动态显示这个字符串。

### 准备工作

首先，确保你的开发环境中安装了以下工具：
- Xcode
- Frida（一个动态代码插桩工具包）

### 步骤 1: 创建一个简单的iOS Swift应用

1. 打开Xcode，创建一个新的iOS项目。
2. 选择App模板，输入项目名称，选择Swift作为开发语言。
3. 在项目的`ViewController.swift`文件中，添加一个按钮和一个标签用于显示文本。

```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // 创建一个按钮
        let button = UIButton(frame: CGRect(x: 100, y: 200, width: 100, height: 50))
        button.setTitle("点击我", for: .normal)
        button.backgroundColor = .blue
        button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)
        self.view.addSubview(button)
        
        // 创建一个标签
        let label = UILabel(frame: CGRect(x: 50, y: 100, width: 300, height: 50))
        label.textAlignment = .center
        label.text = "Hello, World"
        label.isHidden = true // 初始时隐藏标签
        label.tag = 100 // 设置标签的tag
        self.view.addSubview(label)
    }

    @objc func buttonTapped() {
        if let label = self.view.viewWithTag(100) as? UILabel {
            label.isHidden = !label.isHidden
        }
    }
}
```

### 步骤 2: 使用Frida进行代码插桩

1. 确保你的iOS设备已经越狱，并且安装了Frida。
2. 编写一个Frida脚本来修改`buttonTapped`方法的行为，使其在每次点击时都打印"Hello, World"。

```javascript
// hook.js
Interceptor.attach(ObjC.classes.ViewController['- buttonTapped'].implementation, {
    onEnter: function(args) {
        console.log("Hello, World");
    }
});
```

3. 将你的iOS设备连接到电脑，并通过USB或网络确保两者可以通信。
4. 在终端中运行以下命令来启动Frida脚本：

```bash
frida -U -f com.example.YourAppName -l hook.js --no-pause
```

替换`com.example.YourAppName`为你的应用的bundle identifier。

### 运行和测试

启动应用，点击按钮，你应该能在Frida的控制台看到"Hello, World"的输出，同时标签的显示状态会改变。

这个例子展示了如何在运行时使用Frida对Swift编写的iOS应用进行简单的代码插桩。这种技术可以用于更复杂的应用场景，如功能测试、安全分析等[5][10].

Citations:
[1] https://www.cnblogs.com/xiaowenshu/p/10382177.html
[2] https://www.cnblogs.com/lofly/articles/17488061.html
[3] https://ming1016.github.io/2021/05/22/acfun-swift-practice/
[4] https://github.com/alphaSeclab/awesome-reverse-engineering
[5] https://blog.betamao.me/posts/2021/trace_to_dbi_to_frida/
[6] https://zhengry.github.io/2020/07/21/iOS%E4%B9%8BApp%E5%90%AF%E5%8A%A8%E4%BC%98%E5%8C%96/
[7] https://www.zhihu.com/account/unhuman?need_login=true&type=S6E3V1
[8] https://juejin.cn/post/6844904130406793224
[9] https://github.com/luckypoem/awesome-hacking-lists-1/blob/master/README.md
[10] https://juejin.cn/post/7251501966592917563
[11] http://satanwoo.github.io
[12] https://www.infoq.cn/article/ot9iwcf7fkjeoeqzlwgl
[13] https://blog.csdn.net/cunchi4221/article/details/107479105
[14] https://blog.csdn.net/wywinstonwy/article/details/125263277
[15] https://blog.csdn.net/freeking101/article/details/136944696
[16] https://juejin.cn/post/6844904168201666574
[17] https://blog.csdn.net/tjcwt2011/article/details/128618347
[18] https://cloud.tencent.com/developer/news/582864
[19] https://www.volcengine.com/theme/9150195-Z-7-1
[20] https://cloud.tencent.com/developer/article/2191246
