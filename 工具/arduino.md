# arduino 学习笔记

## 项目
1. 项目名字：ledDemo  led呼吸灯示例

## 学习过程
5. https://github.com/markszabo/IRremoteESP8266 可以下载这个给8266的红外库
    是不是还esp32 的红外库

4. 追踪web server的输出（web.h)

   #define WebPrintf(c, fmt, ...) 
   { char webBuff[192]; snprintf_P(webBuff, sizeof(webBuff), PSTR(fmt), ## __VA_ARGS__); (c)->print(webBuff); delay(10);}

   #define WebPrintfPSTR(c, fmt, ...) 
   { char webBuff[192]; snprintf_P(webBuff, sizeof(webBuff), (fmt), ## __VA_ARGS__); (c)->print(webBuff); delay(10);}

   参数c是一个对象，它就是:
    WiFiClient

3. Serial.print 用法：
    void setup() {
        Serial.begin(9600);
    }
    void loop() {
        // 在0号模拟输入插口读取值
        analogValue = analogRead(0);

        // 以多种格式输出
        Serial.println(analogValue);

        // 以ASCII编码十进制浮点值输出
        Serial.print(analogValue, DEC);

        // 以ASCII编码十进制浮点值输出
        Serial.println(analogValue, HEX);

        // 以ASCII编码十六进制输出
        Serial.println(analogValue, OCT);

        // 以ASCII编码八进制输出
        Serial.println(analogValue, BIN);

        // 以ASCII编码二进制输出
        //Serial.println(analogValue, BYTE);

        delay(10);
    }

2. 总结开发arduion项目步骤
    1. 安装(库---在Arduino/libraries目录下)
        (本机cf-nx2位置: ~/soft/arduino****)
    2. 设置

1. 如何使用指定的库
   <<Arduino IDE 查找和添加库文件>> https://jingyan.baidu.com/article/19192ad815730ee53e570797.html
    讲到了温度传感器DS18B20的库的添加方法
