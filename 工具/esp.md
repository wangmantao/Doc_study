#ESP IOC 应用
##资源. 
   1. << How to use ESP32 Dual Core with Arduino IDE >>
    https://randomnerdtutorials.com/esp32-dual-core-arduino-ide/
   2. << Getting Started with ESP32 Bluetooth Low Energy (BLE) on Arduino IDE >>
    https://randomnerdtutorials.com/esp32-bluetooth-low-energy-ble-arduino-ide/
   3. 官方资料下载
   https://www.espressif.com/en/support/download/documents

https://docs.espressif.com/projects/esp-idf/zh_CN/latest/get-started/index.html
https://docs.espressif.com/projects/esp-idf/en/latest/api-guides/jtag-debugging/index.html

## 
9. 上次进行的内容不知道在哪了
    arduino 命令　打开了 ledDemo
    (在myProj/wmt/eps32/led/ledDemo)
    见以下第３项：
        作用：知道这个软件的用法
            1. 打开的项目文件是什么
            2. 怎么完成編译
            3. 主要有哪些头文件，要不要关心

8. 中文的git项目：
  https://github.com/lixy123/ESP32-AUDIO-REC
   spissf  psram inmp441 
   接线：sck io26 / ws 33 /sd 34/ LR gnd/ LED 27
  
   接下来加载程序，运行，看SD SCK的波形

7. git demo 实验
    https://github.com/maspetsberger/esp32-i2s-mems
    
6. ESP32 学习笔记（七）I2S - Inter—IC Sound
    https://blog.csdn.net/qq_27114397/article/details/81611917

5. 查看esp-idf (esp 开发环境<SDK>) 版本
    cd $IDF_PATH
    git describe --tags --dirty 
        v3.1-dev-443-g17e8d49f
4. i2s 示例
    /home/wmt/myProj/wmt/esp32/esp-idf/examples/peripherals/i2s

3. arduino 添加esp32硬件的支持
    in hardware dir new espressif,
    in espressif dir git clone https://github.com/espressif/arduino-esp32.git  (时间久)
    hardware\espressif\esp32\tools\get 运行
    IDE tool 菜单　开发板-> esp dev module
    https://www.jianshu.com/p/ee6145286a22 示例一个呼吸灯

2. 项目构买清单
    杜邦线
    面包板
    电池盒(锂电池？)

1. esp32 vs. esp8266
https://makeradvisor.com/esp32-vs-esp8266/
同:    
    32-bit processor. 
    GPIO 实现protocols like SPI, I2C, UART, and more. 
    wireless networking
异：
        The ESP32 is dual core 160MHz to 240MHz CPU 
        the ESP8266 is a single core processor that runs at 80MHz.

        esp32 : bluetooth
                hall effect sensor
                touch sensitive pins that can be used to wake up the ESP32 

        [ SPI/I2C/I2S/UART ] esp8266 : esp32
        2/1/2/2 : 4/2/2/2
        [ ADC ]
        10-bit :  12-bit

         [ esp32 GPIO layout ]
         https://makeradvisor.com/wp-content/uploads/2018/04/ESP32-DOIT-DEVKIT-V1-Board-Pinout-36-GPIOs-Copy.jpg
