Bitcoin v0.01 version source code and libs VC++6.0 Project All In One
======================================

As a bitcoin enthusiast, I collated the original code from sourceforge when Satoshi launched bitcoin v0.01; With the classic VC++6.0 compilation, set all the header files and library files that were relied on at that time, for everyone to study the appearance of Bitcoin at the beginning of creation;



Build
======================================
1. set up WinXP SP2 32bit Env
2. set up [VC++6.0](https://github.com/brain-zhang/bitcoin_satoshi/releases/download/v0.01_compile_env/Visual.C++.6.0_simple_for_bitcoin0.1_compile_version.zip)
3. `git clone https://github.com/brain-zhang/bitcoin_satoshi.git`
4. open `bitcoin.dsw` by VC++6.0
5. compile in Win32 Debug mode


Run
======================================
bitcoin.exe is an executable file compiled by Satoshi Nakamoto's original compilation environment under the winxp platform. You can run two virtual machines, copy the dll directory file to C:\windows\system32, and set the initial PEER IP address. You can run the first version of the bitcoin client within a LAN;

You can download bitcoin.exe and dlls in [Release](https://github.com/brain-zhang/bitcoin_satoshi/releases)

Startup steps:

1. Prepare two VMS A and B on the LAN
2. Create the addr.txt file in the same directory as bitcoin.exe and fill in the file with the connection IP address of the peer end :PORT, for example, 192.168.2.7:8333
3. Start bitcoin.exe for A and B respectively
4. Connect the peer node and start mining
5. For the convenience of testing, the difficulty value is lowered by default



satoshi origin bitcoin v0.01-ALPHA
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
