# SSOCR 用法学习
## 资源
3. git 项目　
    https://github.com/auerswal/ssocr
2. 简价
    https://ourcodeworld.com/articles/read/741/how-to-recognize-seven-segment-displays-content-with-ssocr-seven-segment-optical-character-recognition-in-ubuntu-1604

1. gm工具
    http://www.charry.org/docs/linux/ImageMagick/ImageMagick.html

## 学习灵感
2. 确定相相和镜头
    
1. 备先相机和镜头
镜头：
https://item.taobao.com/item.htm?spm=a1z0d.7625083.1998302264.5.5c5f4e695BsTdw&id=18069146605
相机:
https://item.taobao.com/item.htm?spm=a230r.1.14.16.11642df2wEg6TS&id=18709094110&ns=1&abbucket=7#detail

## 知识点滴
14. ssocr 要处理的图像加载方法是：
    461: Imlib_Image image = imlib_load_image_with_error_return
    477   /* set the image we loaded as the current context image to work on */ 
 478   imlib_context_set_image(image); 
 479  
 480   /* get image parameters */ 
 481   w = imlib_image_get_width(); 
 482   h = imlib_image_get_height();


13. 相机获取图像缓存
    CameraGetImageBuffer(相机句柄, 贞信息指针，buffer的指针,整数)
    imgageProcess (是处理图像后得到 RGB888 格式数据)

12. 关于ssocr 
    定义了两个结构：
        3. 发现用opencv解决要大众化一点
        1. 数字结构 x1,y1,x2,y2 digit (数字在图像中的位置)
        2. 颜色结构 R,G,B,A
    函数列表:
         // 从stdin复制图像到一个临时文件, 并且返回一个文件名字
         static char * tmp_imgfile(unsigned int flags)
        
        // 返回前景像素的数量（一个扫描行的数量)
        static unsigned int scanline(Imlib_Image *image, Imlib_Image *debug_image,

         imgfile = argv[argc-1]; //最后一个参数是图像文件名
         if(strcmp("-", imgfile) == 0) /* read image from stdin? */ {
           if(flags & VERBOSE)
             fprintf(stderr, "using temporary file to hold data from stdin\n");
                        // 使用临时文件处理数据, 需要数据
           use_tmpfile = 1;
           imgfile = tmp_imgfile(flags); // 没有给文件参数，则用临时文件处理
         }

        *** 思考imgfile 读取到内存中后，再怎么处理
         image = imlib_load_image_with_error_return(imgfile, &load_error);


11. 关于ssocr chartset (想要小数点的字符集)
    charset_array  静态的变理字符集.
    函数init_charset(cs)用来初始化以下字符集
    函数print_digit(digit, flage) 
        if(!((c == '.') && (flags & OMIT_DECIMAL))) 
        
    cs的范围：CS_FULL (等)
            理想选择是 CS_HEXADECIMAL(含小数点的16HEX) 
    
    OMIT_DECIMAL 为１，才会print'

10. 读取视频的数据
//preview_thread 读取数据
        void * preview_thread(void* arg)
        {
            while(g_display_state == 1) //1 进行读取数据
            {   
                pthread_mutex_lock(&mutex);  //加互斥锁
                if(tmp_state==0){
                    read_data(Demo_display_window);
                }   
                pthread_mutex_unlock(&mutex); //解互斥锁
                usleep(1);
            }   
            pthread_exit(NULL);
            return ((void *)0);
        }

9. SDK  CameraGetCapability 相机的能力参数
        {pTriggerDesc = 0x616050, iTriggerDesc = 3, 
        pImageSizeDesc = 0x6160d0, iImageSizeDesc = 13,  //13种预设的分辨率模式

          pClrTempDesc = 0x617ce0, iClrTempDesc = 4, pMediaTypeDesc = 0x61dee0, iMediaTypdeDesc = 1,
          pFrameSpeedDesc = 0x616810, iFrameSpeedDesc = 3, pPackLenDesc = 0x0, iPackLenDesc = 0,
          iOutputIoCounts = 0, iInputIoCounts = 0, pPresetLutDesc = 0x615d80, iPresetLut = 4,
          iUserDataMaxLen = 256, bParamInDevice = 0, pAeAlmSwDesc = 0x0, iAeAlmSwDesc = 0,
          pAeAlmHdDesc = 0x0, iAeAlmHdDesc = 0, pBayerDecAlmSwDesc = 0x0, iBayerDecAlmSwDesc = 0,
          pBayerDecAlmHdDesc = 0x0, iBayerDecAlmHdDesc = 0, sExposeDesc = {uiTargetMin = 40,
            uiTargetMax = 160, uiAnalogGainMin = 8, uiAnalogGainMax = 120, fAnalogGainStep = 0.125,
            uiExposeTimeMin = 1, uiExposeTimeMax = 12288}, sResolutionRange = {iHeightMax = 1536,
            iHeightMin = 0, iWidthMax = 2048, iWidthMin = 0, uSkipModeMask = 5, uBinSumModeMask = 0,
            uBinAverageModeMask = 0, uResampleMask = 0}, sRgbGainRange = {iRGainMin = 0,
            iRGainMax = 400, iGGainMin = 0, iGGainMax = 400, iBGainMin = 0, iBGainMax = 400},
          sSaturationRange = {iMin = 0, iMax = 200}, sGammaRange = {iMin = 0, iMax = 1000},
          sContrastRange = {iMin = 0, iMax = 200}, sSharpnessRange = {iMin = 0, iMax = 100},
          sIspCapacity = {bMonoSensor = 0, bWbOnce = 1, bAutoWb = 0, bAutoExposure = 1,
            bManualExposure = 1, bAntiFlick = 1, bDeviceIsp = 0, bForceUseDeviceIsp = 0, bZoomHD = 0}}

        8. 相机开始工作
    CameraPlay(hCamera);

7. 设置分辨率
    tSdkImageResolution     sImageSize; // --> 可以引用(9中) Capabiblity.pImageSizeDesc[11]
     CameraSetImageResolution(hCamera,&sImageSize);
     ---------------
     CameraGetImageResolution(hCamera,&sResolution); // 获取查看方法

6. 设置相机的句柄
    int hCamera; 
    int iStatus = CameraInit(&tCameraEnumList[num],-1,-1,&hCamera);
    iStatus != CAMERA_STATUS_SUCCESS => 相机有问题                     

5. 设置相机曝光亮
    double t;
    CameraGetExposureTime(hCamera, &t); //当前曝光时间
    CameraSetExposureTime(hCamera, 799624); //设置曝光时间/*

         /*  的单位是按照行来计算的，因此，曝光时间并不能在微秒
            级别连续可调。而是会按照整行来取舍。这个函数的
            作用就是返回CMOS相机曝光一行对应的时间。
        */
            CameraGetExposureLineTime(g_hCamera, &m_fExpLineTime);

        /*
            设置曝光时间。单位为微秒。对于CMOS传感器，其曝光
            的单位是按照行来计算的，因此，曝光时间并不能在微秒
            级别连续可调。而是会按照整行来取舍。在调用
            本函数设定曝光时间后，建议再调用CameraGetExposureTime
            来获得实际设定的值。
        */
    CameraSetExposureTime(g_hCamera,iPos*m_fExpLineTime);

    //除以1000是换算成毫秒
    sprintf( buffer,"%0.3f",((iPos*m_fExpLineTime)/1000));



4. 项目使用该库的方法：
    qt pro 中加入库的引用 LIBS += -LMVSDK
    cpp 文件中头文件引用 INCLUDEPATH += /home/wmt/myProj/XiaDengMing/arm-charger/cam/taobaoPackage/MVSDK/include 
    cpp 执行设备枚举函数

3. 相机的linux SDK
    demo 安装位置 /XiangDengMin/cam/
    lib 安装位置  /lib64/libMVSDK.so

2. 使用
    ssocr -P -p -o 11-1 grayscale 11.jpg
ssocr -P -p --luminance=red  dynamic_threshold 95 98 -o 11-1  11.jpg
    ssocr -P -p -g  -o 11-1 gray_stretch 60 70 grayscale 11.jpg (这个好)   
     ssocr -P  -g -d 1 -b black -o 11-1 gray_stretch 60 70 grayscale 11.jpg (得到了１)
    ssocr -P  -g -d 1 -b black 11-1 (这个处理png 更快更保险)

   (裁剪后的彩图不作处理<gray-strach>还是会出错)
   (经过下面处理，就好了<不在本地输出文件应该也可以>)
   ssocr -P  -g -d 1 -b black -o 3-2.png gray_stretch 60 70 grayscale 3-1.jpg 
1. 初装过程：
        libX11-devel-1.6.7-alt1.x86_64.rpm
        1. rpm -ivh compat-libtiff3-3.9.4-11.el7.x86_64.rpm
        2. sudo yum install libimlib2.x86_64  libimlib2-devel.x86_64
        3. git clone https://github.com/auerswal/ssocr.git
        4. sudo mv ./ssocr /usr/local/bin/ssocr
