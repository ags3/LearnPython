## 7. Working with files and directories

### 7.1 Opening files
To open a file, one can use the _open()_ function:

```python
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```
Where:  
- _file_ represents a _path-like_ object containing the file pathname (either absolute or relative)  
- _mode_ represents the mode in which the file is opened:  

| Character     | Meaning                                                         |
| --------------|-----------------------------------------------------------------|
|'r'            | open for reading (default)                                      |
|'w'            | open for writing, truncating the file first                     |
|'x'            | open for exclusive creation, failing if the file already exists |
|'a'            | open for writing, appending to the end of the file if it exists |
|'b'            | binary mode                                                     |
|'t'            | text mode (default)                                             |
|'+'            | open for updating (reading and writing)                         |

- _encoding_ represents the encoding to be used (applies to file open in text mode).
  
For the rest of the parameters, you can look in the official documentation: https://docs.python.org/3/library/functions.html#open  
In case of success, the _open()_ function returns a _file object_, otherwise an _OSError_ exception will be raised.  

Example:  
```python
>>> my_file = open("data/sample_read.txt", "r", encoding="utf-8")
>>> my_file
<_io.TextIOWrapper name='data/sample_read.txt' mode='r' encoding='utf-8'>
>>> my_file.closed
False
```

Here, we open a file in read text mode. As we can see, the managed to open the file successfully in read because the file already existed.  
Eventually we should always close the files we have opened successfully, by using the _close()_ function:  

```python
>>> my_file.close()
>>> my_file.closed
True
```

What is happening if we are trying to open a file which does not exists in read mode ?

```python
>>> my_file = open("data/no_such_file.txt", "rb")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: 'data/no_such_file.txt'
```

A _FileNotFoundError_ exception is raised.

### 7.2 Reading files
Let us read the entire content of the file in a string variable:  

```python
>>> my_file = open("data/sample_read.txt", "r", encoding="utf-8")
>>> content = my_file.read()
>>> print(content)
In the year 1878 I took my degree of Doctor of Medicine of the
University of London, and proceeded to Netley to go through the course
prescribed for surgeons in the army. Having completed my studies there,
I was duly attached to the Fifth Northumberland Fusiliers as Assistant
Surgeon. The regiment was stationed in India at the time, and before
I could join it, the second Afghan war had broken out. On landing at
Bombay, I learned that my corps had advanced through the passes, and
was already deep in the enemy’s country. 

>>> my_file.close()
```

The entire content of the file was read in the _content_ string variable. Be careful, tought, for very big files this might not be feasible due to increased memory consumption.

Does it change something if we will open the file in read binary mode ?

```python
>>> my_file = open("data/sample_read.txt", "rb")
>>> content = my_file.read()
>>> print(content)
b'In the year 1878 I took my degree of Doctor of Medicine of the\nUniversity of London, and proceeded to Netley to go through the course\nprescribed for surgeons in the army. Having completed my studies there,\nI was duly attached to the Fifth Northumberland Fusiliers as Assistant\nSurgeon. The regiment was stationed in India at the time, and before\nI could join it, the second Afghan war had broken out. On landing at\nBombay, I learned that my corps had advanced through the passes, and\nwas already deep in the enemy\xe2\x80\x99s country. \n'
>>> my_file.close()
```

We notice a few things:
- To open the file in read binary mode, we have used the "rb" string passed to the mode argument.
- We don't (and can't) specify an encoding when we are open a file in binary mode.
- The content of the file was actually read in a _bytes_ variable, not a string one.

Let us read the file line by line and print its contents:  

```python
>>> my_file = open("data/sample_read.txt", "r", encoding="utf-8")
>>> for line in my_file:
...     line = line.strip()
...     print(line)
... 
In the year 1878 I took my degree of Doctor of Medicine of the
University of London, and proceeded to Netley to go through the course
prescribed for surgeons in the army. Having completed my studies there,
I was duly attached to the Fifth Northumberland Fusiliers as Assistant
Surgeon. The regiment was stationed in India at the time, and before
I could join it, the second Afghan war had broken out. On landing at
Bombay, I learned that my corps had advanced through the passes, and
was already deep in the enemy’s country.
>>> my_file.close()
>>> 
```

Here, we _iterate_ over all the lines of the file using a _for_ statement.  
Each line is ending with a new-line and the new-line character is included in the read line.  
We can strip the read line using the _strip()_ function.  

This is the same as the following example:

```python
>>> my_file = open("data/sample_read.txt", "r", encoding="utf-8")
>>> line = my_file.readline()
>>> while line:
...     line = line.strip()
...     print(line)
...     line = my_file.readline()
... 
In the year 1878 I took my degree of Doctor of Medicine of the
University of London, and proceeded to Netley to go through the course
prescribed for surgeons in the army. Having completed my studies there,
I was duly attached to the Fifth Northumberland Fusiliers as Assistant
Surgeon. The regiment was stationed in India at the time, and before
I could join it, the second Afghan war had broken out. On landing at
Bombay, I learned that my corps had advanced through the passes, and
was already deep in the enemy’s country.
>>> my_file.close()
```

But the previous example is both more simple and more _pythonic_.  

Our file reading examples so far are _naive_ and _error prone_. Exceptions might be raised during file reading and in that case we fail to close the file, which is a _programming error_ (a.k.a _bug_).  
Using what we have learned so far about exceptions, we can fix that and write a piece of code which displays a text file line by line as follows:

```python
>>> my_file = open("data/sample_read.txt", "r", encoding="utf-8")
>>> try:
...     for line in my_file:
...         line = line.strip()
...         print(line)
... finally:
...     my_file.close()
... 
In the year 1878 I took my degree of Doctor of Medicine of the
University of London, and proceeded to Netley to go through the course
prescribed for surgeons in the army. Having completed my studies there,
I was duly attached to the Fifth Northumberland Fusiliers as Assistant
Surgeon. The regiment was stationed in India at the time, and before
I could join it, the second Afghan war had broken out. On landing at
Bombay, I learned that my corps had advanced through the passes, and
was already deep in the enemy’s country.
>>> my_file.closed
True
```

Here, we close the file in a _finally_ block, so that to be executed regardless if an exception was raised or not.  
While this work, there is a better way: by using the _with_ statement:

```python
>>> with open("data/sample_read.txt", "r", encoding="utf-8") as my_file:
...     for line in my_file:
...         line = line.strip()
...         print(line)
... 
In the year 1878 I took my degree of Doctor of Medicine of the
University of London, and proceeded to Netley to go through the course
prescribed for surgeons in the army. Having completed my studies there,
I was duly attached to the Fifth Northumberland Fusiliers as Assistant
Surgeon. The regiment was stationed in India at the time, and before
I could join it, the second Afghan war had broken out. On landing at
Bombay, I learned that my corps had advanced through the passes, and
was already deep in the enemy’s country.
>>> my_file.closed
True
```

In this case, the file gets closed when exiting the _with_ block. This is the recommended way of reading a file line by line in Python.  
We can also read all the lines from the file in a _list_ of _string_s.  

```python
>>> with open("data/sample_read.txt", "r", encoding="utf-8") as my_file:
...     lines = my_file.readlines()
...     print(lines)
... 
['In the year 1878 I took my degree of Doctor of Medicine of the\n', 'University of London, and proceeded to Netley to go through the course\n', 'prescribed for surgeons in the army. Having completed my studies there,\n', 'I was duly attached to the Fifth Northumberland Fusiliers as Assistant\n', 'Surgeon. The regiment was stationed in India at the time, and before\n', 'I could join it, the second Afghan war had broken out. On landing at\n', 'Bombay, I learned that my corps had advanced through the passes, and\n', 'was already deep in the enemy’s country. \n']
```

But also be careful, the memory consumption can be high for large files.  

We can also read a given number of characters (or bytes) from a file. While we are reading the file, we can also find out the position at which we currently are located by using the _tell()_ function:

```python
>>> with open("data/sample_read.txt", "r", encoding="utf-8") as my_file:
...     first_chars = my_file.read(16)
...     print(first_chars)
...     print(my_file.tell())
... 
In the year 1878
16
```

### 7.3 Writing files

We can also write text in a file, by using the _write()_ function. For example:  

```python
>>> with open("data/sample_write.txt", "w", encoding="utf-8") as my_file:
...     for line in ("one", "two", "three"): 
...         my_file.write(line)
... 
3
3
5
```

```bash
%> cat data/sample_write.txt
%> onetwothree
```

The _write()_ function returns the number of character written.  
Also, please be aware that when you open a file in write mode, the previous contents of the file will be erased if the file already exists.   

If you have a list of strings, you can write all the elements from the list in the file by using the _writelines_ function:

```python
>>> lines = ["one", "two", "three"]
>>> with open("data/sample_write.txt", "w", encoding="utf-8") as my_file:
...     my_file.writelines(lines)
```

```bash
%> cat data/sample_write.txt
%> onetwothree
```

You can also write binary data to a file. For example:  

```python
>>> with open("data/sample_write.txt", "wb") as my_file:
...     my_file.write(b"\x48\x65\x6c\x6c\x6f\x20\x77\x6f\x72\x6c\x64")
... 
11
```

```bash
cat data/sample_write.txt
Hello world
```

Here, we wrote the _bytes_ corresponding to the ASCII codes for the string "Hello world".

### 7.4 Listing directories

There are several ways of listing directories content.  
One is by using the _os.listdir()_ function:

```python
>>> import os
>>> os.listdir("data")
['sample_write.txt', 'sample_read.txt']
```

Please note that when we list a directory, we get only the files and sub-directories in that directory (we do not recurse into the content of the sub-directories).

Modern python prefers using the _pathlib_ module:

```python
>>> from pathlib import Path
>>> dir_entries = Path("data/")
>>> for dir_entry in dir_entries.iterdir():
...     print(dir_entry.name)
... 
sample_write.txt
sample_read.txt
```

### 7.5 Checking if a filesystem entry is a file or directory

We can use the _os.path.exists()_ function to check if we have a file _or_ a directory with a given name.  
We can use the _os.path.isfile()_ function to test for file presence or the _os.path.isdir()_ one to test for directory presence:

```python
>>> import os
>>> os.path.exists("data")
True
>>> os.path.isdir("data")
True
>>> os.path.isfile("data")
False
>>> os.path.exists("data/sample_read.txt")
True
>>> os.path.isfile("data/sample_read.txt")
True
>>> os.path.isdir("data/sample_read.txt")
False
```

Of course, modern python prefer to use the _pathlib_ module:

```python
>>> from pathlib import Path
>>> Path("data").is_dir()
True
>>> Path("data").is_file()
False
>>> 
>>> Path("data/sample_read.txt").is_file()
True
>>> Path("data/sample_read.txt").is_dir()
False
```

### 7.6 Joining and spliting paths

We can use the _os.path.join()_, _os.path.split()_ and _os.path.splitext()_ for joining and spliting paths.  
Also, we can use the _os.path.basename()_ function to get the filename from a path, the _os.path.dirname()_ function to get the parent directory and the _os.path.splitext()_ to get the file extension.

```python
>>> import os
>>> joined_path = os.path.join("dir_name", "subdir_name1", "subdir_name2", "file_name.txt")
>>> joined_path
'dir_name/subdir_name1/subdir_name2/file_name.txt'
>>> os.path.split(joined_path)
('dir_name/subdir_name1/subdir_name2', 'file_name.txt')
>>> os.path.basename(joined_path)
'file_name.txt'
>>> os.path.dirname(joined_path)
'dir_name/subdir_name1/subdir_name2'
>>> os.path.splitext(joined_path)[-1]
'.txt'
```

Of course, we can do de same using the _pathlib_ module:

```python
>>> from pathlib import Path
>>> p = Path("dir_name")/Path("subdir_name1")/Path("subdir_name2")/Path("file_name.txt")
>>> p
PosixPath('dir_name/subdir_name1/subdir_name2/file_name.txt')
>>> p.as_posix()
'dir_name/subdir_name1/subdir_name2/file_name.txt'
>>> p.name
'file_name.txt'
>>> p.parent.as_posix()
'dir_name/subdir_name1/subdir_name2'
>>> p.suffix
'.txt'
```

Assuming that we want to show only the files in a given directory, and not also the sun-directories, we can proceed as follows:

```python
>>> for dir_entry in os.listdir("data"):
...     if os.path.isfile(os.path.join("data", dir_entry)):
...         print(dir_entry)
... 
sample_write.txt
sample_read.txt
```

Or, using the _pathlib_ module:

```python
>>> from pathlib import Path
>>> for dir_entry in Path("data").iterdir():
...     if dir_entry.is_file:
...         print(dir_entry.name)
... 
sample_write.txt
sample_read.txt
```

### 7.7 Getting files and directories attributes

One way is to use the _os.stat()_ function:

```python
>>> os.stat("data/sample_read.txt")
os.stat_result(st_mode=33204, st_ino=10224776, st_dev=2049, st_nlink=1, st_uid=1000, st_gid=1000, st_size=528, st_atime=1571696623, st_mtime=1571696103, st_ctime=1571696103)
>>> 
```

```bash
%> stat data/sample_read.txt 
  File: data/sample_read.txt
  Size: 528       	Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d	Inode: 10224776    Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/  adrian)   Gid: ( 1000/  adrian)
Access: 2019-10-22 01:23:43.560000000 +0300
Modify: 2019-10-22 01:15:03.248000000 +0300
Change: 2019-10-22 01:15:03.316000000 +0300
 Birth: -
```

Another way is, of course, by usng the _pathlib_ module:

```python
>>> from pathlib import Path
>>> Path("data/sample_read.txt").stat()
os.stat_result(st_mode=33204, st_ino=10224776, st_dev=2049, st_nlink=1, st_uid=1000, st_gid=1000, st_size=528, st_atime=1571696623, st_mtime=1571696103, st_ctime=1571696103)
```

Of course, you can obtain the attributes of a directory also. Here is how you can do it using the _pathlib_ module (but the _os.stat()_ function also works fine):

```python
>>> Path("data").stat()
os.stat_result(st_mode=16893, st_ino=10224752, st_dev=2049, st_nlink=2, st_uid=1000, st_gid=1000, st_size=4096, st_atime=1571700625, st_mtime=1571700625, st_ctime=1571700625)
```

### 7.8 Creating and deleting directories

You can create a new directory by using the _os.mkdir()_ function:

```python
>>> import os
>>> os.path.exists("dir_to_delete")
False
>>> os.mkdir("dir_to_delete")
>>> os.path.exists("dir_to_delete")
True
```

Check that the directory was created from the terminal:

```bash
%> ls -lah dir_to_delete/
total 8,0K
drwxrwxr-x 2 adrian adrian 4,0K oct 22 03:37 .
drwxrwxr-x 4 adrian adrian 4,0K oct 22 03:37 ..
```

You can remove empty directories using the _os.rmdir()_ function:

```python
>>> import os
>>> os.rmdir("dir_to_delete")
>>> os.path.exists("dir_to_delete")
False
```

You can do the same operations by using the _patlib_ module:

```python
>>> from pathlib import Path
>>> Path("dir_to_delete").mkdir()
>>> Path("dir_to_delete").is_dir()
True
>>> Path("dir_to_delete").rmdir()
>>> Path("dir_to_delete").is_dir()
False
```

The methods used above for deleting directories works only if the directory is empty.   
To illustrate this, first create a non-empty directory:  

```bash
%> mkdir dir_to_delete
%> touch dir_to_delete/file_to_delete
```

Now, try to delete it:

```python
>>> from pathlib import Path
>>> Path("dir_to_delete").rmdir()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/adrian/miniconda3/lib/python3.7/pathlib.py", line 1295, in rmdir
    self._accessor.rmdir(self)
OSError: [Errno 39] Directory not empty: 'dir_to_delete'
>>> Path("dir_to_delete").is_dir()
True
```

No luck, an _OSError_ exception was raised.

We _can_, however, delete that directory by using the _shutil.rmtree()_ function:

```python
>>> import shutil
>>> from pathlib import Path
>>> Path("dir_to_delete").is_dir()
True
>>> shutil.rmtree("dir_to_delete")
>>> Path("dir_to_delete").is_dir()
False
```

### 7.9 Deleting files

You can delete files by using the _os.remove()_ function or using the _pathlib_ module.  
First, create some files to be removed:  

```bash
%> touch file1_to_delete file2_to_delete
```

Now remove them using python:  

```python
>>> from pathlib import Path
>>> os.path.isfile("file1_to_delete")
True
>>> Path("file2_to_delete").is_file()
True
>>> os.remove("file1_to_delete")
>>> Path("file2_to_delete").unlink()
>>> os.path.isfile("file1_to_delete")
False
>>> Path("file2_to_delete").is_file()
False
```

### 7.10 Copying. moving and renaming files and directories

We can copy files using the _shutil.copy()_ or _shutil.copy2()_ functions.  
The _shutil.copy()_ copy the files and preserve the files permissions. The _shutil.copy2()_ does the same but also preserves other files metadata, such as the files creation and modification timestamps.  

```python
>>> from pathlib import Path
>>> import shutil
>>> 
>>> 
>>> dir_to_delete = Path("dir_to_delete")
>>> dir_to_delete.is_dir()
False
>>> dir_to_delete.mkdir()
>>> dir_to_delete.is_dir()
True
>>> src_file_path = Path("data/sample_read.txt")
>>> src_file_path.is_file()
True
>>> dest_file_path = Path("dir_to_delete/sample_read.txt")
>>> dest_file_path.is_file()
False
>>> shutil.copy("data/sample_read.txt", "dir_to_delete")
'dir_to_delete/sample_read.txt'
>>> dest_file_path.is_file()
True
>>> shutil.rmtree("dir_to_delete")
>>> dir_to_delete.is_dir()
False
```

To copy directories, we can use the _shutil.copytree()_ function. Here is an example:  

```python
>>> from pathlib import Path
>>> import shutil
>>> 
>>> dir_to_delete = Path("dir_to_delete")
>>> dir_to_delete.is_dir()
False
>>> shutil.copytree("data", "dir_to_delete")
'dir_to_delete'
>>> dir_to_delete.is_dir()
True
>>> Path("dir_to_delete/sample_read.txt").is_file()
True
>>> Path("dir_to_delete/sample_write.txt").is_file()
True
>>> shutil.rmtree("dir_to_delete")
>>> dir_to_delete.is_dir()
False
```

To move files and directories. we can use the _shutil.move()_ function:

```python
>>> from pathlib import Path
>>> import shutil
>>> 
>>> src_dir = Path("data")
>>> src_dir.is_dir()
True
>>> dest_dir = Path("data2")
>>> dest_dir.is_dir()
False
>>> shutil.move("data", "data2")
'data2'
>>> src_dir.is_dir()
False
>>> dest_dir.is_dir()
True
>>> shutil.move("data2", "data")
'data'
>>> dest_dir.is_dir()
False
>>> src_dir.is_dir()
True
```

Finally, we can rename files or directories using either the _os.rename()_ function or the _pathlib_ module.  
Here is an example using the _pathlib_ module:

```python
>>> from pathlib import Path
>>> import shutil
>>> 
>>> src_file = Path("data/sample_read.txt")
>>> src_file.is_file()
True
>>> src_file.rename("data/sample_read_new.txt")
>>> src_file.is_file()
False
>>> dest_file = Path("data/sample_read_new.txt")
>>> dest_file.is_file()
True
>>> dest_file.rename("data/sample_read.txt")
>>> src_file.is_file()
True
>>> dest_file.is_file()
False
```
