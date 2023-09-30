# USB_Keyboard

简单实现了USB-HID Keyboard的功能

1. **注意:要在usb_device.h文件中include进usb_hid.h文件**     
2. **在主文件(或者usb_device)中extern出USB的结构体**   
**extern USBD_HandleTypeDef hUsbDeviceFS;**

## **与USB-HID鼠标不同的地方**

1. CubeMX生成的USB-HID文件默认是配置USB鼠标的,需要修改如下文件:
   > 1. usbd_hid.c中的文件     
   > 
   >     __ALIGN_BEGIN static uint8_t HID_MOUSE_ReportDesc[HID_MOUSE_REPORT_DESC_SIZE] __ALIGN_END =   
        {   
        0x05,0x01,//使用情况页(通用桌面)  
        0x09,0x06,//用法(通用设备控件)   
        0xA1,0x01,//收集(申请) 
        0x05,0x07,//用法页(键盘/键盘)      
        0x19,0xE0,//最小使用量(按钮224)    
        0x29,0xE7,//最大使用量(按钮231)  
        0x15,0x00,//逻辑最小值(0)     
        0x25,0x01,//逻辑最大值(1)     
        0x75,0x01,//Report Size (1)    
        0x95,0x08,//Report Count (8)      
        0x81,0x02,//Input (Data，Var，Abs，NWrp，Lin，Pref，NNul，Bit)      
        0x95,0x01,//Report Count (1)      
        0x75,0x08,//Report Size (8)    
        0x81,0x01,//Input (Data，Var，Abs，NWrp，Lin，Pref，NNul，Bit)   
        0x95,0x05,//Report Count (5)      
        0x75,0x01,//Report Size (1)    
        0x05,0x08,//使用页面(LED)    
        0x19,0x01,//使用最少(按钮1)    
        0x29,0x05,//最大用量(按钮5)    
        0x91,0x02,//输出(Data，Var，Abs，NWrp，Lin，Pref，NNul，NVol，Bit)     
        0x95,0x01,//Report Count (1)   
        0x75,0x03,//Report Size (3)    
        0x91,0x01,//输出(Data，Var，Abs，NWrp，Lin，Pref，NNul，NVol，Bit)     
        0x95,0x06,//Report Count (6)      
   
   > 2. usbd_hid.h中的宏定义       
   > 
   > #define HID_MOUSE_REPORT_DESC_SIZE                 63U  //默认74U,改为63,keyboard描述符的字节共63    
   > 
   > #define HID_EPIN_SIZE                              0x08U    //8字节长度,键盘报文每次发送8byte数组

2. 发送报文需要一定时间来发送,建议使用阻塞式的while循环来等待其发送完毕
3. 报文具有自己的数据格式,可参考文件夹内的pdf官方文件