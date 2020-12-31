## 低功耗门铃

### 判断是否是低功耗门铃

根据`TuyaSmartDeviceModel+IPCSDK`中的接口判断设备是否为低功耗设备。

**接口说明**

设备是否是低功耗设备

```objc
- (BOOL)isLowPowerDevice;
```

如果设备即是摄像机设备又是低功耗设备，可以认为设备是低功耗门铃。

### 休眠唤醒

低功耗门铃由电池供电，为了节省电量，在一定时间内没有 p2p 连接会休眠，休眠后无法直接连接 p2p，需要先唤醒设备，再连接 p2p 通道。使用`TuyaSmartDevice`类提供的接口来唤醒设备。

**接口说明**

唤醒休眠中的低功耗设备。

```objc
- (void)awakeDeviceWithSuccess:(nullable TYSuccessHandler)success failure:(nullable TYFailureError)failure;
```

当 `success`调用时，只是表示唤醒命令已经成功下发给门铃设备，并不代表门铃设备已启动。当门铃设备启动时，会通过设备功能点`TuyaSmartCameraWirelessAwakeDPName`上报 `YES`。

**示例代码**

ObjC

```objc
- (void)viewDidLoad {
		[super viewDidLoad];
	  self.dpManager = [[TuyaSmartCameraDPManager alloc] initWithDeviceId:self.devId];
		self.device = [TuyaSmartDevice deviceWithDeviceId:self.devId];
		// 添加 DP 监听
		[self.dpManager addObserver:self];
  
  	[self start];
}

- (void)start {
  	if (self.isConnected) {
				[self.videoContainer addSubview:self.camera.videoView];
				self.camera.videoView.frame = self.videoContainer.bounds;
        [self.camera startPreview];
		}else if (!self.isConnecting) {
      	if (self.device.deviceModel.isLowPowerDevice) {
        		[self.device awakeDeviceWithSuccess:nil failure:nil];
    		}
				[self.camera connect];
      	self.isConnecting = YES;
		}
}

```

Swift

``` swift
func viewDidLoad() {
  	super.viewDidLoad()
	  self.dpManager = TuyaSmartCameraDPManager(deviceId: self.devId)
		self.device = TuyaSmartDevice(deviceId: self.devId)
		// 添加 DP 监听
		self.dpManager?.addObserver(self)
  
  	self.start()
}

func start() {
  	guard self.isConnected || self.isConnecting else {
	    	if self.device?.deviceModel.isLowPowerDevice() {
          	self.device?.awake(success: nil, failure: nil)
        }
				self.camera.connect()
				self.isConnecting = true
      	return
    }
    self.videoContainer.addSubView(self.camera.videoView)
    self.camera.videoView.frame = self.videoContainer.bounds
    self.camera.startPreview()
}
    
```

### 门铃呼叫

设备成功绑定到家庭并且在线状态下，有人按门铃，IPC SDK 将收到门铃呼叫的事件。事件以通知的形式广播。

* 通知名：**kNotificationMQTTMessageNotification**
* 参数：
  * **devId**：触发门铃呼叫的设备 id
  * **etype**：事件标识，```doorbell```表示门铃呼叫

**示例代码**

ObjC

```objc
#define kTuyaDoorbellNotification @"kNotificationMQTTMessageNotification"

- (void)observeDoorbellCall:(void(^)(NSString *devId, NSString *type))callback {
    if (!callback) {
        return;
    }
  	// 监听门铃呼叫的通知
    [[NSNotificationCenter defaultCenter] addObserverForName:kTuyaDoorbellNotification object:nil queue:nil usingBlock:^(NSNotification *note) {
        NSDictionary *eventInfo = note.object;
        NSString *devId = eventInfo[@"devId"];
        NSString *eType = [eventInfo objectForKey:@"etype"];
        if ([eType isEqualToString:@"doorbell"]) {
            callback(devId, eType);
        }
    }];
}
```

Swift

```swift
func obserDoorbellCall(_ callBack: @escaping (String, String) -> Void) {
	  // 监听门铃呼叫的通知
    NotificationCenter.default.addObserver(forName: NSNotification.Name(rawValue: "kNotificationMQTTMessageNotification"), object: nil, queue: nil) { (noti) in
        if let eventInfo = noti.object as? [String: Any?] {
            let devId = eventInfo["devId"] as! String
            let eType = eventInfo["etype"] as! String
            if eType == "doorbell" {
                callBack(devId, eType)
            }
        }
    }
}
```

### 电池管理

低功耗门铃有两种供电方式，插电和电池供电。通过 IPC SDK 可以查询到设备当前的供电模式以及当前的电量。还可以设置一个低电量报警阈值，当电量过低时，会触发一个报警通知。

**示例代码**

ObjC

```objc
- (void)viewDidLoad {
		if ([self.dpManager isSupportDP:TuyaSmartCameraWirelessPowerModeDPName]) {
        TuyaSmartCameraPowerMode powerMode = [[self.dpManager valueForDP:TuyaSmartCameraWirelessPowerModeDPName] tysdk_toString];
        if ([powerMode isEqualToString:TuyaSmartCameraPowerModePlug]) {
						// 插电供电
        }else if ([powerMode isEqualToString:TuyaSmartCameraPowerModeBattery]) {
            // 电池供电
        }
        
    }
    
    if ([self.dpManager isSupportDP:TuyaSmartCameraWirelessElectricityDPName]) {
        NSInteger electricity = [[self.dpManager valueForDP:TuyaSmartCameraWirelessElectricityDPName] tysdk_toInt];
        NSLog(@"设备当前的电量为：%@%%", @(electricity));
    }
    
    if ([self.dpManager isSupportDP:TuyaSmartCameraWirelessLowpowerDPName]) {
        // 设置电量低于 20% 时，触发低电量警告
        [self.dpManager setValue:@(20) forDP:TuyaSmartCameraWirelessLowpowerDPName success:^(id result) {
            
        } failure:^(NSError *error) {
            // 网络错误
        }];
    }
    
    if ([self.dpManager isSupportDP:TuyaSmartCameraWirelessBatteryLockDPName]) {
        // 解除电池锁，以拆卸电池
        [self.dpManager setValue:@(NO) forDP:TuyaSmartCameraWirelessBatteryLockDPName success:^(id result) {
            
        } failure:^(NSError *error) {
            // 网络错误
        }];
    }
}
```

Swift

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    if self.dpManager.isSupportDP(.wirelessPowerModeDPName) {
        let powerMode = self.dpManager.value(forDP: .wirelessPowerModeDPName) as! String
        switch TuyaSmartCameraPowerMode(rawValue: powerMode) {
        case .plug: break
            // 插电供电
        case .battery: break
            // 电池供电
        default: break
        }
    }

    if self.dpManager.isSupportDP(.wirelessElectricityDPName) {
        let electricity = self.dpManager.value(forDP: .wirelessElectricityDPName) as! Int
        print("设备当前的电量为：", electricity)
    }

    if self.dpManager.isSupportDP(.wirelessLowpowerDPName) {
        // 设置电量低于 20% 时，触发低电量警告
        self.dpManager.setValue(20, forDP: .wirelessLowpowerDPName, success: { _ in

        }) { _ in
            // 网络错误
        }
    }

    if self.dpManager.isSupportDP(.wirelessBatteryLockDPName) {
        // 解除电池锁，以拆卸电池
        self.dpManager.setValue(false, forDP: .wirelessBatteryLockDPName, success: { _ in

        }) { _ in
            // 网络错误
        }
    }
}
```

