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
* 在链接阶段，需要增加选项 -lSDL2_ttf 
* 如果要在windows的cmd环境中运行，则需要将如下库拷贝到ffplay.exe所在目录：  
  C:\msys64\mingw64\bin\SDL2_ttf.dll  
  C:\msys64\mingw64\bin\libfreetype-6.dll  
  C:\msys64\mingw64\bin\zlib1.dll

   
## 3 编译(代码目录为F:ffmpeg-4.2.3-VCmpTool)  
* 运行 C:\msys64\mingw64.exe  
* cd /f/ffmpeg-4.2.3-VCmpTool  
* ./configure --enable-gpl --enable-static --disable-w32threads --disable-shared --enable-ffplay
* make -j8  
* 上一步make最终在生成ffplay.exe会出错，这是正常的，因为ffplay需要调用SDL_ttf等库，此时只要执行如下命令即可：
  gcc -I. -I./ -D_ISOC99_SOURCE -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -U__STRICT_ANSI__ -D__USE_MINGW_ANSI_STDIO=1 -D__printf__=__gnu_printf__ -D_POSIX_C_SOURCE=200112 -D_XOPEN_SOURCE=600 -DPIC -DZLIB_CONST -std=c11 -fomit-frame-pointer -g -Wdeclaration-after-statement -Wall -Wdisabled-optimization -Wpointer-arith -Wredundant-decls -Wwrite-strings -Wtype-limits -Wundef -Wmissing-prototypes -Wno-pointer-to-int-cast -Wstrict-prototypes -Wempty-body -Wno-parentheses -Wno-switch -Wno-format-zero-length -Wno-pointer-sign -Wno-unused-const-variable -Wno-bool-operation -Wno-char-subscripts -O3 -fno-math-errno -fno-signed-zeros -fno-tree-vectorize -Werror=format-security -Werror=implicit-function-declaration -Werror=missing-prototypes -Werror=return-type -Werror=vla -Wformat -fdiagnostics-color=auto -Wno-maybe-uninitialized -I/mingw64/include/SDL2  -I/mingw64/include/SDL2 -Dmain=SDL_main -MMD -MF fftools/ffplay.d -MT fftools/ffplay.o -c -o fftools/ffplay.o fftools/ffplay.c && gcc -Llibavcodec -Llibavdevice -Llibavfilter -Llibavformat -Llibavresample -Llibavutil -Llibpostproc -Llibswscale -Llibswresample -Wl,--nxcompat,--dynamicbase -Wl,--high-entropy-va -Wl,--as-needed -Wl,--warn-common -Wl,-rpath-link=:libpostproc:libswresample:libswscale:libavfilter:libavdevice:libavformat:libavcodec:libavutil:libavresample  -Wl,--image-base,0x140000000 -o ffplay_g.exe fftools/cmdutils.o fftools/ffplay.o  -lavdevice -lavfilter -lavformat -lavcodec -lpostproc -lswresample -lswscale -lavutil -lpsapi -lole32 -lstrmiids -luuid -loleaut32 -lshlwapi -lgdi32 -lm -lvfw32 -L/mingw64/lib -lmingw32 -lSDL2main -lSDL2 -lm -lm -lbz2 -lz -lsecur32 -lws2_32 -liconv -lm -llzma -lz -lole32 -luser32 -lm -lm -lm -lm -luser32 -lbcrypt  -L/mingw64/lib -lmingw32 -lSDL2main -lSDL2 -lshell32  -lSDL2_ttf && strip -o ffplay.exe ffplay_g.exe
