# Windows File Systems

## FAT (File Allocation Table)
### Basics
- Used in flash media (USB drives)
- Versions:
	- FAT12
	- FAT16
	- FAT32
	- vFAT

### FAT Directory Entry
- Each file or directory are references and described in a separate directory entry


### Data Clusters
- Consist of one or more sectors
- Cluster is the smallest unit in which data can be stored
- Stored in a linked list

### File Allocation table
- Tracks the sequence and allocation of clusters for each file
- Tracks the free/available clusters that can be written to
- Tracks bad clusters (dont overwrite them)
- The FAT number is the size of bits for each entry in the table


### FAT Area
- Contains:
	- Allocation status of each cluster
	- The clusters the follow the starting cluster for  a given file or directory 


### FAT Entries




## NTFS

### NTFS vs FAT
- Better read/write/
