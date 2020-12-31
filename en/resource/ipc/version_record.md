# Change Log

## Document change log

| Version | Description | Author | Date       |
| ------- | ----------- | ------ | ---------- |
| v1.0.0  | Create      | FuLang | 2020-03-01 |
| v1.1.0  | Update      | FuLang | 2020-06-05 |
| v1.1.1  | Update      | FuLang | 2020-07-31 |
| v1.2.0  | Update      | FuLang | 2020-11-14 |
| v1.3.0  | Update      | FuLang | 2020-12-30 |



## SDK change log

### 3.22.0 (2020-12-30)

| 模块                | 版本   |
| ------------------- | ------ |
| TuyaCameraSDK       | 3.22.0 |
| TuyaSmartCameraBase | 4.22.0 |
| TuyaSmartCameraM    | 4.22.0 |
| TuyaSmartCameraT    | 4.22.0 |
| TuyaSmartCameraKit  | 4.22.0 |
| TYEncryptImage      | 4.22.0 |

**更新说明**

* The camera object creation process is simplified.
* Sound playback provides switching between speaker and earpiece playback modes.
* Add the multiple playback method of the memory card playback video.
* Add cloud storage download and delete methods.
* Supports direct download of encrypted pictures.

### 3.20.0 (2020-11-14)

| 模块                | 版本   |
| ------------------- | ------ |
| TuyaCameraSDK       | 3.20.1 |
| TuyaSmartCameraBase | 4.20.0 |
| TuyaSmartCameraM    | 4.20.0 |
| TuyaSmartCameraKit  | 4.20.0 |
| TuyaCameraUIKit     | 3.21.0 |

**New Features**

* The original alarm video playback interface is deprecated, and the `TuyaSmartCameraMessageMediaPlayer` class is added to play the attachment of the alarm message.
* The interface for getting and setting definition is updated.
* Added a live video playback interface with specified definition.
* Added object outline function.
* Added timeline UI commponent.
* Detect message and cloud event added enctrpted image enable interface.

### TYEncryptImage-3.20.0

**New Features**

* After importing the TYEncryptImage component, the data obtained from the image url path carried in the alarm message and cloud storage event will become an encrypted image and cannot be viewed directly. You need to use the interface provided by the TYEncryptImage component to load the image.

### 3.17.0 (2020-06-05)

| Module              | Version |
| ------------------- | ------- |
| TuyaCameraSDK       | 3.17.3  |
| TuyaSmartCameraBase | 4.17.0  |
| TuyaSmartCameraM    | 4.17.3  |
| TuyaSmartCameraT    | 4.17.6  |
| TuyaSmartCameraKit  | 4.17.1  |

**New Features**

* P2P communication library upgrade, improve connection speed and stability.
* The open api for request p2p config data needs to be updated to `tuya.m.rtc.session.init`, the version is `1.0`.

### 3.15.0 (2020-02-21)

| Module              | Version |
| ------------------- | ------- |
| TuyaCameraSDK       | 3.15.3  |
| TuyaSmartCameraBase | 4.4.2   |
| TuyaSmartCameraM    | 4.3.4   |
| TuyaSmartCameraKit  | 4.4.4   |

**New Features**

* TuyaCamera is renamed TuyaCameraSDK. If you have specified the TuyaCamera version number in the podfile, you need to change the library name and version.
* Removed some classes and interfaces that have not been standardized (not described in the documentation).
* Alarm messages and pictures in cloud storage events are changed to unencrypted.
* Fixed some stability issues.

### 3.13.3 (2019-12-20)

| Module              | Version |
| ------------------- | ------- |
| TuyaSmartCameraBase | 4.2.6   |
| TuyaSmartCameraM    | 4.2.6   |
| TuyaCamera          | 3.13.4  |

**New Features**

* Add interface to get audio data of talk, provide to development process the audio.

### 3.13.0 (2019-12-05)

| Module              | Version |
| ------------------- | ------- |
| TuyaSmartCameraBase | 4.2.5   |
| TuyaSmartCameraM    | 4.2.5   |
| TuyaSmartCameraKit  | 4.3.2   |
| TuyaCamera          | 3.13.3  |

**New Features**

* The P2P and audio and video codec libraries are independent TuyaCamera components, and they are changed to dynamic library  to resolve the conflict of different library versions.
* Add interfaces for recording, screenshots, and saving to a specified file path.

### 3.10.0 (2019-10-25)

| Module                       | Version |
| ---------------------------- | ------- |
| TuyaSmartCameraBase          | 4.0.2   |
| TuyaSmartCameraM             | 4.0.2   |
| TuyaSmartCameraKit           | 4.0.1   |
| TYCameraCloudServicePanelSDK | 0.2.1   |
| TuyaSmartLogger              | 0.1.0   |

