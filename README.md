# ffmpeg-4.2.3-VCmpTool for Windows
A video compare tool based on ffmpeg-4.2.3

# Windows Build Steps:
## 1 Prepare build enviroment(msys2):
* Download from https://www.msys2.org/ and install to C:\msys64.
* 运行 C:\msys64\mingw64.exe
* 安装工具前，先配置msys2的下载源（提高软件安装速度）：
  msys64\etc\pacman.d 目录下有三个文件 mirrorlist.msys, mirrorlist.mingw64, mirrorlist.mingw32  
  更换为清华的源:  
  mirrorlist.msys    增加一行： Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/$arch  
  mirrorlist.mingw32 增加一行： Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/i686  
  mirrorlist.mingw64 增加一行： Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/x86_64  
* 安装编译所需相关软件： 
  1. 安装make, diffutils, yasm, pkg-config  
   pacman -S make  
   pacman -S diffutils  
   pacman -S yasm  
   pacman -S pkg-config  
  2. 安装mingw-w64(包括gcc，gdb)  
   pacman -S mingw-w64-x86_64-gcc  
   pacman -S mingw-w64-x86_64-gdb  
  3. 安装SDL2 (ffmpeg 4.1需要 SDL2)  
   pacman -S mingw-w64-x86_64-SDL2  
* 配置系统环境变量
  环境变量没配好会导致无法识别到SDL库，最终无法编译出ffplay。  

## 2 本项目使用SDL_ttf在屏幕上输出
* sdl_ttf下载地址： http://www.libsdl.org/projects/SDL_ttf/release/SDL2_ttf-devel-2.0.15-mingw.tar.gz  
* 下载后，解压，将文件夹SDL2_ttf-2.0.15\x86_64-w64-mingw32\ 与 C:\msys64\mingw64 合并。  

   
## 3 编译(代码目录为F:ffmpeg-4.2.3-VCmpTool)  
* 运行 C:\msys64\mingw64.exe  
* cd /f/ffmpeg-4.2.3-VCmpTool  
* ./configure --enable-gpl --enable-static --disable-w32threads --disable-shared --enable-ffplay
* make -j8  
