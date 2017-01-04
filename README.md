# FontSizeModify
iOS 关于字体根据不同屏幕尺寸等比适配的问题


网上找的一个人的demo,自己添加了一些东西，怕以后找不到，特传到git上，有需要屏幕适配的朋友可以看看

解决方法
新建一个UIButton的类别 重写 load 方法 利用OC的运行时 对所有的Button Label作处理（一般有文字的大部分是 Button Label）
代码如下
UIButton+MyFont.h
#import <UIKit/UIKit.h>
#import <objc/runtime.h>

/**
*  按钮
*/
@interface UIButton (myFont)

@end

/**
*  Label
*/
@interface UILabel (myFont)

@end
UIButton+MyFont.m

#import "UIButton+MyFont.h"

//不同设备的屏幕比例(当然倍数可以自己控制)
#define SizeScale ((IPHONE_HEIGHT > 568) ? IPHONE_HEIGHT/568 : 1)

@implementation UIButton (myFont)

+ (void)load{
Method imp = class_getInstanceMethod([self class], @selector(initWithCoder:));
Method myImp = class_getInstanceMethod([self class], @selector(myInitWithCoder:));
method_exchangeImplementations(imp, myImp);
}

- (id)myInitWithCoder:(NSCoder*)aDecode{
[self myInitWithCoder:aDecode];
if (self) {
//部分不像改变字体的 把tag值设置成333跳过
if(self.titleLabel.tag != 333){
CGFloat fontSize = self.titleLabel.font.pointSize;
self.titleLabel.font = [UIFont systemFontOfSize:fontSize*SizeScale];
}
}
return self;
}


@end

@implementation UILabel (myFont)

+ (void)load{
Method imp = class_getInstanceMethod([self class], @selector(initWithCoder:));
Method myImp = class_getInstanceMethod([self class], @selector(myInitWithCoder:));
method_exchangeImplementations(imp, myImp);
}

- (id)myInitWithCoder:(NSCoder*)aDecode{
[self myInitWithCoder:aDecode];
if (self) {
//部分不像改变字体的 把tag值设置成333跳过
if(self.tag != 333){
CGFloat fontSize = self.font.pointSize;
self.font = [UIFont systemFontOfSize:fontSize*SizeScale];
}
}
return self;
}

@end
