
# CMake Scripts for Embedded Development

This [repository](https://github.com/embedded-office/cmake-scripts) is intended as a Git submodule in `/cmake` within your Embedded programming project.

## Usage

The build system is highly inspirated from the book:
> [Professional CMake: A Practical Guide](https://crascit.com/)  
> 12th Edition  
> ISBN 978-1-925904-18-5  
> © 2018-2022 by Craig Scott  

Therefore, it is no surprise that we establish the following top level directories:

```
+- cmake        : folder is a Git submodule, referencing this repository
+- dependencies : contains definitions and patches to external components
+- doc          : documentation of the project
+- src          : the source code of the project
+- tests        : tests for the project features
```

## Content of cmake

The following scripts are included:


### CMake Package Management

[CPM.cmake](https://github.com/cpm-cmake/CPM.cmake) is a cross-platform CMake script that adds dependency management capabilities to CMake. It's built as a thin wrapper around CMake's [FetchContent](https://cmake.org/cmake/help/latest/module/FetchContent.html) module that adds version control, caching, a simple API [and more](https://github.com/cpm-cmake/CPM.cmake#comparison-to-pure-fetchcontent--externalproject).

The script in this repository is the [`get_cpm.cmake`](https://github.com/cpm-cmake/CPM.cmake/blob/master/cmake/get_cpm.cmake) of the CPM release during configuration of the project build.

*Example:*

add script in toplevel `CMakeLists.txt`:

```cmake
include(cmake/CPM.cmake)
add_subdirectory(dependencies)
```

and use it in `/dependencies`:

```cmake
CPMAddPackage(
  NAME    depProjectTarget
  URL     depProjectUrl
)
```

### Toolchain Files

The toolchain file describes the cross-compiler settings, including two interface functions.

**`setOutfile(target filename)`**

This function is useful for executable targets.

The filename of the generated elf-image (without extension) and the matching map-file is added to the linker options. The location of these files is `/out/<build-type>`

**`setLinkerScript(target path/filename)`**

This function is useful for executable targets. 

The given linker script (without extension) is added to the linker options. The linker script extension is added in according to the used linker tool.

*Example:*

```cmake
add_executable(myApp)
# add your source files
target_source(myApp ...)
# add your include directories
target_include_directories(myApp ...)
# add dependency targets
target_link_libraries(myApp ...)
# define linker script / section layout
setLinkerScript(myApp "${CMAKE_CURRENT_SOURCE_DIR}/board/foo")
# define output and mapfile filename
setOutputfile(myApp "my-great-app")
```
