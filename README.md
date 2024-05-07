# iOS常用方法

## keywindow

```
-(UIWindow*) getTopWindow{
    UIWindow* window = nil;
    if (@available(iOS 13.0, *)){
        for (UIWindowScene* windowScene in [UIApplication sharedApplication].connectedScenes){
            if (windowScene.activationState == UISceneActivationStateForegroundActive){
                window = windowScene.windows.firstObject;
                break;
            }
        }
    }else{
        window = [UIApplication sharedApplication].keyWindow;
    }
    return window;
}
```

## 半透明颜色

```
[[UIColor whiteColor] colorWithAlphaComponent:透明值(0-1)];
```

##  navigatonbar

```
//隐藏navigationbar
self.navigationController.navigationBar.hidden = YES;
//修改默认返回按钮
self.navigationItem.backBarButtonItem = [[UIBarButtonItem alloc] initWithImage:[[UIImage imageNamed:@"fanhui"] imageWithRenderingMode:UIImageRenderingModeAlwaysTemplate] style:UIBarButtonItemStylePlain target:nil action:nil];
//返回按钮颜色
self.navigationController.navigationBar.tintColor = [UIColor systemOrangeColor];
//隐藏原来的返回按钮
self.navigationController.navigationBar.backIndicatorImage = [UIImage new];
self.navigationController.navigationBar.backIndicatorTransitionMaskImage = [UIImage new];
//消除底部阴影
self.navigationController.navigationBar.shadowImage = [UIImage new];
//消除bar半透明遮罩
self.navigationController.navigationBar.translucent = NO;
//修改title位置的view
self.navigationItem.titleView = xxxView;
```

## TableViewCell

```objective-c
//取消cell点击选中效果
self.selectionStyle = UITableViewCellSelectionStyleNone;
//分割线顶格 在willDisplayCell
if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {
		[cell setSeparatorInset:UIEdgeInsetsZero];
}
if ([cell respondsToSelector:@selector(setPreservesSuperviewLayoutMargins:)]) {
		[cell setPreservesSuperviewLayoutMargins:NO];
}
if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
		[cell setLayoutMargins:UIEdgeInsetsZero];
}
//隐藏分割线 在willDisplayCell 适用隐藏单个的情况
[cell setSeparatorInset:UIEdgeInsetsMake(0, 0, 0, SCREEN_WIDTH * 2 )];
```

## TableView

```
//去除分割线
[self.tableView  setSeparatorStyle:UITableViewCellSeparatorStyleNone];
//移除空白行下划线
tableView.tableFooterView = [[UIView alloc] initWithFrame:CGRectZero];
//去除果冻回弹效果
self.tableView.bounces = NO;
//设置分割线颜色
.separatorColor = 
//设置分割线四周间距
.separatorInset = UIEdgeInsets(top: 0, left: 20, bottom: 0, right: 20)
```

## UICollectionView
```
.alwaysBounceVertical = YES; //MARK: 使内容不满一屏幕也可以滚动
```

## UITextField

```
//收起键盘
[textfield resignFirstResponder];
[textfield endEditing:YES];
```

```
//键盘类型 keyboardType
UIKeyboardTypeDecimalPad 纯数字+小数点
UIKeyboardTypeNumberPad 纯数字
```

```
//判断是否是拼音候选词状态
//非候选状态
textField.markedTextRange == nil
//全选文字
UITextPosition *endDocument = textField.endOfDocument;
UITextPosition *end = [textField positionFromPosition:endDocument offset:0];
UITextPosition *start = [textField positionFromPosition:end offset:-textField.text.length];
textField.selectedTextRange = [textField textRangeFromPosition:start toPosition:end];
```

## UIScrollView

```
//设置整页滚动避免半张图的情况
.pagingEnabled = YES
//滑动时收起软键盘
.keyboardDismissMode = UIScrollViewKeyboardDismissModeOnDrag;
//滚动回弹效果
alwaysBounceHorizontal / alwaysBounceVertical
//停止滑动
[scrollView setContentOffset:scrollView.contentOffset animated:NO];
```

## NSNotification

```objective-c
//注册通知
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(changeLanguagesNotification) name:@"通知名" object:nil];
//通知对应方法
-(void)方法名:(NSNotification *)notification{
  //object
  NSDictionary * infoDic = [notification object];
  //userinfo
  [notification.userInfo objectForKey:@"text"]
}
//发送通知
 [[NSNotificationCenter defaultCenter] postNotificationName:@"通知名" object:@{@"parameter1":@"1",@"parameter2":@"2"} userInfo:nil];
//注销通知 iOS9之后不需要
//[[NSNotificationCenter defaultCenter] removeObserver:self];

// 清空锁屏通知
[[UNUserNotificationCenter currentNotificationCenter] removeAllDeliveredNotifications];

```

## UIView 布局相关
```objective-c
.autoresizingMask = //设置在父view中的对齐方式 flex
UIViewAutoresizingNone,
UIViewAutoresizingFlexibleLeftMargin,
UIViewAutoresizingFlexibleWidth,
UIViewAutoresizingFlexibleRightMargin,
UIViewAutoresizingFlexibleTopMargin,
UIViewAutoresizingFlexibleHeight,
UIViewAutoresizingFlexibleBottomMargin
//从父view添加、移除的事件(newSuperView==nil 代表是移除)
- (void)willMoveToSuperview:(nullable UIView *)newSuperview;
```
## UIView 点击事件
```objective-c
UITapGestureRecognizer *tapGestureRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(bgClick:)];
[xxView addGestureRecognizer:tapGestureRecognizer];

- (void)bgClick:(UITapGestureRecognizer *)gestureRecognizer {
}
```
## 剪贴板

```
UIPasteboard* pasteboard = [UIPasteboard generalPasteboard];
[pasteboard setString:@"复制的字符串内容"];
```

## UISWitch

```
//设置开关
xxx.on = YES;
//带动画效果的设置开关
[xxx setOn:YES animated:YES]
//设置背景色
xxx setOnTintColor:xxColor
```

## NSString

```objective-c
//截取前x位 下标从1算起 下同
[string substringToIndex:x]
//从第x位截取
[string substringFromIndex:x]
//判断是否包含xx
[目标字符串 rangeOfString:@"xx"].location != NSNotFound
//忽略大小写
[目标字符串 rangeOfString:@"xx" options:NSCaseInsensitiveSearch].length > 0
//不忽略大小写
[目标字符串 rangeOfString:@"xx" options:NSLiteralSearch].length > 0
//按照xx分割字符串为数组
数组 = [目标 componentsSeparatedByString:@"xx"]
//按照xx将数组组成字符串
NSString *string = [array componentsJoinedByString:@"xx"];
//替换字符串 把AA替换成CC
str = [str stringByReplacingOccurrencesOfString:@"AA" withString:@"CC"];
//追加字符串
NSString *str   stringByAppendingFormat
NSMutableString *str	appendFormat 
/**
 判断是否包含中文字符
 */
-(BOOL) isChinese:(NSString *) str{
    for(int i=0; i< [str length];i++){
        int a = [str characterAtIndex:i];
        if( a > 0x4E00 && a < 0x9FFF){
            return YES;
        }
    }
    return NO;
}
```

## 导航栏高度
> iPhone X 状态栏高度44,tabbar高度83 iPhone 6 状态栏高度 20,tabbar高度49

```
CGRect statusRect = [[UIApplication sharedApplication] statusBarFrame];
CGRect navRect = self.navigationController.navigationBar.frame;
//导航栏+状态栏的高度
CGFloat navigationAndStatus = statusRect.size.height + navRect.size.height;
//tabbar高度
self.tabBarController.tabBar.bounds.size.height
```

## 枚举

```
typedef NS_ENUM(NSUInteger, xxx) {
    xxxxx[=1],  
    
};
```

## 回到上2级页面
```
NSInteger index = (NSInteger)[[self.navigationController viewControllers] indexOfObject:self];
if (index > 2) {
       [self.navigationController popToViewController:[self.navigationController.viewControllers objectAtIndex:(index-2)] animated:YES];
}else{
       [self.navigationController popToRootViewControllerAnimated:YES];
}
```

## LLDB

```
expr/call 执行命令
```

## 加载框

```
[ProgressHUD showLoadingCalculators:^(ProgressHUD *make) {
    make.maskType(HUDMaskType_Clear);//不允许操作其他
    make.message(loading);
    make.inView(self.view);
    make.waitingTime(6).changeMaskType(YES);
    make.afterDelay(3);//失败提示展示的时间
}];
```

## MJExtension

```
//替换属性别名
+ (NSDictionary *)mj_replacedKeyFromPropertyName{
    return @{
             @"detail_id" : @"id"//前边的是你想用的key，后边的是返回的key
             };
}
//JSON数组指定类型
+ (NSDictionary *)mj_objectClassInArray{
    return @{@"colorArr" : @"AccessoryMaterialColorModel"};//前边，是属性数组的名字，后边就是类名
}

```

## 文件存储 持久化

### 常用目录获取

```
NSString *home = NSHomeDirectory();
NSString *temp = NSTemporaryDirectory();
NSString *documents = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];
NSString *cache =  [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];
```
### plist(不能存储自定义对象)
```
NSString *dir = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
NSString *path = [dir stringByAppendingPathComponent:@"test.plist"];
NSMutableDictionary *dict = [NSMutableDictionary dictionary];
dict[@"value"] = @"Hello World!";
BOOL result = [dict writeToFile:path atomically:YES];
```

### NSUserDefaultst(不能存储自定义对象)

```
[[NSUserDefaults standardUserDefaults] setObject:@"xxx" forKey:@"view"];
[[NSUserDefaults standardUserDefaults] stringForKey:@"view"];
```

## Archiver 
```
//对象实现NSCoding的方法
- (instancetype)initWithCoder:(NSCoder *)coder;
- (void)encodeWithCoder:(NSCoder *)coder;
//归档1
NSData *data = [NSKeyedArchiver archivedDataWithRootObject:array requiringSecureCoding:YES error:nil];
[[NSFileManager defaultManager] createFileAtPath:path contents:data attributes:nil];
//归档2
BOOL result = [NSKeyedArchiver archiveRootObject:person toFile:path];

//解档1
NSMutableArray *array = [NSMutableArray array];
NSData *data = [fm contentsAtPath:filePath];
array = [NSKeyedUnarchiver unarchivedObjectOfClasses:[NSSet setWithObjects:[NSArray class],[TodoItem class] ,nil]  fromData:data error:nil];
//解档2
Person *unPerson =  [NSKeyedUnarchiver unarchiveObjectWithFile:path];
```


## 打包

```
# Type a script or drag a script file from your workspace to insert its path.
#自增buildnumber
ipaName='TS_wanghui17'
echo ${CONFIGURATION}
if [ "Debug" == "${CONFIGURATION}" ]
then
buildNumber=$(/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "${PROJECT_DIR}/${INFOPLIST_FILE}")
if [ "$buildNumber" == "\$(CURRENT_PROJECT_VERSION)" ]
then
buildNumber=$CURRENT_PROJECT_VERSION
fi
buildNumber=$(($buildNumber + 1))
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "${PROJECT_DIR}/${INFOPLIST_FILE}"
echo "build number increase"
fi
echo "开始打包……"
echo ${URLNAME}
dateStr=$(date "+%y%m%d")
echo $dateStr
echo $buildNumber
cd ${BUILD_DIR}/${CONFIGURATION}-iphoneos
PAYLOAD='Payload'
# 这里的-d 参数判断$PAYLOAD是否存在
if [ ! -d "$PAYLOAD" ]
then
mkdir $PAYLOAD
fi
cp -r ./${PROJECT_NAME}.app $PAYLOAD
zip -q -r  /Volumes/home/ipa/${ipaName}_${dateStr}_${buildNumber}.ipa $PAYLOAD
#在Finder中打开，在使用中可暂时注释掉避免每次都打开
#open -R .

```
