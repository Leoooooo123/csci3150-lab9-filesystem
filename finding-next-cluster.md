# Finding Next Cluster {#finding-next-cluster}

After reading the current cluster, how can we advance to the next cluster? 

**Answer**: by checking the FAT table. With the current cluster number, we can get the next cluster in FAT, or a EOF.

How can we know the terminating condition? There are three conditions in total. If we encounter these during search, that means it reaches the end.

1. End of File
   **EOF**
   \(0x0ffffff8\)
2. Damaged \(Bad Cluster Mark\) \(0x0ffffff7\)
3. Unallocated \(0x0\)

```c
uint32_t next_cluster(FILE *fp, off_t fat_offset, uint32_t current_cluster) {
    uint32_t next_cluster_number = 0;
    fseek(fp, fat_offset + current_cluster * sizeof next_cluster_number,
          SEEK_SET);
    size_t ret = fread(&next_cluster_number, 1, sizeof next_cluster_number, fp);
    return next_cluster_number;
}
```

