### mingw-w64

```shell
sudo apt install mingw-w64
```

```shell
i686-w64-mingw32-gcc 42341.c -o syncbreeze_exploit.exe
```

```shell
i686-w64-mingw32-gcc 42341.c -o syncbreeze_exploit.exe -lws2_32
#-lws2_32 linker libs
```