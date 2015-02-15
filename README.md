OpenResty Windows Port
=========
The following readme is based on [moonbingbing/openresty_windows](https://github.com/moonbingbing/openresty_windows)


Prebuilt binaries
=========
You can go to release, direct download the compiled package.  
These files are built using VS2013 and VS2008.

Build OpenResty
=========
## build tools
*    msys or other GNU tools collection
*    Perl(to build openssl)
*    Visual Studio

### download requirements
*    Download Openssl source, put binary and headers into `3rd_party_libs/openssl-1.0.1j/openssl`
*    Download zlib source 
*    Download PostgreSQL client binary and source
*    Download pcre library (should use 8.31)
*    Extract them into `3rd_party_libs`
*    Download and extract Openresty release into this directory, **be sure not to overwrite any existing files!**

You can check the directory structure with this project.

### prepare
*    Copy all files in `nginx-x.x.x*` into `src`
*    Start Your_Visual_Studio_Path\VC\vcvarsall.bat   
*    cd into `LuaJIT-2.1-20150120\src` (or other version) and run *msvcbuild.bat*    Copy PostgreSQL client binary to objs/ (libpq.dll), you may also need dependencies if error occured later(libintl.dll, usually shipped with name libintl-8.dll); copy `LuaJIT-2.1-20150120\src\lua51.dll`, openssl libs to objs/
*    Run  C:\mingw\msys\1.0\msys.bat in the above command window
*    cd to the project dir in the above command window

### set environment variables
*    export LUAJIT_LIB=LuaJIT-2.1-20150120/src/
*    export LUAJIT_INC=LuaJIT-2.1-20150120/src/
*    export LIBPQ_INC=3rd_party_libs/libpq-9.3.5/
*    export LIBPQ_LIB=3rd_party_libs/libpq-9.3.5/lib/

Note that version number may change.
    
### run configure script
```Shell
  auto/configure --with-cc=cl --with-cc-opt=-DFD_SETSIZE=1024\
  --builddir=objs  --prefix= \
  --add-module=ngx_devel_kit-0.2.19 \
  --add-module=ngx_coolkit-0.2rc2 \
  --add-module=set-misc-nginx-module-0.28 \
  --add-module=ngx_lua-0.9.14 \
  --add-module=echo-nginx-module-0.57 \
  --add-module=headers-more-nginx-module-0.25 \
  --add-module=ngx_postgres-1.0rc5 \
  --add-module=rds-json-nginx-module-0.13 \
  --with-pcre=3rd_party_libs/pcre-8.31 \
  --with-zlib=3rd_party_libs/zlib-1.2.7 \
  --with-openssl=3rd_party_libs/openssl-1.0.1j \
  --with-select_module --with-http_ssl_module \
  --with-http_realip_module --with-http_addition_module \
  --with-http_sub_module --with-http_dav_module \
  --with-http_stub_status_module --with-http_flv_module \
  --with-http_mp4_module --with-http_gzip_static_module \
  --with-http_random_index_module --with-http_secure_link_module \
  --with-mail --with-mail_ssl_module --with-ipv6
```
      
  Note that version number may change.

### some dirty mods
*  Find `wchar.h` in your MSVC include path(mostly like *C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\include\wchar.h*), and replace **_off_t** with **long**. A somple file can be found under `dirty/wchar.h`
*  Find these lines in `src/http/ngx_http_request.h` and comment the macros
```C
    //#if (NGX_HTTP_X_FORWARDED_FOR)
        ngx_array_t                       x_forwarded_for;
    //#endif
```
    
*  Add `#include <stdint.h>` to `ngx_postgres-1.0rc5/src/ngx_postgres_output.c`
*  find `#if (NGX_THREADS)` in ngx_http_lua_socket_udp.c, comment out all inside the macro(for 0.9.14, it's on line 1403)
*  If you want to manully edit config files of `ngx_lua` and `ngx_postgres`, add lines between "#################################Add" in sample file `ngx_lua-0.9.14/config` and `ngx_postgres-1.0rc5/config`

### make
  nmake

License
=========
(The MIT License)

Copyright (c) 2012 WenMing <moonbingbing@gmail.com>