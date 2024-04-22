# ios-Swizzling-quickstart

在iOS应用程序中进行运行时代码插桩通常涉及到动态修改应用程序的行为，这可以通过各种技术实现，例如使用Method Swizzling或者动态库注入。以下是一个简单的例子，展示了如何在iOS应用程序中使用Objective-C的Method Swizzling技术来插入一个"Hello World"消息。

首先，你需要有一个现有的iOS项目或者创建一个新的项目。在这个例子中，我们将会修改一个视图控制器（ViewController），在视图加载时显示"Hello World"。

### 步骤 1: 创建一个新的Objective-C类

在Xcode中，创建一个新的Objective-C类，命名为`SwizzlingExample`。

```objc
// SwizzlingExample.h
#import <Foundation/Foundation.h>

@interface SwizzlingExample : NSObject
@end
```

```objc
// SwizzlingExample.m
#import "SwizzlingExample.h"
#import <objc/runtime.h>

@implementation SwizzlingExample

+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        Class class = [self class];
        
        SEL originalSelector = @selector(viewDidLoad);
        SEL swizzledSelector = @selector(xxx_viewDidLoad);
        
        Method originalMethod = class_getInstanceMethod(class, originalSelector);
        Method swizzledMethod = class_getInstanceMethod(class, swizzledSelector);
        
        BOOL didAddMethod = class_addMethod(class,
                                            originalSelector,
                                            method_getImplementation(swizzledMethod),
                                            method_getTypeEncoding(swizzledMethod));
        
        if (didAddMethod) {
            class_replaceMethod(class,
                                swizzledSelector,
                                method_getImplementation(originalMethod),
                                method_getTypeEncoding(originalMethod));
        } else {
            method_exchangeImplementations(originalMethod, swizzledMethod);
        }
    });
}

#pragma mark - Method Swizzling

- (void)xxx_viewDidLoad {
    [self xxx_viewDidLoad];
    NSLog(@"Hello World");
}

@end
```

### 步骤 2: 在你的视图控制器中导入SwizzlingExample

在你的`ViewController.m`文件中导入`SwizzlingExample`类。

```objc
#import "SwizzlingExample.h"
```

### 步骤 3: 运行你的应用程序

现在，当你的视图控制器加载时，它将会调用`xxx_viewDidLoad`方法，该方法会先调用原始的`viewDidLoad`方法，然后打印出"Hello World"。

请注意，这个例子假设你正在修改的是`UIViewController`的`viewDidLoad`方法。如果你想要修改其他类或者方法，你需要相应地调整`originalSelector`和`swizzledSelector`。

此外，由于苹果的安全限制，这种方法只能在开发过程中使用，用于调试和测试目的。在发布到App Store的应用程序中，不允许使用此类技术来修改应用程序的行为。

Citations:
[1] https://mas.owasp.org/MASTG/techniques/generic/MASTG-TECH-0051/
[2] https://www.appcoda.com/learnuikit/hello-world-explained.html
[3] https://www.codingexplorer.com/hello-world-first-ios-app-swift/
[4] https://www.reddit.com/r/swift/comments/5jkdkk/ios_dynamic_library_injection_you_can_hack_any/
[5] https://www.sitepoint.com/creating-hello-world-app-swift/
[6] https://docs.sentry.io/platforms/apple/guides/ios/performance/instrumentation/automatic-instrumentation/
[7] https://support.apple.com/guide/playgrounds-ipad/enter-code-in-an-app-playground-itcd4b4d3587/ipados
[8] https://stackoverflow.com/questions/31824046/instrumenting-ios-application
[9] https://developer.apple.com/videos/play/wwdc2019/411/
[10] https://www.youtube.com/watch?v=09TeUXjzpKs
[11] https://www.kodeco.com/16126261-instruments-tutorial-with-swift-getting-started
[12] https://developer.apple.com/videos/play/wwdc2022/110348/
[13] https://www.reddit.com/r/iOSProgramming/comments/ut5cje/where_do_you_store_your_ios_programming_notes/
[14] https://www.youtube.com/watch?v=jc81KrICVC4
[15] https://www.avanderlee.com/debugging/xcode-instruments-time-profiler/
[16] https://developer.apple.com/ios/planning/
[17] https://forums.macrumors.com/threads/notes-and-code-blocks-tips.2300321/
[18] https://apple.stackexchange.com/questions/312107/code-markup-in-apple-notes-app
[19] https://www.youtube.com/watch?v=HJDCXdhQaP0
[20] https://developer.apple.com/tutorials/app-dev-training/
