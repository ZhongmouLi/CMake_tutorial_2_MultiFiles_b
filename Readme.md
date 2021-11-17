# Dealing with multiple files with Cmake

This practice is to show
- how to write a CMakeLists.txt when there a library is located in a different folder



# CMakeLists when libraries are located in different folder
We sperate libraries in "buidling" process by using add_library since we do not need to take wheels all the time. Simimarly, those wheels will not be touched and modified often. 

## structure tree
Thus, we put those wheels, libraries, in different floders than main file. 

It looks like

-----root<br/>
&emsp;| <br/>
&emsp;|--- CMakeLists.txt <br/>
&emsp;| <br/>
&emsp;|--- main.cpp<br/>
&emsp;|<br/>
&emsp;|--- function<br/>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|---- function1.cpp<br/>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|---- function1.hpp<br/>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;|---- CMakeLists.txt<br/>

## Cmake questions and solution
There are two main questions for the above structure:
1. an independent CMakeLists.txt is for the library;
2. tell the main file the location of the library.


### CMakesLists for library
The purpose of the library CMakeLists is simple, just to generate the lirbrary using the cpp and hpp files. It is the same with the previous example.

So, in the function folder, we should write it using add_library as<br/>
```
add_library(adder SHARED
            function1.hpp
            function1.cpp)  
```
At the same time, the main CMakeLists.txt should be able to "call" the library CMakeLists.txt to buid the library in order to precess the whole program.

In the main CMakeList.txt, we add the following command
```
add_subdirectory(function)
```
before "linking" the library to the corresponding target.

### Location of library in calling it
In learning Cmake, we should not forget c++ programming. For example, we have talked a lot of building libraries in order to use them. Nevertheless, do we still rememeber how our main.cpp calls the library?

The answer is **\include "function1.hpp"**. Let us go back to the previous example 2MultiFiles where library files and main files are in the same folder. 

We know they are in the same folder, thus we call the library in the main.cpp as 
```
#include "function1.hpp"
```
Now, how can we know the library location, or the hpp location, with respect to the main.cpp. They could be placed in different folders, different disks etc.

Fortunately, we know the relative location of library hpp file to their CMakeLists.txt. Besides, Cmake knows where all the CMakeLists.txt are.

lib hpp----->lib CMakeLists----->main CMakeLists----->main

Thus, we just need to add the relative location information of hpp to the library. After that, we are able to call this hpp using this relative location information. 

This is done with 2 steps:<br/>
1. for the CMakeLists.txt in library folder, i.e. funciton,
```
target_include_directories(function_name PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}")
```

Then, the main file or any other lib uses this library will know where the relative location of hpp is.<br/>

2. for the main.cpp, we write
```
#include "function1.hpp"
```
which is the relative location of function1.hpp to the lib CMakeList.txt.


# Source
Youtuber **vector-of-bool**, How to CMake Good - 1c - Subdirectories and Target Interface Properties, [link](https://www.youtube.com/watch?v=SYgESCQeGJY&list=PLK6MXr8gasrGmIiSuVQXpfFuE1uPT615s&index=8&ab_channel=vector-of-bool).

