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
- Files and directories are arranged in a tree starting with root ("/")


## Operations on files
### Creating a file
- `open()` syscall with flags to create a file
- Returns number called "file descriptor"
```c
int fd = open("foo", O_CREAT|O_WRONGLY|O_TRUNC, S_IRUSR|SIWUSR)
```

### Opening a file
- Existing files must be opened before they can be read/written
- Also uses `open()` syscall, and returns fd

### C