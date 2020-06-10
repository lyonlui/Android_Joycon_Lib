# Android_Joycon_Lib
a hidapi for joycon stick on android platform


After a long time dig. Finally i made android can communicate with switch joycon stick.


If you want try this. You need change ASOP,Then complier it, Flash it.



What you need to do is :

1.open the hidraw file node (due to some unkone resone, Android close the /dev/hidraw* device file node, so you need open it.)

2.change some file in Android linux kernel.(linux/hidraw.h   linux/hidraw.c)

  you can find the different between liunx kernel and android liunx kernel in this two files.
  linux kernel :
  
  https://github.com/torvalds/linux/blob/master/drivers/hid/hidraw.c#L456
  https://github.com/torvalds/linux/blob/master/include/uapi/linux/hidraw.h#L42
  
  add this code to your ASOP 
  
  file => private/msm-google/drivers/hid/hidraw.c
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
 file => private/msm-google/include/uapi/linux/hidraw.h
 ```c
 #define HIDIOCGRAWUNIQ(len)     _IOC(_IOC_READ, 'H', 0x08, len)
 ```
