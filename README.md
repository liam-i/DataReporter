## DataReporter

[![name](https://img.shields.io/badge/release-1.5.2-green?style=flat)]()
[![license](https://img.shields.io/badge/license-MIT-brightgreen?style=flat)]()
[![platform](https://img.shields.io/badge/Platform-Android%20%7C%20iOS-red?style=flat)]()

(中文版本请参看[这里](#data_reporter_cn))

DataReporter is a cross-platform infrastructure component developed by luojisiwei dedao ebook Team.
It is proved to be effective by tens of millions of dedao users.

1. Cross platform, easy to deploy if you are developing multi-platform or multi-business application.
2. Suitable for Real-time reporting of data.
3. Anti-data loss.
4. Orderly reporting of data.
5. Mobile platform friendly, low power.

## Samples
Start with sample usage [here](https://github.com/luojilab/DataReporter/tree/master/sample).

## Getting started
Choose [Android](#android) or [iOS](#apple)

### <a name="android">[Android]()</a>
## Use AAR
1. Join to the build.gradle where in the project root directory
repositories {
        mavenCentral()
    }
2. Join to the build.gradle where in the project directory
implementation 'io.github.luojilab:datareporter:1.5.2'
3. Access according to the call mode of the demo

## Source Compile
Preparation Condition:
1. AndroidStudio3.1 And above
2. android-ndk-r14b And above

step:
1. Use AndroidStuido open build/android in the source path
2. open File menu on AndroidStudio and build Structure menu
3. config the NDK path on the Android NDK location
4. run the datareporter release compile task
5. the Generated datareporter-release.aar use for your build

## Way of use

    /**
     * Create a Reporter instance, the instance is native, saved in the returned int value
     * @param uuid      Business unique id, defined by the user as a unique id
     * @param cachePath Cache path, must be globally unique, different paths for different services
     * @param encryptKey Cache Encrypt Key
     * @param reportImp Reporting implementation interface
     * @return
     */
    public static native long makeReporter(String uuid, String cachePath, String encryptKey, IReport reportImp);
    
    /**
     * Set the number of data to be reported in a single time, and call it before start
     *
     * @param nativeReporter return by makeReporter
     * @param count          Number of data reported at one time
     */
    public static native void setReportCount(long nativeReporter, int count);
    
    /**
     * Set the maximum size limit of the cache file single file, the default value is 10k
     *
     * @param nativeReporter return by makeReporter
     * @param fileMaxSize    Cache file maximum size
     */
    public static native void setFileMaxSize(long nativeReporter, int fileMaxSize);
    
    /**
     * Set the cache file validity period
     *
     * @param nativeReporter return by makeReporter
     * @param expiredTime    Data validity period
     */
    public static native void setExpiredTime(long nativeReporter, long expiredTime);
    
     /**
     * Set the reporting interval
     *
     * @param nativeReporter return by makeReporter
     * @param reportingInterval   Reporting interval
     */
    public static native void setReportingInterval(long nativeReporter, long reportingInterval);
    
    /**
     * Start reporting thread After setting the relevant parameters, execute it once.
     *
     * @param nativeReporter return by makeReporter
     */
    public static native void start(long nativeReporter);
    
    /**
     * Wake up the report, call when the network status is good or other wake up scene call
     *
     * @param nativeReporter return by makeReporter
     */
    public static native void reaWaken(long nativeReporter);


    /**
     * Data reporting call interface
     *
     * @param nativeReporter return by makeReporter
     * @param data           Data to be reported
     */
    public static native void push(long nativeReporter, byte[] data);
    
    /**
     * Call this method after the report is successful
     *
     * @param nativeReporter return by makeReporter
     * @param key            The value returned by the escalation implementation interface
     */
    public static native void uploadSucess(long nativeReporter, long key);
    
    /**
     * Call this method after the report fails
     *
     * @param nativeReporter return by makeReporter
     * @param key            The value returned by the escalation implementation interface
     */
    public static native void uploadFailed(long nativeReporter, long key);


    /**
     * Release the report instance, other methods can not be called after release
     *
     * @param nativeReporter return by makeReporter
     */
    public static native void releaseReporter(long nativeReporter);
## Note: After calling this method, you can not call any of the above methods. Because the instance has been released

## Confused configuration
-keep class com.iget.datareporter.\*\*{\*;}

### <a name="apple">[iOS]()</a>

## use static library
1. Copy the static library file corresponding to the architecture under release\iOS to the application build.
2. Refer to the following call mode to access

Preparation Condition:
1. xcode last version
2. cmake last version

step:
1. Use the terminal to enter the build\iOS directory in the build.
2. Compile the real machine library and execute the static library file generated after the build_os_arch.sh execution. libdata-reporter.a in the build folder.
3. Compile the emulator library to execute the build_x86_64.sh static library file generated after execution in the build folder libdata-reporter.a
4. Generate debug build execution generate_project.sh will generate debug xcode build after execution, build file in build folder

## way of use

    /**
     *  Create a Reporter instance
     *  @param uuid      Business unique id, defined by the user as a unique id
     *  @param cachePath Cache path, must be globally unique, different paths for different services
     *  @param encryptKey Cache Encrypt Key
     *  @param reportImp Reporting implementation interface
     *  @return
     */
    (void *)MakeReporter:(NSString *)uuid cachePath:(NSString *)cachePath encryptKey:(NSString *)encryptKey uploadBlock:(void(^)(int64_t key, NSArray *dataArray))uploadBlock;
  

    /**
     *  Set the number of data to be reported in a single time, and call it before start
     *  @param nativeReporter The value returned by makeReporter
     *  @param count          Number of data reported at one time
     */	
    (void)SetReportCount:(void *)nativeReporter count:(NSInteger)count;

    /**
     *Set the maximum size limit of the cache file single file, the default value is 10k
     *  @param nativeReporter The value returned by makeReporter
     *  @param fileMaxSize    Cache file maximum size
     */				 
    (void)SetFileMaxSize:(void *)nativeReporter fileMaxSize:(NSUInteger)fileMaxSize;

    /**
     *  Set the cache effective time in seconds
     *  @param nativeReporter The value returned by makeReporter
     *  @param expiredTime    Validity period is the unit
     */
    (void)SetExpiredTime:(void *)nativeReporter expiredTime:(int64_t)expiredTime;

    /**
     *  Set the reporting interval
     *  @param nativeReporter The value returned by makeReporter
     *  @param reportingInterval    Report interval, in seconds
     */
    (void)SetReportingInterval:(void *)nativeReporter reportingInterval:(int64_t)reportingInterval;
 
    /**
     *  Start reporting thread After setting the relevant parameters, execute it once.
     *
     *  @param nativeReporter The value returned by makeReporter
     */
    (void)Start:(void *)nativeReporter;

    /**
     *  Wake up the report, call when the network status is good or other wake up scene call
     *
     *  @param nativeReporter The value returned by makeReporter
     */		   
    (void)ReaWaken:(void *)nativeReporter;

    /**
     *  Data reporting call interface
     *
     *  @param nativeReporter The value returned by makeReporter
     *  @param data           Data to be reported
     */

    (void)Push:(void *)nativeReporter byteArray:(NSData *)byteArray;

    /**
     *  Call this method after the report is successful
     *
     *  @param nativeReporter The value returned by makeReporter
     *  @param key            The value returned by the escalation implementation interface
     */
    (void)UploadSucess:(void *)nativeReporter key:(NSInteger)key;

    /**
     *  Call this method after the report fails
     *
     *  @param nativeReporter The value returned by makeReporter
     *  @param key            The value returned by the escalation implementation interface
     */
    (void)UploadFailed:(void *)nativeReporter key:(NSInteger)key;

    /**
     *  Release the report instance, other methods can not be called after release
     *
     *  @param nativeReporter The value returned by makeReporter
     */
    (void)ReleaseReporter:(void *)nativeReporter;

## Note: After calling this method, you can no longer call any of the above methods. Because the instance has been released

## <a name="data_reporter_cn">DataReporter</a>

DataReporter 是基于mmap的数据实时上报组件
主要用于数据的高性能实时上报，网络情况不好时，数据会缓存到本地。网络状态转好，马上上报。保证数据顺序上报。防数据丢失，即使应用发生崩溃，缓存数据不会丢失。

## 特点：
1. 跨平台，DataReporter是跨平台实现，适用于Android和iOS。
2. 准实时上报，网络状态好，直接上报，网络状态不好在网络转好后上报，同时上报顺序不乱序。
3. 不丢数据，采用mmap实现缓冲区。即使发生应用crash或者进程被杀，数据不会丢失，下次启动会再次上报。
4. 低功耗，采用c++实现，整个上报共用一个线程，多实例多业务场景共用同一线程，当数据全部上报完成后，线程休眠。
5. 高效率，可配置一次上报数据条数。增加上报效率。有效利用网络。减少链接次数。同时采用native方式实现，性能方面更优秀。

## 原理
数据通过调用接口进入DataReporter，先缓存到数据缓冲区，缓冲区实现是mmap，即使发生crash数据不丢失。然后启动线程进行上报。上报开始调用上报接口。成功后调用者通知上报完成，开始进行下一批数据上报。当上层调用过快，缓冲区数据达到指定阈值，数据开始下沉写入文件。写文件缓冲区同样采用mmap实现，防止数据丢失。上报优先级是先取文件数据，然后再取内存数据。保证数据顺序与调用顺序相同。

demo 的使用请参考[这里](https://github.com/luojilab/DataReporter/tree/master/sample)。

## 开始接入

接入 [Android](#android_cn) 或者 [iOS](#apple_cn)

### <a name="android_cn">[Android]()</a>
## aar接入
1. 在项目跟目录build.gradle中加入
repositories {
        mavenCentral()
    }
2. 在项目build.gradle中加入
implementation 'io.github.luojilab:datareporter:1.5.2'
3. 按照demo的调用方式接入

## 源码编译
准备条件：
1. AndroidStudio3.1及以上版本
2. android-ndk-r14b及以上版本

步骤：
1. AndroidStuido打开源码路径project/android AndroidStuido项目文件
2. 打开AndroidStudio中File菜单，Project Structure菜单
3. 在Android NDK location中配置NDK位置
4. 在AndroidStuido中的Gradle菜单中找到位于Android菜单下的datareporter，在Tasks中找到build，选择assembleRelease进行编译。
5. 生成datareporter-release.aar 导入到项目中进行使用

## 调用方式

    /**
     * 创建Reporter实例，实例是native的，保存在返回的int值中
     *
     * @param uuid      业务唯一id,由使用者定义一个唯一id
     * @param cachePath 缓存路径，必须全局唯一，不同业务不同路径
     * @param encryptKey 缓存加密key
     * @param reportImp 上报实现接口
     * @return
     */
    public static native long makeReporter(String uuid, String cachePath, String encryptKey, IReport reportImp);
    
    /**
     * 设置单次上报的数据条数，在start之前调用
     *
     * @param nativeReporter 由makeReporter返回的值
     * @param count          一次上报的数据条数
     */
    public static native void setReportCount(long nativeReporter, int count);
    
    /**
     * 设置缓存文件单文件最大大小限制，默认值为10k
     *
     * @param nativeReporter 由makeReporter返回的值
     * @param fileMaxSize    缓存文件最大大小
     */
    public static native void setFileMaxSize(long nativeReporter, int fileMaxSize);
    
    /**
     * 设置缓存文件有效期
     *
     * @param nativeReporter 由makeReporter返回的值
     * @param expiredTime    数据有效期
     */
    public static native void setExpiredTime(long nativeReporter, long expiredTime);
    
     /**
     * 设置上报间隔
     *
     * @param nativeReporter 由makeReporter返回的值
     * @param reportingInterval   上报间隔
     */
    public static native void setReportingInterval(long nativeReporter, long reportingInterval);
    
    /**
     * 开始上报线程 设置完相关参数之后，执行一次。
     *
     * @param nativeReporter 由makeReporter返回的值
     */
    public static native void start(long nativeReporter);
    
    /**
     * 唤醒上报，在网络状态转好时调用或者其他想唤醒场景调用
     *
     * @param nativeReporter 由makeReporter返回的值
     */
    public static native void reaWaken(long nativeReporter);


    /**
     * 数据上报调用接口
     *
     * @param nativeReporter 由makeReporter返回的值
     * @param data           需要上报的数据
     */
    public static native void push(long nativeReporter, byte[] data);
    
    /**
     * 上报成功后调用该方法
     *
     * @param nativeReporter 由makeReporter返回的值
     * @param key            由上报实现接口返回的值
     */
    public static native void uploadSucess(long nativeReporter, long key);
    
    /**
     * 上报失败后调用该方法
     *
     * @param nativeReporter 由makeReporter返回的值
     * @param key            由上报实现接口返回的值
     */
    public static native void uploadFailed(long nativeReporter, long key);


    /**
     * 释放上报实例，释放后其他方法都不可以调用
     *
     * @param nativeReporter 由makeReporter返回的值
     */
    public static native void releaseReporter(long nativeReporter);
## 注意：调用该方法后，不可以再调用以上任何方法。因为实例已经释放

## 混淆配置
-keep class com.iget.datareporter.\*\*{\*;}


### <a name="apple_cn">[iOS]()</a>

## 库接入
1. 拷贝release\iOS下对应架构的静态库文件导入到应用项目
2. 参照下面的调用方式接入

## 源码编译
准备条件：
1. xcode最新版本
2. cmake最新版本

步骤：
1. 使用终端进入项目中project\iOS目录
2. 编译真机版本库执行 build_os_arch.sh 执行后生成的静态库文件在build文件夹下libdata-reporter.a
3. 编译模拟器版本库执行 build_x86_64.sh 执行后生成的静态库文件在build文件夹下libdata-reporter.a
4. 生成调试工程执行 generate_project.sh 执行后会生成调试的xcode工程，工程文件在build文件夹下

## 调用方式

    /**
     *  创建Reporter实例
     *
     *  @param uuid      业务唯一id,由使用者定义一个唯一id
     *  @param cachePath 缓存路径，必须全局唯一，不同业务不同路径
     *  @param encryptKey 缓存加密key
     *  @param reportImp 上报实现接口
     *  @return
     */
    (void *)MakeReporter:(NSString *)uuid cachePath:(NSString *)cachePath encryptKey:(NSString *)encryptKey uploadBlock:(void(^)(int64_t key, NSArray *dataArray))uploadBlock;	

    /**
     *  设置单次上报的数据条数，在start之前调用
     *
     *  @param nativeReporter 由makeReporter返回的值
     *  @param count          一次上报的数据条数
     */	
     (void)SetReportCount:(void *)nativeReporter count:(NSInteger)count;

    /**
     *  设置缓存文件单文件最大大小限制，默认值为10k
     *
     *  @param nativeReporter 由makeReporter返回的值
     *  @param fileMaxSize    缓存文件最大大小
     */				 
     (void)SetFileMaxSize:(void *)nativeReporter fileMaxSize:(NSUInteger)fileMaxSize;

    /**
     *  设置缓存有效时间，秒为单位
     *
     *  @param nativeReporter 由makeReporter返回的值
     *  @param expiredTime    有效期 秒为单位
     */
     (void)SetExpiredTime:(void *)nativeReporter expiredTime:(int64_t)expiredTime;

    /**
     *  设置上报间隔
     *
     *  @param nativeReporter 由makeReporter返回的值
     *  @param reportingInterval    上报间隔，秒为单位
     */
     (void)SetReportingInterval:(void *)nativeReporter reportingInterval:(int64_t)reportingInterval;

    /**
     *  开始上报线程 设置完相关参数之后，执行一次。
     *
     *  @param nativeReporter 由makeReporter返回的值
     */
    (void)Start:(void *)nativeReporter;

    /**
     *  唤醒上报，在网络状态转好时调用或者其他想唤醒场景调用
     *
     *  @param nativeReporter 由makeReporter返回的值
     */		   
    (void)ReaWaken:(void *)nativeReporter;

    /**
     *  数据上报调用接口
     *
     *  @param nativeReporter 由makeReporter返回的值
     *  @param data           需要上报的数据
     */
    (void)Push:(void *)nativeReporter byteArray:(NSData *)byteArray;

    /**
     *  上报成功后调用该方法
     *
     *  @param nativeReporter 由makeReporter返回的值
     *  @param key            由上报实现接口返回的值
     */
    (void)UploadSucess:(void *)nativeReporter key:(NSInteger)key;


    /**
     *  上报失败后调用该方法
     *
     *  @param nativeReporter 由makeReporter返回的值
     *  @param key            由上报实现接口返回的值
     */
    (void)UploadFailed:(void *)nativeReporter key:(NSInteger)key;

    /**
     *  释放上报实例，释放后其他方法都不可以调用
     *
     *  @param nativeReporter 由makeReporter返回的值
     */
    (void)ReleaseReporter:(void *)nativeReporter;

## 注意：调用该方法后，不可以再调用以上任何方法。因为实例已经释放

## Support

还有其他问题？

1. 参看 [DataReporter/sample](https://github.com/luojilab/DataReporter/tree/master/sample)；
2. 阅读 [源码](https://github.com/luojilab/DataReporter/tree/master/src)；
3. 阅读 [wiki](https://github.com/luojilab/DataReporter/wiki)；
4. 联系我们。得到客户端技术组 QQ:21314601

## 致谢：
感谢腾讯团队开源的MMKV项目。本项目借鉴了部分源码。
感谢开源。

## License
DataReporter 使用的 MIT 协议，详细请参考 [LICENSE](https://github.com/luojilab/DataReporter/blob/master/LICENSE.md)。