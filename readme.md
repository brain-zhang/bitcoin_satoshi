比特币v0.01版本源码整理
======================================

比特币已经运行10年了，我经历了详细的搜索，认为blockchain这个单词其实最早出自于bitcoin v0.01的代码注释中；

中本聪是比特币之父，比特币是区块链之母；这一点谁也无法否认；

令人感觉荒谬的是，在信息如此发达的今天，事情的本源反而隐藏到纷纷扰扰中看不清楚；

作为一个比特币爱好者，我从sourceforge上整理了中本聪发布bitcoin v0.01时的原始代码；用经典的VC++6.0编译，集齐了当时依赖的所有头文件及库文件，供大家研究创世之初比特币的样子；

./bin/bitcoin.exe 是我在winxp平台下，还原中本聪的原始编译环境编译出来的可执行文件，您可以运行两台虚拟机，把dll目录的文件拷贝到 C:\windows\system32 下，设置初始PEER IP地址，就可以在一个局域网内运行初版bitcoin客户端;

启动步骤:

1. 准备局域网内的两台虚拟机A、B
2. 在bitcoin.exe同级目录建立addr.txt文件，里面填写对端的连接IP:PORT，如192.168.2.7:8333
3. 分别启动A、B的bitcoin.exe
4. 连接对方节点后启动挖矿
5. 为了测试方便，默认将难度值调低了

细读代码，就可以发现中本聪是一个熟练的MS流派C++程序员，他对于密码学、GUI编程有深入了解，但不是神；

他的代码中有很多随意的东西，比如代表比特币难度的BLOCK HEADER的bits大小设置，比如初版Genius block手工构造后，没有写入到索引文件中，比如对于openssl库的理解有偏差，造成了公钥存储浪费了一些字节空间；

但中本聪在代码中展现了更多的设计上的天才和实现上的严谨，从bitcoin启动寻找peer的过程中，我们看到他对这一点考虑如何的周密，这绝对是经过了长时间大量专一的思考才能得到的成果；

Talk is cheap, show me your code. Just get fun.


中本聪发布0.01版声明
======================================
BitCoin v0.01 ALPHA

Copyright (c) 2009 Satoshi Nakamoto
Distributed under the MIT/X11 software license, see the accompanying
file license.txt or http://www.opensource.org/licenses/mit-license.php.
This product includes software developed by the OpenSSL Project for use in
the OpenSSL Toolkit (http://www.openssl.org/).  This product includes
cryptographic software written by Eric Young (eay@cryptsoft.com).


Compilers Supported
-------------------
* MinGW GCC (v3.4.5)
* Microsoft Visual C++ 6.0 SP6


Dependencies
------------
Libraries you need to obtain separately to build:

name        | default path| download
------------| ----------- | --------
wxWidgets   | \wxWidgets  |  http://www.wxwidgets.org/downloads/
OpenSSL     |  \OpenSSL   |  http://www.openssl.org/source/
Berkeley DB |  \DB        |  http://www.oracle.com/technology/software/products/berkeley-db/index.html
Boost       |  \Boost     |  http://www.boost.org/users/download/

Their licenses:

* wxWidgets:     LGPL 2.1 with very liberal exceptions
* OpenSSL:       Old BSD license with the problematic advertising requirement
* Berkeley DB:   New BSD license with additional requirement that linked software must be free open source
* Boost:         MIT-like license


OpenSSL
-------
Bitcoin does not use any encryption.  If you want to do a no-everything
build of OpenSSL to exclude encryption routines, a few patches are required.
(OpenSSL v0.9.8h)

Edit engines\e_gmp.c and put this #ifndef around #include <openssl/rsa.h>
```
  #ifndef OPENSSL_NO_RSA
  #include <openssl/rsa.h>
  #endif
```

Add this to crypto\err\err_all.c before the ERR_load_crypto_strings line:
```
  void ERR_load_RSA_strings(void) { }
```

Edit ms\mingw32.bat and replace the Configure line's parameters with this
no-everything list.  You have to put this in the batch file because batch
files can't handle more than 9 parameters.
```
  perl Configure mingw threads no-rc2 no-rc4 no-rc5 no-idea no-des no-bf no-cast no-aes no-camellia no-seed no-rsa no-dh
```

Also REM out the following line in ms\mingw32.bat.  The build fails after it's
already finished building libeay32, which is all we care about, but the
failure aborts the script before it runs dllwrap to generate libeay32.dll.
  REM  if errorlevel 1 goto end

Build
```
  ms\mingw32.bat
  ```

If you want to use it with MSVC, generate the .lib file
```
  lib /machine:i386 /def:ms\libeay32.def /out:out\libeay32.lib
```


Berkeley DB
-----------
MinGW with MSYS:
```
cd \DB\build_unix
sh ../dist/configure --enable-mingw --enable-cxx
make
```


Boost
-----
You may need Boost version 1.35 to build with MSVC 6.0.  I couldn't get
version 1.37 to compile with MSVC 6.0.
