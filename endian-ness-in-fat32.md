# Endian-ness in FAT32 {#endianness-in-fat32}

Endian-ness is about byte ordering. We have two types: **big endian** and **little endian**.

Suppose we see the following, which represents the bytes per sector.

![](assets/BPS.png)

We have two ways to interpret this number, either _0x00 02_ or _0x02 00_

## Big Endian {#big-endian}

0x0002 = 2 bytes

## Little Endian {#little-endian}

0x0200 = 512 bytes

As in FAT32, **little Endian** is used. Therefore, the number of bytes per sector is _512 bytes_.

