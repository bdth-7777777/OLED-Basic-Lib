# **OLED Basic Lib**

> **介绍：**一个**轻量级**的OLED屏幕驱动库，支持**中英文混合**显示，支持**动态可变区域**和**动态自适应帧率**刷新屏幕。
---
![OLED Basic Lib-1](https://github.com/user-attachments/assets/95e1ce45-b45f-44ca-9525-2468d29b0386)

---
* **Q : 这是什么？**
* **A :**  这是一个用于驱动OLED的C语言库，起源于[【 江协科技的OLED教程 】 ](https://www.bilibili.com/video/BV1EN41177Pc/?spm_id_from=333.1387.homepage.video_card.click&vd_source=1183a399479e248598095c5e770be232)，我在此基础上更改并适配了多种芯片的OLED，使用此库，您可以在您的项目当中方便地使用各种分辨率的OLED显示图片，图像，中英文混合字符串等。此外，[ OLED UI ](https://github.com/bdth-7777777/OLED_UI)是基于此库开发的。
---
* **Q : 有什么优势？**
* **A :** **优势如下：**  
    1. **便于移植：** 可以方便地移植进各种不同单片机的项目
    2. **支持中英文混合显示：** 就是 ~~一个字~~ 两个字，美观！
    3. **占用程序空间不多：** 与著名开源项目U8g2不同，虽然功能没有U8g2多，但是不会占用更多的程序空间，性价比高。
    4. **动态可变区域刷新屏幕：** 这算是此项目的一个特色了。您可以手动开启或关闭此功能。顾名思义，此功能能够使得在显存没有变化的时候不刷新屏幕，在显存变化的时候，只刷新有变化的对应页最小区块,从而节省资源。
---
* **Q : 为什么此项目叫“OLED Basic Lib”？只能给OLED屏幕使用吗？**
* **A :**  因为一开始此项目仅仅是为了给OLED屏幕使用的。此项目可以给所有支持按页显示的单色显示屏使用，比如LCD、OLED等。
---
## 当前支持情况
### OLED屏幕
   | 屏幕类型与分辨率  | 通讯协议 |屏幕主控 | STM32f103cxt6 |ESP32 DEV MODULE|
   |-------|-----|------|----|----| 
   | OLED 72x40    |   SPI  | SSD1315  |   有例程    |   有例程    |
   | OLED 72x40    |   IIC  | SSD1315  |   有例程    |   有例程    |
   | OLED 128x32   |  IIC   | SSD1306  |   有例程    |   有例程    |
   | OLED 128x32   |   SPI  | SSD1305  |   有例程    |   有例程    |
   | OLED 128x64   |   SPI  | SSD1306  |   有例程    |   有例程    |
   | OLED 128x64   |  IIC   | SSD1306  |   有例程    |   有例程    |
   | OLED 128x64   |  SPI   | SH1106   |   有例程    |   有例程    |
   | OLED 128*128  |  SPI   | SH1107   |   有例程    |   有例程    |
   | OLED 128*128  |  IIC   | SH1107   |   有例程    |   有例程    |
   | OLED 128*160  |  SPI   | SH1107   |   有例程    |   有例程    |
### LCD屏幕
   | 屏幕类型与分辨率  | 通讯协议 |屏幕主控 | STM32f103cxt6 |ESP32 DEV MODULE|
   |-------|-----|------|----|----| 
   | LCD 48x64（淘宝2.2元透明LCD）[链接](https://e.tb.cn/h.TFodxVo2dQGE5Ax?tk=OXwlePW9FBr)    |   SPI  | ST7565  |   有例程    |   有例程    |
   | LCD 128x64 （淘宝2.2元LCD）[链接](https://e.tb.cn/h.TFjmrZ7lSpX4zYu?tk=LzRqePWPPx6)      |   SPI  | ST7565  |   有例程    |   有例程    |
   | LCD 128x64 （淘宝1元LCD）[链接](https://e.tb.cn/h.TFjNBltaNEq0UXE?tk=bHhfePWlHvE)        |   SPI  | UC1701C  |   有例程    |   有例程    |
---
## 目录
- [实用函数介绍](#1-实用函数介绍)
- [移植部署方式](#2-移植部署方式)
- [可配置宏介绍](#3-可配置宏介绍)
- [文字以及图片取模教程](#4-文字以及图片取模教程)
---

## 1. 实用函数介绍

#### 1. `void OLED_Init(void);`
**介绍：** 初始化OLED屏幕，在程序开始的时候调用一次即可。

----
#### 2. `void OLED_Update(void);`
**介绍：** 刷新屏幕，将显存发送到屏幕

**补充：** 在 `OLED_driver.c` 文件的头部有一个宏  `IF_ENABLE_DYNAMIC_REFRESH` ，用户可以配置这个宏。如果用户将其配置为 `false` ,则不开启动态刷新，配置为 `true` 则开启动态刷新。

---
#### 3. `void OLED_UpdateArea(int16_t X, int16_t Y, int16_t Width, int16_t Height);`
**介绍：** 刷新屏幕，将显存发送到屏幕


**参数：**
- `X`：刷新区域的左上角横坐标
- `Y`：刷新区域的左上角纵坐标
- `Width`：刷新区域的宽度
- `Height`：刷新区域的高度

**补充：** 这个函数并不是精准刷新用户指定的区域，而是刷新最小的包含用户指定区域的页的最小区块。

---
#### 4. `void OLED_SetColorMode(bool colormode)`
**介绍：** 设置屏幕的颜色模式

**参数：**
- `colormode`：`true` 为黑底白字，`false` 为白底黑字

---
#### 5. `void OLED_Brightness(int16_t Brightness);`
**介绍：** 设置屏幕的亮度

**参数：**
- `Brightness`：亮度值，范围为0~255

---
#### 6. `bool OLED_IfChangedScreen(void);`
**介绍：** 检测显存数组是否发生变化

**返回值：** `true` 屏幕发生变化，`false` 屏幕未发生变化
**补充：** 此函数用于上层程序计算帧率。如果上一次刷屏之后显存没有变化，则此函数返回 `false`，否则返回 `true`。

---
#### 7. `void OLED_DelayMs(uint32_t xms);`
**介绍：** 就是一个普通的延时函数

**参数：**
- `xms`：延时时间，单位为毫秒

---

#### 8. `void OLED_Clear(void);`
**介绍：** 清空显存

---

#### 9. `void OLED_ClearArea(int16_t X, int16_t Y, int16_t Width, int16_t Height);`
**介绍：** 清空指定区域显存

**参数：**
- `X`：区域的左上角横坐标
- `Y`：区域的左上角纵坐标
- `Width`：区域的宽度
- `Height`：区域的高度

---

#### 10. `void OLED_Reverse(void);`
**介绍：** 将全屏显存数组取反

---
#### 11. `void OLED_ReverseArea(int16_t X, int16_t Y, int16_t Width, int16_t Height);`
**介绍：** 将指定区域的显存数组取反

**参数：**
- `X`：区域的左上角横坐标
- `Y`：区域的左上角纵坐标
- `Width`：区域的宽度
- `Height`：区域的高度

---

#### 12. `void OLED_ShowImage(int16_t X, int16_t Y, uint16_t Width, uint16_t Height, const uint8_t *Image);`
**介绍：** 显示图片

**参数：**
- `X`：图片左上角横坐标
- `Y`：图片左上角纵坐标
- `Width`：图片宽度
- `Height`：图片高度
- `Image`：图片数组指针

---

#### 13. `void OLED_Printf(int16_t X, int16_t Y, uint8_t Font,const char *format, ...);`
**介绍：** 格式化显示字符串
**参数：**
- `X`：字符串左上角横坐标
- `Y`：字符串左上角纵坐标
- `Font`：请使用预先定义的宏，他们的值就是字体的高:
    - `OLED_FONT_8`  
    - `OLED_FONT_12` 
    - `OLED_FONT_16` 
    - `OLED_FONT_20` 

- `format`：格式化字符串，支持%d、%c、%s等格式，同样的，它也支持`\n`换行符。

---

#### 14. `void OLED_ShowImageArea(int16_t X_Area, int16_t Y_Area, int16_t AreaWidth, int16_t AreaHeight, int16_t X_Pic, int16_t Y_Pic, int16_t PictureWidth, int16_t PictureHeight, const uint8_t *Image);`
**介绍：** 显示图片在指定区域

**参数：**
- `X_Area`：区域的左上角横坐标
- `Y_Area`：区域的左上角纵坐标
- `AreaWidth`：区域的宽度
- `AreaHeight`：区域的高度
- `X_Pic`：图片左上角横坐标
- `Y_Pic`：图片左上角纵坐标
- `PictureWidth`：图片宽度
- `PictureHeight`：图片高度
- `Image`：图片数组指针

**补充：** 这个函数可以将图片显示在指定区域，如果图片超出区域，则会自动裁剪。你可以理解为在图片的上方生成一个矩形框，然后将图片显示在矩形框内，如果图片超出矩形框，则不显示。
---

#### 15. `void OLED_PrintfArea(int16_t RangeX,int16_t RangeY,int16_t RangeWidth,int16_t RangeHeight,int16_t X, int16_t Y, uint8_t Font, char *format, ...); `
**介绍：** 格式化显示字符串在指定区域

**参数：**
- `RangeX`：区域的左上角横坐标
- `RangeY`：区域的左上角纵坐标
- `RangeWidth`：区域的宽度
- `RangeHeight`：区域的高度
- `X`：字符串左上角横坐标
- `Y`：字符串左上角纵坐标
- `Font`：请使用预先定义的宏，他们的值就是字体的高:
    - `OLED_FONT_8`  
    - `OLED_FONT_12` 
    - `OLED_FONT_16` 
    - `OLED_FONT_20` 

- `format`：格式化字符串，支持%d、%c、%s等格式，同样的，它也支持`\n`换行符。

**补充：** 这个函数可以将字符串显示在指定区域，如果字符串超出区域，则会自动裁剪。你可以理解为在字符串的上方生成一个矩形框，然后将字符串显示在矩形框内，如果字符串超出矩形框，则不显示。 

---
#### 16. `void OLED_DrawPoint(int16_t X, int16_t Y);`
**介绍：** 画点

**参数：**
- `X`：点的横坐标
- `Y`：点的纵坐标

---

#### 17. `uint8_t OLED_GetPoint(int16_t X, int16_t Y);`
**介绍：** 获取点是否有值

**参数：**
- `X`：点的横坐标
- `Y`：点的纵坐标

**返回值：** `0` 点无值，`1` 点有值

---

#### 18. `void OLED_DrawLine(int16_t X0, int16_t Y0, int16_t X1, int16_t Y1);`
**介绍：** 画线

**参数：**
- `X0`：线的起点横坐标
- `Y0`：线的起点纵坐标
- `X1`：线的终点横坐标
- `Y1`：线的终点纵坐标

---
#### 19. `void OLED_DrawRectangle(int16_t X, int16_t Y, int16_t Width, int16_t Height, uint8_t IsFilled);`
**介绍：** 画矩形

**参数：**
- `X`：矩形左上角横坐标
- `Y`：矩形左上角纵坐标
- `Width`：矩形宽度
- `Height`：矩形高度
- `IsFilled`：请使用宏，`OLED_FILLED` 填充矩形，`OLED_UNFILLED` 仅画边框
---

#### 20. `void OLED_DrawTriangle(int16_t X0, int16_t Y0, int16_t X1, int16_t Y1, int16_t X2, int16_t Y2, uint8_t IsFilled);`
**介绍：** 画三角形

**参数：**
- `X0`：三角形第一个顶点横坐标
- `Y0`：三角形第一个顶点纵坐标
- `X1`：三角形第二个顶点横坐标
- `Y1`：三角形第二个顶点纵坐标
- `X2`：三角形第三个顶点横坐标
- `Y2`：三角形第三个顶点纵坐标
- `IsFilled`：请使用宏，`OLED_FILLED` 填充三角形，`OLED_UNFILLED` 仅画边框

---

#### 21. `void OLED_DrawCircle(int16_t X, int16_t Y, int16_t Radius, uint8_t IsFilled);`
**介绍：** 画圆

**参数：**
- `X`：圆心横坐标
- `Y`：圆心纵坐标
- `Radius`：圆的半径
- `IsFilled`：请使用宏，`OLED_FILLED` 填充圆，`OLED_UNFILLED` 仅画边框

---

#### 22. `void OLED_DrawEllipse(int16_t X, int16_t Y, int16_t A, int16_t B, uint8_t IsFilled);`
**介绍：** 画椭圆

**参数：**
- `X`：椭圆中心横坐标
- `Y`：椭圆中心纵坐标
- `A`：椭圆长轴长度
- `B`：椭圆短轴长度
- `IsFilled`：请使用宏，`OLED_FILLED` 填充椭圆，`OLED_UNFILLED` 仅画边框

---

#### 23. `void OLED_DrawArc(int16_t X, int16_t Y, int16_t Radius, int16_t StartAngle, int16_t EndAngle, uint8_t IsFilled);`
**介绍：** 画圆弧

**参数：**
- `X`：圆心横坐标
- `Y`：圆心纵坐标
- `Radius`：圆的半径
- `StartAngle`：起始角度，单位为度
- `EndAngle`：终止角度，单位为度
- `IsFilled`：请使用宏，`OLED_FILLED` 填充圆弧，`OLED_UNFILLED` 仅画边框

---

#### 24. `void OLED_DrawRoundedRectangle(int16_t X, int16_t Y, int16_t Width, int16_t Height, int16_t Radius, uint8_t IsFilled);`
**介绍：** 画圆角矩形

**参数：**
- `X`：矩形左上角横坐标
- `Y`：矩形左上角纵坐标
- `Width`：矩形宽度
- `Height`：矩形高度
- `Radius`：矩形圆角半径
- `IsFilled`：请使用宏，`OLED_FILLED` 填充矩形，`OLED_UNFILLED` 仅画边框

---

## 2. 移植部署方式

   | 文件名  | 描述 | 移植的时候是否需要修改 |
   |-------|-----|------|
   | `OLED_Driver.c`  | 底层驱动文件         |   需要    |
   | `OLED_Driver.h`  | 底层驱动文件的头文件  |   需要    |
   | `OLED_Fonts.c`   | 字库文件          |   不需要  |
   | `OLED_Fonts.h`   | 字库文件的头文件  |   不需要  |
   | `OLED.c`         | 应用层文件        |   不需要  |
   | `OLED.h`         | 应用层文件的头文件 |   不需要  |

#### 1. `OLED_Driver.h` 文件
- 这个文件没啥要改的
#### 2. `OLED_Driver.c` 文件
- 修改屏幕像素的高度和宽度：
    ```c
    #define OLED_HEIGHT_DRIVER	        	(128)					//OLED像素的高度

    #define OLED_WIDTH_DRIVER		    	(128)					//OLED像素的宽度 
    ```
    把他们修改成你实际的OLED的高度和宽度。他们在oled底层驱动当中起关键作用。
- 修改有关SPI端口的宏定义或函数。例如在SPI的OLED_driver.c文件中，他们是这样的：
    ```c
    //使用宏定义，速度更快（寄存器方式）
    #define OLED_SCL_Clr()  (GPIOB->BRR = GPIO_Pin_8)   // 复位 SCL (将     GPIOB 的 8 号引脚拉低)
    #define OLED_SCL_Set()  (GPIOB->BSRR = GPIO_Pin_8)  // 置位 SCL (将     GPIOB 的 8 号引脚拉高)

    #define OLED_SDA_Clr()  (GPIOB->BRR = GPIO_Pin_9)   // 复位 SDA (将     GPIOB 的 9 号引脚拉低)
    #define OLED_SDA_Set()  (GPIOB->BSRR = GPIO_Pin_9)  // 置位 SDA (将     GPIOB 的 9 号引脚拉高)

    #define OLED_RES_Clr()  (GPIOB->BRR = GPIO_Pin_5)   // 复位 RES (将     GPIOB 的 5 号引脚拉低)
    #define OLED_RES_Set()  (GPIOB->BSRR = GPIO_Pin_5)  // 置位 RES (将     GPIOB 的 5 号引脚拉高)

    #define OLED_DC_Clr()   (GPIOB->BRR = GPIO_Pin_6)   // 复位 DC (将  GPIOB 的 6 号引脚拉低)
    #define OLED_DC_Set()   (GPIOB->BSRR = GPIO_Pin_6)  // 置位 DC (将  GPIOB 的 6 号引脚拉高)

    #define OLED_CS_Clr()   (GPIOB->BRR = GPIO_Pin_7)   // 复位 CS (将  GPIOB 的 7 号引脚拉低)
    #define OLED_CS_Set()   (GPIOB->BSRR = GPIO_Pin_7)  // 置位 CS (将  GPIOB 的 7 号引脚拉高)
    ```
    这些宏定义和函数是用来控制OLED的GPIO引脚的，根据你实际的OLED的引脚连接情况，修改这些宏定义和函数。

- 修改有关OLED的初始化函数。例如在OLED_Driver.c文件中，请找到 `OLED_Init()` 函数，修改它，正确的初始化GPIO引脚，使它能够正确初始化你的OLED。

- 修改`void OLED_DelayMs(uint32_t xms)` 这就是一个普通的延时函数。请在移植的时候正确地实现它。

#### 3. `OLED.h` 文件
- 修改OLED的高度和宽度，使得它与你实际的OLED的高度和宽度相匹配。
    ```c
    //使用宏定义的方式确定oled的横向像素与竖向像素
    #define OLED_WIDTH						(128)					
    #define OLED_HEIGHT 					(128)
    ```
#### 4. `OLED.c` 文件
- 这个文件没啥要改的。

#### 5. `OLED_Fonts.h` 文件
- 正常情况下，这个文件不需要修改。如果你添加的其他图片的字库，请在这里添加图片数组的声明，使得他们能够被上层调用。例如：
    ```c
    /*图像数据声明*/
    extern const uint8_t Arrow[];
    ```
#### 6. `OLED_Fonts.c` 文件
- 正常情况下，这个文件不需要修改。如果你添加的其他图片的字库，请在这里添加图片数组的定义，例如：
    ```c
    const uint8_t Arrow[] = {
    0x02,0x06,0x0E,0x0E,0x06,0x02,
    };/*"指示箭头",0*/
    ```

## 3. 可配置宏介绍

#### 1. `OLED_Driver.c` 文件
```c
#define IF_ENABLE_DYNAMIC_REFRESH       (false)
#define DYNAMIC_REFRESH_LENGHT          (16)   // 动态刷新区块的长度，单位为像素。
```
* `IF_ENABLE_DYNAMIC_REFRESH` 这个宏决定是否开启自适应刷新，`true`为开启。如果开启了自适应刷新，那么屏幕只会在内容变化的时候才刷新包含对应变化区域的最小刷新区块。
* `DYNAMIC_REFRESH_LENGHT` 这个宏配置了动态刷新区块的长度。请将其设置为能被屏幕宽度整除。

在这里，我略微细致地介绍一下这个动态刷新机制。首先，主流oled屏幕的显存是按页存储的，每页会有 **屏幕宽度** 乘 **8** 个像素。那么如何知道一个屏幕有多少页呢？非常简单，用 **屏幕宽度** 除以 **8** 即可得到屏幕的页数。在oled屏幕刷新的时候，发送数据的最小单位是1个字节，也就是8个比特。

动态刷新的核心就是对显存的各个区块计算当前的校验值，并与上一轮的校验值做对比。如果校验值不同，代表需要更新这个区块的内容，否则不更新该区块内容。`DYNAMIC_REFRESH_LENGHT ` 这个宏定义的值就是区块的宽度。如果您的屏幕是128*128，并且您的`DYNAMIC_REFRESH_LENGHT `定义为了128，那么整个屏幕的显存就会被分为16个区块。即使第一个区块只有一个像素点的值变化了，这个区块的所有位置都会被刷新。所以您可以适当地将这个宏设置得小一些，这样能够显著节省资源。

#### 2. `OLED.c` 文件
```c
/**关于字符串最大长度的宏，用于格式化输出字符串*/
#define  MAX_STRING_LENGTH   (128)
```
* `MAX_STRING_LENGTH` 这个宏的长度就是OLED_Printf函数能够打印的最大长度字符串所占的字节数。

```c
/**关于是否在绘制图像或是文字之前提前清除绘制区域显存的宏 */
#define IF_CLEAR_AREA        (true)
```
* `IF_CLEAR_AREA` 这个宏用于指定在绘制图片或者文字的时候是否需要先清空对应位置。

#### 3. `OLED.h` 文件
```c
/*字体大小参数取值*/
#define OLED_FONT_8                          (8)                   
// #define OLED_FONT_12                         (12) 
// #define OLED_FONT_16                         (16)
// #define OLED_FONT_20                         (20)
```

* 这几个宏表示字体的高度。注意，在这里还有一个特殊的使用方法：在您开启编译器优化的时候，如果您只需要使用其中一种高度的字体，那么您可以将不使用的字体高度注释掉，这样编译器就不会将您不需要的字体编译进最终的二进制文件。这样有助于节省程序空间。


```c
/**关于是否使用精简版vsprintf函数代替标准vsprintf函数的宏。如果开启此选项，代码体积将减少约5kb，有利于小型嵌入式系统**/
#define USE_SIMPLE_VSPRINTF   (true)
```
* `USE_SIMPLE_VSPRINTF` 这个宏用于指定是否需要使用精简版的vsprintf函数。如果使用精简版，程序的code部分体积可以减小约 **5kb** 。

## 4. 文字以及图片取模教程
#### 1. 英文字母取模
* 英文字母无需取模
#### 2. 中文取模
* 下载仓库，解压 `取模软件与辅助工具` 压缩包，运行 `PCtoLCD2002.exe` 程序。
![image-1](https://github.com/user-attachments/assets/47df19fa-8cf5-47d2-866f-1fa24dac09ca)

* 点击设置，确保选项与下图一致（选择c51，然后点击确认按钮以保存设置）：
![image-2](https://github.com/user-attachments/assets/bc5cf8e5-806d-4b05-8b2f-bde34148b107)


* 下图①处数据为字模的数据大小，②处为字模的字体大小。在输入框当中输入需要用到的字体，点击生成字模按钮。

![image-3](https://github.com/user-attachments/assets/aa4c1ed6-81a1-4a87-be28-682539231f9e)

生成的字模是这样的：

    ```c
    {0x00,0x80,0x60,0xF8,0x07,0x40,0x20,0x18,0x0F,0x08,0xC8,0x08,0x08,0x28,0x18,0x00,0x01,0x00, 0x00,0xFF,0x00,0x10,0x0C,0x03,0x40,0x80,0x7F,0x00,0x01,0x06,0x18,0x00},/*"你",0*/
    {0x10,0x10,0xF0,0x1F,0x10,0xF0,0x00,0x80,0x82,0x82,0xE2,0x92,0x8A,0x86,0x80,0x00,0x40,0x22, 0x15,0x08,0x16,0x61,0x00,0x00,0x40,0x80,0x7F,0x00,0x00,0x00,0x00,0x00},/*"好",1*/
    {0xFC,0x04,0xFC,0x00,0xFE,0x42,0xBE,0x00,0xF2,0x12,0xF2,0x02,0xFE,0x02,0x00,0x00,0x0F,0x04, 0x0F,0x00,0xFF,0x10,0x0F,0x00,0x0F,0x04,0x4F,0x80,0x7F,0x00,0x00,0x00},/*"啊",2*/
    ```
    你需要将这些字模数据复制到 `OLED_Fonts.c` 文件中，并将其格式改为这样：
    ```c
    {{"你"},
    {0x20,0x10,0xFC,0x03,0x10,0xCF,0x04,0xF4,0x04,0x54,0x8C,0x00,0x00,0x00,0x0F,0x00,0x02,0x01, 0x08,0x0F,0x00,0x00,0x03,0x00}},
    {{"好"},
    {0x88,0x78,0x0F,0x88,0x78,0x42,0x42,0xF2,0x4A,0x46,0x40,0x00,0x08,0x05,0x02,0x05,0x08,0x00, 0x08,0x0F,0x00,0x00,0x00,0x00}},
    ...
    ```


格式正确的字模才会被正确处理。你也许会觉得手动改格式会有些麻烦。没关系，我们有自动化的脚本来帮助你完成这个工作。

解压 `取模软件与辅助工具` 压缩包，运行 `OLED_UI字模格式修改器.exe` 程序。

![image-4](https://github.com/user-attachments/assets/f28605f1-1995-45a6-9850-bbfcdf7c0f53)

将原始的字模数据复制到输入区域。点击转换，就可以获取到格式正确的字模数据。

#### 3. 图片取模
* 下载仓库，解压 `取模软件与辅助工具` 压缩包，运行 `Img2Lcd.exe` 程序，按照下图步骤操作并保存

![image-5](https://github.com/user-attachments/assets/3b39e9eb-de35-4153-ad33-4856dc34680e)

* 再次打开 `Img2Lcd.exe` 程序，在 `模式` 选择图形模式，在 `文件` 打开刚才生成bmp格式的图片。

![image-6](https://github.com/user-attachments/assets/aaaf28b3-e09e-4297-97fb-4ca4ac460019)

在此界面可以编辑图片，鼠标左键点亮像素，右键取消点亮像素。然后点击生成字模，复制生成的字模数据到 `OLED_Fonts.c` 文件中，并将其格式改为这样：

    ```c
    const uint8_t OLED_UI_LOGO[] = { 
    0x00,0x00,...
    };/*128x60*/

    ```
然后在`OLED_Fonts.h`文件内声明这个图片数组，以便于调用。

    ```c
    extern const uint8_t OLED_UI_LOGO[];
    ```
这样，你就可以在你的程序中调用这个图片数组了。
