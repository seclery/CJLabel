# CJLabel

## 功能简介

 * CJLabel 继承自 UILabel，其文本绘制基于NSAttributedString实现，同时增加了图文混排、富文本展示以及添加自定义点击链点并设置点击链点文本属性的功能。
 *
 * CJLabel 与 UILabel 不同点：
 *
   1. `- init` 不可直接调用init初始化，请使用`initWithFrame:` 或 `initWithCoder:`，以便完成相关初始属性设置
 
   2. `attributedText` 与 `text` 均可设置文本，注意 [self setText:text]中 text类型只能是NSAttributedString或NSString
 
   3. `NSAttributedString`不再通过`NSTextAttachment`显示图片（使用`NSTextAttachment`不会起效），请调用
      `- configureAttributedString: addImageName: imageSize: atIndex: attributes:`或者
      `- configureLinkAttributedString: addImageName: imageSize: atIndex: linkAttributes: activeLinkAttributes: parameter: clickLinkBlock: longPressBlock:`方法添加图片
 
   4. 新增`extendsLinkTouchArea`， 设置是否加大点击响应范围，类似于UIWebView的链点点击效果
 
   5. 新增`shadowRadius`， 设置文本阴影模糊半径，可与 `shadowColor`、`shadowOffset` 配合设置，注意改设置将对全局文本起效
 
   6. 新增`textInsets` 设置文本内边距
 
   7. 新增`verticalAlignment` 设置垂直方向的文本对齐方式
   
   8. 新增`delegate` 点击链点代理
 *
 * CJLabel 已知bug：
 *
   `numberOfLines`大于0且小于实际`label.numberOfLines`，同时`verticalAlignment`不等于`CJContentVerticalAlignmentTop`时，文本显示位置有偏差

## CJLabel引用
### 一、直接导入
下载demo，将CJLabel文件夹导入项目，引用头文件`#import "CJLabel.h"`
### 二、CocoaPods安装
* Podfile<br/>
```ruby
platform :ios, '7.0'
pod 'CJLabel', '~> 2.1.2'
```

## API介绍
### 1、NSMutableAttributedString 增加若干属性<br/>

   /**
    背景填充颜色。值为UIColor。默认 `nil`。
    该属性优先级低于NSBackgroundColorAttributeName，如果设置NSBackgroundColorAttributeName会覆盖kCJBackgroundFillColorAttributeName
    */<br/>
   `extern NSString * const kCJBackgroundFillColorAttributeName;`

   /**
    背景边框线颜色。值为UIColor。默认 `nil`
    */<br/>
   `extern NSString * const kCJBackgroundStrokeColorAttributeName;`

   /**
    背景边框线宽度。值为NSNumber。默认 `1.0f`
    */<br/>
   `extern NSString * const kCJBackgroundLineWidthAttributeName;`

   /**
    背景边框线圆角角度。值为NSNumber。默认 `5.0f`
    */<br/>
   `extern NSString * const kCJBackgroundLineCornerRadiusAttributeName;`

   /**
    点击时候的背景填充颜色。值为UIColor。默认 `nil`。
    该属性优先级低于NSBackgroundColorAttributeName，如果设置NSBackgroundColorAttributeName会覆盖kCJActiveBackgroundFillColorAttributeName
    */<br/>
   `extern NSString * const kCJActiveBackgroundFillColorAttributeName;`

   /**
    点击时候的背景边框线颜色。值为UIColor。默认 `nil`
    */<br/>
   `extern NSString * const kCJActiveBackgroundStrokeColorAttributeName;`

### 2、CJLabel API
* 计算指定NSAttributedString的size大小
```objective-c
+ (CGSize)sizeWithAttributedString:(NSAttributedString *)attributedString
                   withConstraints:(CGSize)size
            limitedToNumberOfLines:(NSUInteger)numberOfLines;
CGSize size = [CJLabel sizeWithAttributedString:str withConstraints:CGSizeMake(320, CGFLOAT_MAX) limitedToNumberOfLines:0]
  ```
  
* 插入图片链点
在指定位置插入图片，插入图片为可点击的链点！！！返回插入图片后的NSMutableAttributedString（图片占位符所占的NSRange={loc,1}）
```objective-c
+ (NSMutableAttributedString *)configureLinkAttributedString:(NSAttributedString *)attrStr
                                                addImageName:(NSString *)imageName
                                                   imageSize:(CGSize)size
                                                     atIndex:(NSUInteger)loc
                                              linkAttributes:(NSDictionary *)linkAttributes
                                        activeLinkAttributes:(NSDictionary *)activeLinkAttributes
                                                   parameter:(id)parameter
                                              clickLinkBlock:(CJLabelLinkModelBlock)clickLinkBlock
                                              longPressBlock:(CJLabelLinkModelBlock)longPressBlock;
attStr = [CJLabel configureLinkAttributedString:attStr
                                               addImageName:@"CJLabel.png"
                                                  imageSize:CGSizeMake(60, 43)
                                                    atIndex:imageRange.location+imageRange.length
                                             linkAttributes:@{
                                                              kCJBackgroundStrokeColorAttributeName:[UIColor blueColor],
                                                              kCJBackgroundLineWidthAttributeName:@(self.index == 5?1:2),
                                                              }
                                       activeLinkAttributes:@{
                                                              kCJActiveBackgroundStrokeColorAttributeName:[UIColor redColor],
                                                              }
                                                  parameter:@"图片参数"
                                             clickLinkBlock:^(CJLabelLinkModel *linkModel){
                                                 [self clickLink:linkModel isImage:YES];
                                             }longPressBlock:^(CJLabelLinkModel *linkModel){
                                                 [self clicklongPressLink:linkModel isImage:YES];
                                             }];
  ```
## cocoapods安装
* Podfile<br/>
```ruby
platform :ios, '7.0'
pod 'CJLabel', '~> 1.0.3'
```

## V1.0.0
v1.0.0版本注意：文本内存在相同链点时只有首次出现的链点能够响应点击

<br/>
## V1.0.1
*  增加文本中内容相同的链点能够响应点击属性sameLinkEnable，必须在设置self.attributedText前赋值，默认值为NO，只取文本中首次出现的链点。<br/>
*  CJLinkLabelModel的linkString改为NSString类型<br/>

<br/>
## V1.0.2
* 点击链点增加扩展属性parameter<br/>
* 增加方法`- addLinkString: linkAddAttribute: linkParameter: block:`
