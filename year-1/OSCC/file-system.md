# File System & Directories

## The file abstraction

### Files 
- Linear array of bytes, stored persistently
- Identified with file name and OS-level ID (Inode number)
-  Inode number:
	- Unique within file system
	- Serves as a unique ID for a specific piece of metadata

### Directory 
- Contains other sub-directories and files along with inode numbers
- Stored like a file, whose contents are filename-to-inode mapping
