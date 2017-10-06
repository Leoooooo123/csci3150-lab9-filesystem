# Header: Boot Sector {#header-boot-sector}

```c
 struct fat_boot_sector {
         __u8    ignored[3];     /* Boot strap short or near jump */
         __u8    system_id[8];   /* Name - can be used to special case
                                    partition manager volumes */
         __u8    sector_size[2]; /* bytes per logical sector */
         __u8    sec_per_clus;   /* sectors/cluster */
         __le16  reserved;       /* reserved sectors */
         __u8    fats;           /* number of FATs */
         __u8    dir_entries[2]; /* root directory entries */
         __u8    sectors[2];     /* number of sectors */
         __u8    media;          /* media code */
         __le16  fat_length;     /* sectors/FAT */
         __le16  secs_track;     /* sectors per track */
         __le16  heads;          /* number of heads */
         __le32  hidden;         /* hidden sectors (unused) */
         __le32  total_sect;     /* number of sectors (if sectors == 0) */

         /* The following fields are only used by FAT32 */
         __le32  fat32_length;   /* sectors/FAT */
         __le16  flags;          /* bit 8: fat mirroring, low 4: active fat */
         __u8    version[2];     /* major, minor filesystem version */
         __le32  root_cluster;   /* first cluster in root directory */
         __le16  info_sector;    /* filesystem info sector */
         __le16  backup_boot;    /* backup boot sector */
         __le16  reserved2[6];   /* Unused */
 };
```

To use it, first declare a`fat_boot_sector`structure, then use`fread()`to read the first n bytes of data to that structure, where`n`is the size of`fat_boot_sector`structure. After reading all the parameters are read into the variables respectively.

```c
// Declaration
struct fat_boot_sector boot_entry;
```

## Getting the information {#getting-the-information}

The following can be retrieved directly from the structure:

1. Sectors Per Cluster \(sec\_per\_clus\)
2. Number of Reserved Sector\(reserved\)
3. Number of FATs \(fats\)
4. Sector per FAT \(fat\_length\)

And these need some manipulations/calculations

1. Bytes Per Sector \(sector\_size\[0\] + sector\_size\[1\] 
   &lt;
   &lt;
    8\)
2. Bytes Per Cluster\(Bytes Per Sector \* sec\_per\_clus\)
3. First FAT Starting Position \(Bytes Per Sector \* Number of Reserved Sector\)
4. Data Area Starting Position \(Number of Reserved Sector + Number of Fats \* Sector per FAT\) \* Bytes Per Sector



