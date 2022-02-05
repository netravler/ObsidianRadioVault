# [note.nkmk.me](https://note.nkmk.me/en/)

1.  [Top](https://note.nkmk.me/en/)
 3.  [Python](https://note.nkmk.me/en/python/)

# Check Python version from command line / in script

Posted: 2019-09-20 / Tags: [Python](https://note.nkmk.me/en/python/)

 

This article describes how to check and get the Python version installed and executed.

-   Check the Python version on the command line: `--version`, `-V`, `-VV`
-   Check the Python version in the script: `sys`, `platform`
    -   Various information strings including version number: `sys.version`
    -   Tuple of version numbers: `sys.version_info`
    -   Version number string: `platform.python_version()`
    -   Tuple of version number strings: `platform.python_version_tuple()`

If you get the version number in the script, you can not only display it with `print()`, but you can also switch the code to be executed depending on the version.

If you want to check the version of the package, OS, etc. instead of the version of Python itself, see the following articles.

-   [Check the version of Python package / library](https://note.nkmk.me/en/python-package-version/)
-   [Get the OS and its version where Python is running](https://note.nkmk.me/en/python-platform-system-release-version/)

Sponsored Link

## Check the Python version on the command line: --version, -V, -VV

Execute the `python` or `python3` command with the `--version` or `-V` option on the command prompt on Windows or the terminal on Mac.

`$ python --version
Python 2.7.15

$ python -V
Python 2.7.15

$ python3 --version
Python 3.7.0

$ python3 -V
Python 3.7.0` 

As in the example above, in some environments, the Python2.x series is assigned to `python` command, and the Python3.x series is assigned to `python3` command.

The `-VV` option has been added since Python 3.6. More detailed information than `-V` is output.

`$ python3 -VV
Python 3.7.0 (default, Jun 29 2018, 20:13:13) 
[Clang 9.1.0 (clang-902.0.39.2)]` 

## Check the Python version in the script: sys, platform

You can use the standard library `sys` module or `platform` module to get the version of Python that is actually running.

The same script can be used on Windows, Mac, and Linux such as Ubuntu.

It is useful for checking which version of Python is running in an environment where multiple versions of Python are installed. Even though you thought Python3 was running, there was a case where Python2 was running, so if something goes wrong, check it once.

It is also be used when you want to switch operation depending on whether it is Python2 or Python3.

### Various information strings including version number: sys.version

`sys.version` is a string indicating various information including the version number.

> **sys.version**  
> A string containing the version number of the Python interpreter plus additional information on the build number and compiler used.  
> [sys.version — System-specific parameters and functions — Python 3.7.4 documentation](https://docs.python.org/3/library/sys.html#sys.version)

```jupyter
import sys

print(sys.version)

```
# 3.7.0 (default, Jun 29 2018, 20:13:13) 
# [Clang 9.1.0 (clang-902.0.39.2)]

```jupyter 

print(type(sys.version))
```
# <class 'str'>` 

source: [sys_version.py](https://github.com/nkmk/python-snippets/blob/0c3b7857d8a62492df2e918c3fcb7023a4a978a3/notebook/sys_version.py#L1-L8)

### Tuple of version numbers: sys.version_info

`sys.version_info` is a tuple indicating the version number.

> **sys.version_info**  
> A tuple containing the five components of the version number: major, minor, micro, releaselevel, and serial.  
> [sys — System-specific parameters and functions — Python 3.7.4 documentation](https://docs.python.org/3/library/sys.html#sys.version_info)

```jupyter
print(sys.version_info)
```
# sys.version_info(major=3, minor=7, micro=0, releaselevel='final', serial=0)

print(type(sys.version_info))
# <class 'sys.version_info'>` 

source: [sys_version.py](https://github.com/nkmk/python-snippets/blob/0c3b7857d8a62492df2e918c3fcb7023a4a978a3/notebook/sys_version.py#L10-L14)

`releaselevel` is `str` and the other elements are `int`.

Each value can be obtained by specifying an index.

```jupyter
print(sys.version_info[0])
```
# 3` 

source: [sys_version.py](https://github.com/nkmk/python-snippets/blob/0c3b7857d8a62492df2e918c3fcb7023a4a978a3/notebook/sys_version.py#L16-L17)

From with version 2.7 for Python2 and from version 3.1 for Python3, elements can be obtained by name (`major`,`minor`, `micro`,`releaselevel`, `serial`).

For example, if you want to get a major version:

```jupyter
print(sys.version_info.major)
```
# 3

source: [sys_version.py](https://github.com/nkmk/python-snippets/blob/0c3b7857d8a62492df2e918c3fcb7023a4a978a3/notebook/sys_version.py#L19-L20)

If you want to determine whether Python2 or Python3 is running, you can check the major version with this `sys.version_info.major`. `2` means Python2, and `3` means Python3.

An example of switching between Python2 and Python3 is as follows.

```jupyter
if sys.version_info.major == 3:
    print('Python3')
else:
    print('Python2')
```
# Python3` 

source: [sys_version.py](https://github.com/nkmk/python-snippets/blob/0c3b7857d8a62492df2e918c3fcb7023a4a978a3/notebook/sys_version.py#L22-L26)

Use `sys.version_info.minor` if you want to switch by minor version.

As mentioned above, element access using names is supported from version 2.7 and version 3.1, so if there is a possibility that it will be executed in an earlier version, use `sys.version_info[0]` or `sys.version_info[1]`.

### Version number string: platform.python_version()

`platform.python_version()` returns a string `major.minor.patchlevel`.

> **platform.python_version()**  
> Returns the Python version as string 'major.minor.patchlevel'.[platform — Access to underlying platform’s identifying data — Python 3.7.4 documentation](https://docs.python.org/3/library/platform.html#platform.python_version)
```jupyter
import platform


print(platform.python_version())
```
# 3.7.0

```jupyter
import platform

print(type(platform.python_version()))
```
# <class 'str'>` 

source: [platform_python_version.py](https://github.com/nkmk/python-snippets/blob/0c3b7857d8a62492df2e918c3fcb7023a4a978a3/notebook/platform_python_version.py#L1-L7)

It is useful when you want to get the version number as a simple string.

### Tuple of version number strings: platform.python_version_tuple()

`platform.python_version_tuple()` returns a tuple `(major, minor, patchlevel)`. The type of each element is `str`, not `int`.

> **platform.python_version_tuple()**  
> Returns the Python version as tuple (major, minor, patchlevel) of strings.  
> [platform — Access to underlying platform’s identifying data — Python 3.7.4 documentation](https://docs.python.org/3/library/platform.html#platform.python_version_tuple)

```jupyter
import platform

print(platform.python_version_tuple())
# ('3', '7', '0')
```
print(type(platform.python_version_tuple()))
# <class 'tuple'>` 

source: [platform_python_version.py](https://github.com/nkmk/python-snippets/blob/0c3b7857d8a62492df2e918c3fcb7023a4a978a3/notebook/platform_python_version.py#L9-L13)

Since it is just a tuple unlike `sys.version_info`, it cannot be accessed with the name such as `major` or `minor`.