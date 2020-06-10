# Android_Joycon_Lib
a hidapi for joycon stick on android platform


After a long time dig. Finally i made android can communicate with switch joycon stick.


If you want try this. You need change ASOP,Then complier it, Flash it.



What you need to do is :

1.Open the hidraw file node (due to some unkone resone, Android close the /dev/hidraw* device file node, so you need open it.)

2.Change some file in Android linux kernel.(linux/hidraw.h   linux/hidraw.c)

>>You can find the different between liunx kernel and android liunx kernel in this two files.
  
>>Linux kernel :
  
>>>>https://github.com/torvalds/linux/blob/master/drivers/hid/hidraw.c#L456
  
>>>>https://github.com/torvalds/linux/blob/master/include/uapi/linux/hidraw.h#L42
  
>>Add this code to your ASOP 
  
>>File => private/msm-google/drivers/hid/hidraw.c
  ```java
  if (_IOC_NR(cmd) == _IOC_NR(HIDIOCGRAWUNIQ(0))) {
          int len = strlen(hid->uniq) + 1;
          if (len > _IOC_SIZE(cmd))
            len = _IOC_SIZE(cmd);
          ret = copy_to_user(user_arg, hid->uniq, len) ?
            -EFAULT : len;
          break;
        }
 ```  
 <img src="https://raw.githubusercontent.com/lyonlui/Android_Joycon_Lib/master/img/hidraw_c.jpg" width="350" height="315">

 >>File => private/msm-google/include/uapi/linux/hidraw.h
 ```c
 #define HIDIOCGRAWUNIQ(len)     _IOC(_IOC_READ, 'H', 0x08, len)
 ```
  <img src="https://raw.githubusercontent.com/lyonlui/Android_Joycon_Lib/master/img/hidraw_h.jpg" width="600" height="160">
