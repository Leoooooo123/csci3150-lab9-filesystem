# Traversing Cluster {#traversing-cluster}

In order to search the target cluster, we need to navigate to the Root Directory and search the items one by one. Since the root directory starts from cluster 2, the first search should begin at cluster 2.

```c
for(uint32_t i = 0; i < bpc / sizeof(struct msdos_dir_entry); i++) {
              struct msdos_dir_entry dir_entry;
              size_t ret = fread(&dir_entry, 1, sizeof(struct msdos_dir_entry), fp);
              if(dir_entry.name[0] != 0x00) {
                  if(dir_entry.attr != 0x0f) {
                    // Insert your code here
                    /*
                    Read the file name:
                      1. Check whether the file is deleted or not
                      2. Convert to 8.3 format names
                      3. Check whether it is a subdirectory

                    uint32_t dir_first_cluster = get_dir_first_cluster(&dir_entry);
                    printf("%d, ", _);
                    printf("%s, ", _);
                    printf("%d, ", _);
                    printf("%d", _);
                    printf("\n");
                    */
                  }
                  else {
                      printf("%d, LFN entry\n", idx++);
                  }
              }
```

Reference:

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

Tips1: File Deletion and File name

In FAT32, deleting a file does not mean to remove from the filesystem. It only deallocates the space that the file originally occupied and deletion is marked. In this case, the first character of the filename is marked with a special0xe5.

Tip2: Check whether it is a subdirectory

```c
if((dir_entry.attr & 0x10) != 0) {
  /* It is a subdirectory */
 }
```


