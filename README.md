# Python FS - a pythonic file system wrapper for humans

[![Build Status](https://travis-ci.org/chaosmail/python-fs.svg?branch=master)](https://travis-ci.org/chaosmail/python-fs)

An easy to use file system wrapper for Python that aims to simplify os, os.path, os.walk, shutils, fnmatch, etc.

This is under active development!

## Documentation

```python
import fs
```

### fs.exists(path)

Reurns True if the *path* exists. Returns False if *path* does not exist.

```python
>>> fs.exists('test.txt')
True
>>> fs.exists('some_directory')
True
```

### fs.isfile(path)

Reurns True if the *path* exists and is a file. Returns False if *path* is a directory or does not exist.

```python
>>> fs.isfile('test.txt')
True
>>> fs.isfile('some_directory')
False
```

### fs.isdir(path)

Reurns True if the *path* exists and is a directory. Returns False if *path* is a file or does not exist.

```python
>>> fs.isdir('test.txt')
False
>>> fs.isdir('some_directory')
True
```

### fs.rename(oldPath, newPath)

Renames oldPath to new newPath where *oldPath* can be either a file or directory. Raises *OSError* exception if *oldPath* does not exist.

```python
>>> fs.rename('old_test.txt', 'new_test.txt')
>>> fs.rename('old_directory', 'new_directory')
```

### fs.truncate(path)

Removes all files from the *path* directory.

```python
>>> fs.truncate('some_directory')
```

### fs.chdir(path)

Changes the current directory to *path*.

```python
>>> fs.chdir('some_directory')
```

### fs.abspath(path)

Returns the absolute path from a relative *path* where *path* can be either file or directory.

```python
>>> fs.abspath('test.txt')
'/path/to/file/test.txt'
>>> fs.abspath('some_directory')
'/path/to/file/some_directory'
```

### fs.rm(path)

Deletes *path* where *path* can be either a file or directory. Raises an *OSError* exception if *path* does not exist.

```python
>>> fs.rm('test.txt')
>>> fs.rm('some_directory')
```


### fs.rmfile(path)

Deletes the file *path*. Raises an *OSError* exception if the file does not exist or *path* is a directory.

```python
>>> fs.rmfile('test.txt')
```

The Unix-like *fs.unlink* is the same as *fs.rmfile*.

### fs.rmfiles(paths)

Deletes an array of files *paths*. Raises an *OSError* exception if a file does not exist or an element of *paths* is a directory.

```python
>>> fs.rmfiles(['test.txt', 'another_file.txt'])
```

Example: *Remove all files from the current directory*:

```python
>>> fs.rmfiles( fs.listfiles() )
```


Example: *Remove all .pyc files from a directory*:

```python
>>> fs.rmfiles( fs.find('*.pyc') )
```

### fs.rmdir(path, recursive=True)

Deletes the directory *path* with all containing files and directories. Raises an *OSError* exception if the directory does not exist or *path* is a file.

```python
>>> fs.rmdir('some_directory')
```

### fs.rmdirs(paths, recursive=True)

Deletes an array of directories *paths* with all containing files and directories. Raises an *OSError* exception if a directory does not exist or an element of *paths* is a file.

```python
>>> fs.rmdirs(['some_directory', 'some_other_dir'])
```

Example: *Remove all directories from the current directory*:

```python
>>> fs.rmdirs( fs.listdirs() )
```


Example: *Remove all directories that start with local_*:

```python
>>> fs.rmdirs( fs.finddirs('local_*') )
```


### fs.touch(path)

Sets the modification timestamp of *path* to the current time or creates the file if *path* does not exist. Directories not supported on Windows.

```python
>>> fs.touch('test.txt')
```

### fs.list(path='.')

Generator the returns all files and directories that are contained in the directory *path*. Raises an *OSError* exception if the directory *path* does not exist.

```python
>>> gen = fs.list()
>>> list(gen)
['some_directory', 'test.txt']
>>> gen = fs.list('some_directory')
>>> list(gen)
['another_test.txt']
```

Example: *Loop over all files and directories in the current directory*:

```python
>>> for f in fs.list():
		pass
```


### fs.listfiles(path='.')

Generator the returns all files that are contained in the directory *path*. Raises an *OSError* exception if the directory *path* does not exist.

```python
>>> gen = fs.listfiles()
>>> list(gen)
['test.txt']
>>> gen = fs.listfiles('some_directory')
>>> list(gen)
['/path/to/dir/some_directory/another_test.txt']
```

Example: *Loop over all files in the current directory*:

```python
>>> for f in fs.listfiles():
		pass
```

### fs.listdirs(path='.')

Generator the returns all directories that are contained in the directory *path*. Raises an *OSError* exception if the directory *path* does not exist.

```python
>>> gen = fs.listdirs()
>>> list(gen)
['some_directory']
>>> gen = fs.listdirs('some_directory')
>>> list(gen)
[]
```

Example: *Loop over all directories in the current directory*:

```python
>>> for d in fs.listdirs():
		pass
```

### fs.find(pattern, path='.', exclude=None, recursive=True)

Generator the returns all files that match *pattern* and are contained in the directory *path*. Both *pattern* and *exclude* can be [Unix shell-style wildcards](https://docs.python.org/3.4/library/fnmatch.html) or arrays of wildcards. Raises an *OSError* exception if the directory *path* does not exist.

```python
>>> gen = fs.find('*.txt')
>>> list(gen)
['/path/to/file/test.txt', '/path/to/file/some_directory/another_test.txt']
>>> gen = fs.find('*.txt', exclude='another*')
>>> list(gen)
['/path/to/file/test.txt']
```

Example: *Loop over all .csv files in the current directory*:

```python
>>> for f in fs.find('*.csv', recursive=False):
		pass
```

Example: *Loop over all .xls and .xlsx files in the current directory and all sub-directories*:

```python
>>> for f in fs.find(['*.xls', '*.xlsx']):
		pass
```

Example: *Loop over all .ini files in the config directory and all sub-directories except the ones starting with local_*:

```python
>>> for f in fs.find('*.ini', path='config', exclude='local_*'):
		pass
```

### fs.finddirs(pattern, path='.', exclude=None, recursive=True)

Generator the returns all directories that match *pattern* and are contained in the directory *path*. Both *pattern* and *exclude* can be [Unix shell-style wildcards](https://docs.python.org/3.4/library/fnmatch.html) or arrays of wildcards. Raises an *OSError* exception if the directory *path* does not exist.

```python
>>> gen = fs.finddirs('some*')
>>> list(gen)
['/path/to/file/some_directory']
>>> gen = fs.finddirs('some*', exclude='*directory')
>>> list(gen)
[]
```

Example: *Loop over all .git directories in the current directory and all subdirectories*:

```python
>>> for d in fs.find('.git'):
		pass
```


### fs.content(path, content=None, encoding="UTF-8")

Returns or sets the content of a file *path*.

```python
>>> fs.content('text.txt')
u'test'
>>> fs.content('text.txt', 'test')
```


### fs.get(path, encoding='UTF-8')

Returns the content of a file *path*. Raises an *IOError* exception if the file *path* does not exist.

```python
>>> fs.get('text.txt')
u'test'
```

### fs.put(path, content, encoding='UTF-8')

Sets the content *content* of a file *path*.

```python
>>> fs.set('text.txt', 'test')
```

### fs.append(path, content, encoding='UTF-8')

Appends the content *content* of a file *path*. Raises an *IOError* exception if the file *path* does not exist.

```python
>>> fs.append('text.txt', 'test')
```

### fs.join(part_of_path, part_of_path, ...)

Joins different parts of a path together.

```python
>>> fs.join('/path/to', 'directory')
```

### fs.cwd(path=None)

Returns or sets the current working directory.

```python
>>> fs.cwd()
'/path/to/directory'
>>> fs.cwd('some_dir')
```

### fs.extension(path)

Returns the extension of a file *path*.

```python
>>> fs.extension('test.txt')
'txt'
```

### fs.filename(path)

Returns the filename of a file *path*.

```python
>>> fs.filename('test.txt')
'test'
```

### fs.dirname(path)

Returns the directory of a file *path*.

```python
>>> fs.dirname('/path/to/file/test.txt')
'/path/to/file'
```

## License

This software is provided under the MIT License.