# Tranversing Cluster {#tranversing-cluster}

In order to search the target cluster, we need to navigate to the Root Directory and search the item one by one. As the root directory starts from cluster 2, therefore the first search should begin at cluster 2.

```c
for(uint32_t i = 0; i < bpc / sizeof(struct msdos_dir_entry); i++) {
              struct msdos_dir_entry dir_entry;
              size_t ret = fread(&dir_entry, 1, sizeof(struct msdos_dir_entry), fp);
              if(dir_entry.name[0] != 0x00) {
                  if(dir_entry.attr != 0x0f) {
                      if(dir_entry.name[0] == 0xe5) {
                          dir_entry.name[0] = '?';
                      }
                      char filename[14] = { 0 };
                      convert_83filename(dir_entry.name, filename);
                      uint32_t dir_first_cluster = get_dir_first_cluster(&dir_entry);
                      if((dir_entry.attr & 0x10) != 0) {
                          strcat(filename, "/");
                      }
                      printf("%d, ", idx++);
                      printf("%s, ", filename);
                      printf("%d, ", dir_entry.size);
                      printf("%d", dir_first_cluster);
                      printf("\n");
                  }
                  else {
                      printf("%d, LFN entry\n", idx++);
                  }
              }
```