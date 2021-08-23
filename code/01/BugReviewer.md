# 一
在Vscode中最好单独打开每个工作区，例如：在目录结构:

|  Gameslearning   |  |
|  ----  | ----  |
| code  |  |
| --  | 00 |
| --  | 01 |

中，要运行code/00或01中的代码，最好单独将其打开，相应的.vscode工作区也单独设置，而不是在Gameslearning/的上层文件夹中打开，否则会使得cmake不知道去找哪里的cmakelists.txt(也可能是因为不会配置，存疑，总之单独打开工作区并运行更容易配置)
# 二
对于安装好的OpenCV，在cmakelists.txt中除了要运行```find_package(OpenCV REQUIRED)```还要将其与源文件链接```target_link_libraries(Rasterizer ${OpenCV_LIBRARIES})```

# 三

在配置完Eigen和OpenCV后尝试运行```cmake ..```正常，但是运行```mingw32-make```或```mingw64-make```(后者是自己命名的64位mingw编译器以与前者区分)，均会报错：
```
opencv error: 'recursive_mutex' in namespace 'std' does not name a type
```
尝试在cmakelists中加上```set(CMAKE_CXX_STANDARD 17)```不起作用，该错误与此无关（加不加无所谓），而与**cmake时配置的gcc编译器**有关，因为最开始默认配置的时C:/MinGW/bin下的32位gcc，这里需要在Vscode中的cmake-tools重新配置C:/mingw64/bin的gcc编译器。

但是配置好后cmake-tools报错：
```
Unable to determine what CMake generator to use. Please install or configure a preferred generator, or update settings.json, your Kit configuration or PATH variable. Error: No usable generator found.
```
这需要在.vscode/setting.json中加上```"cmake.generator": "MinGW Makefiles"```制定cmake生成的构建文件即可