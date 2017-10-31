# C Header of FAT {#c-header-of-fat}

In Linux, there is already predefined header for accessing the FAT32 system. Just include :

```c
#include <linux/msdos_fs.h>
```

You can check the whole file in`/usr/include/linux/`folder. Inside this header, there is a predefined structure, called **fat\_boot\_sector** and **msdos\_dir\_entry**. This provides a simple way to get all the parameters

