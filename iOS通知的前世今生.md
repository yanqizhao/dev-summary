# iOS通知的前世今生

在项目决定添加iOS10新增通知的API后我开始总结了下面的内容，没想到一写就是一个多月，从最开始只是改了几个方法到后来基本搞明白不同系统的通知是如何演变而来的，时间虽长，总算没有辜负。

## 本地推送

### 申请

#### iOS10

```
// 获取当前通知中心
UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];

// 设置代理
center.delegate = self;
       
// 申请权限
[center requestAuthorizationWithOptions:UNAuthorizationOptionAlert | UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionCarPlay
                              completionHandler:^(BOOL granted, NSError * _Nullable error) {
        }];
```

#### iOS 8以上10以下

```
// 设置允许展示的内容（角标、声音、窗口）
UIUserNotificationType userNotiType =
        UIUserNotificationTypeBadge |
        UIUserNotificationTypeSound |
        UIUserNotificationTypeAlert;
        
UIUserNotificationSettings *mySettings = [UIUserNotificationSettings settingsForTypes:userNotiType categories:nil];
[[UIApplication sharedApplication] registerUserNotificationSettings:mySettings];
```

#### iOS 7

```
UIRemoteNotificationType remoteNotiType =
        UIRemoteNotificationTypeBadge |
        UIRemoteNotificationTypeAlert |
        UIRemoteNotificationTypeSound;
```
从类名可以看出，在7系统中，本地通知并没有被当做一种打扰用户的行为，所以申请注册通知的类名中用到了remote，本地通知并没有参与其中，并且远程通知也不需要弹窗由用户来决定是否允许推送，这里的申请与远程同种的注册只是针对苹果服务器的。

### 创建和发送

#### iOS 10

```
UNMutableNotificationContent *content = [[UNMutableNotificationContent alloc] init];
content.body = @"notification";
content.sound = [UNNotificationSound defaultSound];
content.userInfo = @{@"index" : @(1)};
        
UNTimeIntervalNotificationTrigger *trigger = [UNTimeIntervalNotificationTrigger triggerWithTimeInterval:3 repeats:NO];
        
UNNotificationRequest *request = [UNNotificationRequest requestWithIdentifier:@"notification1" content:content trigger:trigger];
        
[[UNUserNotificationCenter currentNotificationCenter] addNotificationRequest:request withCompletionHandler:^(NSError * _Nullable error) {
    if (error) {
        DebugLog(@"%@", error);
    }
}];
```

#### iOS 10以下

```
UILocalNotification *localNoti = [[UILocalNotification alloc] init];        
localNoti.fireDate = [NSDate dateWithTimeIntervalSinceNow:3];
localNoti.timeZone = [NSTimeZone defaultTimeZone];
localNoti.soundName = UILocalNotificationDefaultSoundName;
localNoti.alertBody = @"notification";
localNoti.userInfo = @{@"index" : @(1)};
 // 将当前本地通知加入计划当中，到fireDate设置的时间会进行推送       
[[UIApplication sharedApplication] scheduleLocalNotification:localNoti];
// 立刻推送当前的本地通知，不管fireDate的设置如何
[[UIApplication sharedApplication] presentLocalNotificationNow:localNoti];
```

### 展示和处理

#### iOS 10

##### 后台

在该回调中处理。

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void (^)())completionHandler {
    // must be called
    completionHandler();
}
```

##### 关闭进程

在didFinishLaunching方法中取得当前推送并处理。

```
// 这里的通知类型为UIConcreteLocalNotification
UILocalNotification *localNotification = [launchOptions valueForKeyPath:UIApplicationLaunchOptionsLocalNotificationKey];
    if (localNotification) {
        // TODO
    }
```

##### 前台

在该回调中处理。

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler;
```

#### iOS 10以下

##### 后台

在该回调中处理。

```
- (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification;
```

##### 关闭进程

在didFinishLaunching方法中取得当前推送并处理。

```
UILocalNotification *localNotification = [launchOptions valueForKeyPath:UIApplicationLaunchOptionsLocalNotificationKey];
    if (localNotification) {
        // TODO
    }
```

### 撤销

#### iOS 10

```
// 获取计划中的推送
[[UNUserNotificationCenter currentNotificationCenter] getPendingNotificationRequestsWithCompletionHandler:^(NSArray<UNNotification *> * _Nonnull notifications) { 
}];
// 撤销计划中的指定identifier的推送
[[UNUserNotificationCenter currentNotificationCenter]] removePendingNotificationRequestsWithIdentifiers:@"notification1"];
// 撤销全部计划中的推送
[[UNUserNotificationCenter currentNotificationCenter] removeAllPendingNotificationRequests];

// 获取已发送的推送
[[UNUserNotificationCenter currentNotificationCenter] getDeliveredNotificationsWithCompletionHandler:^(NSArray<UNNotification *> * _Nonnull notifications) { 
}];
// 撤销已发送的指定identifier的推送
[[UNUserNotificationCenter currentNotificationCenter]] removeDeliveredNotificationsWithIdentifiers:@"notification1"];
// 撤销全部已发送的推送
[[UNUserNotificationCenter currentNotificationCenter] removeAllDeliveredNotifications];
```
#### iOS 10以下

```
// 撤销指定的通知
[[UIApplication sharedApplication] cancelLocalNotification:localNoti];
// 撤销全部通知
[[UIApplication sharedApplication] cancelAllLocalNotifications];

// 上述两个方法不会立即执行，会等到下一个RunLoop开始的时候执行

// 该方法将计划中的通知全部置空，会立即执行
[[UIApplication sharedApplication] setScheduledLocalNotifications:nil];
/*
@property(nonatomic, copy) NSArray<UILocalNotification *> *scheduledLocalNotifications;

All currently scheduled local notifications.
This property holds an array of UILocalNotification objects representing the current scheduled local notifications. Use this property to access the currently scheduled notifications, perhaps to cancel them. Assigning a new value to this property implicitly schedules all of the notifications in the new array.
This method may be faster than using scheduleLocalNotification: when scheduling a large number of notifications.
*/
```

## 远程推送

### 申请和注册

#### iOS10

```
// 申请同本地通知
// 注册通知
[[UIApplication sharedApplication] registerForRemoteNotifications];
```

#### iOS 8以上10以下

```
// 申请同本地通知
// 注册通知
[[UIApplication sharedApplication] registerForRemoteNotifications];
```

#### iOS 7

```
// 申请同本地推送
// 注册,这里的remoteNotiType为申请权限时允许展示的内容
[[UIApplication sharedApplication] registerForRemoteNotificationTypes:remoteNotiType];
```

### 注册回调

> 不区分系统

#### 成功

```
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken;
```

#### 失败

```
- (void)application:(UIApplication *)app didFailToRegisterForRemoteNotificationsWithError:(NSError *)err;
```

### 创建和发送

第三方平台：信鸽、极光、个推
推送工具测试

### 展示和处理

#### iOS10

##### 后台和关闭进程

在该回调中处理。

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void (^)())completionHandler {
    // must be called
    completionHandler();                 
}
```

##### 前台

在该回调中处理。

```
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler {
    completionHandler(UNNotificationPresentationOptionAlert | UNNotificationPresentationOptionSound);
    NSLog(@"%@", notification);
}
```

#### iOS10以下

在该回调中处理。

```
- (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary *)userInfo; 
```

## 新增功能及特性

![图片来自伟大的喵神](http://upload-images.jianshu.io/upload_images/1684066-216fcb7213a21c15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### iOS 7

> 静默推送

### iOS 8

> 更新权限请求、新增category action、新增本地化内容推送

### iOS 9

> 新增text input action

### iOS 10

> UNUserNotification API

* 获取用户允许的通知形式

	```
	[[UNUserNotificationCenter currentNotificationCenter] getNotificationSettingsWithCompletionHandler:^(UNNotificationSettings * _Nonnull settings) {
        NSLog(@"%ld", settings.authorizationStatus); // UNAuthorizationStatusAuthorized
        NSLog(@"%ld", settings.notificationCenterSetting); // UNNotificationSettingEnabled
        NSLog(@"%ld", settings.alertSetting); // UNNotificationSettingEnabled
        NSLog(@"%ld", settings.soundSetting); // UNNotificationSettingEnabled
        NSLog(@"%ld", settings.badgeSetting); // UNNotificationSettingEnabled
        NSLog(@"%ld", settings.lockScreenSetting);// UNNotificationSettingEnabled
        NSLog(@"%ld", settings.carPlaySetting); // UNNotificationSettingNotSupported
        NSLog(@"%ld", settings.alertStyle); // UNAlertStyleBanner
    }];
	```

* 推送文本分为标题、副标题和内容 （8.2系统就存在了）
* 本地化文本对应 
	[iOS8自定义推送显示按钮及推送优化](http://www.jianshu.com/p/803bfaae989e)
* Category Action

	添加category与action后，用户在收到通知后可通过下拉通知或3D Touch点按通知中心页面中的通知来进行操作，选中action后可打开应用并进行页面跳转
	
	```
	// 创建action
	// options可选项：
	// UNNotificationActionOptionAuthenticationRequired
	// UNNotificationActionOptionDestructive
	// UNNotificationActionOptionForeground
	// UNNotificationActionOptionNone
	
	UNNotificationAction *action1 = [UNNotificationAction actionWithIdentifier:@"action1" title:@"打开应用" options:UNNotificationActionOptionNone];
	UNNotificationAction *action2 = [UNNotificationAction actionWithIdentifier:@"action1" title:@"什么都不做" options:UNNotificationActionOptionDestructive];
	
	// 创建category
	// options可选项：
	// UNNotificationCategoryOptionNone
	// UNNotificationCategoryOptionCustomDismissAction
	// UNNotificationCategoryOptionAllowInCarPlay
	UNNotificationCategory *category = [UNNotificationCategory categoryWithIdentifier:@"category1" actions:@[action1, action2] intentIdentifiers:@[] options:UNNotificationCategoryOptionNone];
	
	// 添加category
	[[UNUserNotificationCenter currentNotificationCenter] setNotificationCategories:[NSSet setWithArray:@[category]]];
	```
* Extension
	
## 注意点
	
### iOS 10
	
1. 需要在Linked Frameworks and Libraries添加UserNotifications.framework
2. 每一个请求的Identifier需不相同，否则会替换已添加到通知中心的通知，如果已经发送相同Identifier的通知，则会被覆盖更新

	![](http://upload-images.jianshu.io/upload_images/1684066-6a6945ebf80eb783.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.  `UNCalendarNotificationTrigger`类的使用存在问题，传入的components与trigger触发通知的时间并不相同，可以改用`UNTimeIntervalNotificationTrigger`计算间隔秒数

	[这里是stackoverflow上的相关问题](http://stackoverflow.com/questions/40411812/does-untimeintervalnotificationtrigger-nexttriggerdate-give-the-wrong-date)

	```
	// 这里传入的components与trigger触发通知的时间并不相同
	UNCalendarNotificationTrigger *trigger = [UNCalendarNotificationTrigger triggerWithDateMatchingComponents:components repeats:NO];
	
	NSLog(@"components:%@\nnextTriggerDate:%@", components, [trigger nextTriggerDate]);
	
	/*console log
	components:<NSDateComponents: 0x1741547f0>
	    Calendar Year: 2016
	    Month: 11
	    Leap month: no
	    Day: 20
	    Hour: 20
	    Minute: 45
	    Weekday: 1
	    Weekday Ordinal: 3
	nextTriggerDate:2016-11-30 16:00:44 +0000
	*/
	```

