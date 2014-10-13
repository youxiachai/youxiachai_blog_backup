# Objective-c 初学者眼里的OBJC(一)

## 前言

在进入IOS 开发的时候,就早有听说,objective-c 是一门很奇怪的语言, 对于,我这个不是新手的新手而言(之前学过 ,c ,java ,js. lua 等语言),objective-c 的确于目前主流语言有太多的不同了,在两天的学习中,从刚开始的不适应到慢慢习惯,从中体会到一些objective-c 与其他语言在设计上的不同.大大丰富了我的眼界.

## 用说不用想

在面向对象的程序语言设计中,最有影响力的莫过于SmallTalk,基本上,目前所有主流的面向对象的编程语言或多或少都有SmallTalk的影子 , 其中, objective-c 更是其中最明显的例子,其连语法都是照搬smalltalk的样式来设计.

smalltalk 算是比较早的面向对象的程序语言,其核心思想任何东西都是对象,而实现对象的交互通过消息要传递,其语言的名字Talking很形象的说明了这点.就好比,有两个叫做人的对象,这两个人要相互认识,得通过talking才能建立联系.在目前主流的面向对象语言,也就只有objective-c 设计上与一脉相承.

如果,说smalltalk的主体是说的话,其语法的设计上都是围绕着如何说来设计的话,那么其他的面向对象编程语言更多在语法设计上使用了想.例如,java:

```java
class Human {
	public void talkToOther (String something,Other other) {
    }
}

Human humanJava = new Human();
human1.talkToOther("Hello World!", new Other());
```

```objective-c
Human.h
@interface Human
- (void) talk:NSString *something ;
@end

Human.m
#import "Human.h"
@implementation
- (void) talk:NSString *something other:Other *other {

}
@end

#import "Human.h"
Human *humanOC = [[Human alloc] init];
[humanOC talk:@"Hello World!" other: [[Other alloc] init]];
```

就语法层面来看,已经可以发现,objective-c 与目前主流的面向对象语言最大的不同,就是在obj-c在对象调用上,有太啰嗦了,目前大部分的面向对象的语言它们更多的是通过想法,进行消息的传递,例如例子中的java,  在实例化一个对象中,要进行消息传递的时候,我们只需要让方法去想就行,而在obj-c 中,我们需要告诉我们的方法是这样,需要具体的消息的主体,这就是我看来obj-c 最奇怪的地方,我既然已经把一个对象实例化,在使用这个对象的时候,居然,要通过一个介质才能把消息的内容传递到对象里面.



## 说法与想法




