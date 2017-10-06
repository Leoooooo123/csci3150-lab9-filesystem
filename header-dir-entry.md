# Header: Dir Entry {#header-dir-entry}

```c
struct msdos_dir_entry {
        __u8    name[MSDOS_NAME];/* name and extension */
        __u8    attr;           /* attribute bits */
        __u8    lcase;          /* Case for base and extension */
        __u8    ctime_cs;       /* Creation time, centiseconds (0-199) */
        __le16  ctime;          /* Creation time */
        __le16  cdate;          /* Creation date */
        __le16  adate;          /* Last access date */
        __le16  starthi;        /* High 16 bits of cluster in FAT32 */
        __le16  time,date,start;/* time, date and first cluster */
        __le32  size;           /* file size (in bytes) */
};

```

This is the predefined structure for each directory entry. Once you reach the beginning of that cluster,`fread`the size of the structure, and you can get all the parameters in the variables respectively.

## Attributes {#attributes}

In the structure, we can locate 1 byte of data storing the attribute of the file. Actually this tells us a lot:

| Bit | Mask | Description |
| :--- | :--- | :--- |
| 0 | 0x01 | Read-Only |
| 1 | 0x02 | Hidden |
| 2 | 0x04 | System Files |
| 3 | 0x08 | Volume Label |
| 4 | 0x10 | Subdirectory |
| 5 | 0x20 | Archive |
| 6 | 0x40 | Device |
| 7 | 0x80 | Reserved |
| 8 | 0x0F | Long File Name \(LFN\) |

In C programming, we can apply bitwise AND \(&\) to check a particular attribute, eg:

```c
if((dir_entry.attr & 0x10) != 0) {
  /* It is a subdirectory */
 }
```



