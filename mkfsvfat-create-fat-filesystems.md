# `mkfs.vfat`: Create FAT Filesystems {#mkfsvfat-create-fat-filesystems}

After creating a blank disk, we have to initalize it with a file system before we can use it.

In Linux, we have a command `mkfs.vfat` to format the disk to FAT.

## Usage {#usage}

```
mkfs.vfat -F FAT-size -f NUMFAT -S logical-sector-size -s sectors-per-cluster -R number-of-reserved-sectors DEVICE

```

You can control various parameters of the FAT filesystem.

## Example {#example}

Suppose we would like to create a FAT32 file systems,with

* 2 FATs
* 512 bytes per sector
* 1 sector per cluster
* 32 Reserved sectors

```
mkfs.vfat -F 32 -f 2 -S 512 -s 1 -R 32 test.disk

```

![](assets/mkfs.png)

Then the virtual disk we created earlier is now initialized.

