# Read the header {#read-the-header}

After you declare the structure for boot sector, you can read the device now.

## Examples {#examples}

**IMPORTANT** Please create a virtual disk beforehand.

```c
  FILE *fp = NULL;
    struct fat_boot_sector boot_entry;

    fp = fopen(device_name, "r+");
    if(fp == NULL)
        exit(-1);
    uint32_t boot_entry_size = fread(&boot_entry, sizeof(struct fat_boot_sector), 1, fp);
    if(boot_entry_size != 1)
        exit(-1);
    //  Bytes per sector. Allowed values include 512, 1024, 2048, and 4096
    uint16_t bps = boot_entry.sector_size[0] + ((uint16_t) boot_entry.sector_size[1] << 8);
    off_t root_entry_offset = ( boot_entry.reserved +
                                boot_entry.fats * boot_entry.fat32.length) * bps;
    uint32_t bpc = bps * boot_entry.sec_per_clus;
    off_t fat_offset = bps * boot_entry.reserved;

    disk_info->fp = fp;
    disk_info->root_entry_offset = root_entry_offset;
    disk_info->bpc = bpc;
    disk_info->bps = bps;
    disk_info->spc = boot_entry.sec_per_clus;
    disk_info->reserved_sectors = boot_entry.reserved;
    disk_info->fat_offset = fat_offset;
    disk_info->num_fats = boot_entry.fats;
    disk_info->fat_size = boot_entry.fat32.length;
```

In the example, the program takes in a`device_name`, and read it from the beginning to the size of`fat_boot_sector`structure. After that the parameters have been stored and can be retrieved from the variables in the structure.

