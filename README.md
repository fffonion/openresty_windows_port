OpenResty_windows
=========
The following readme is based on [moonbingbing/openresty_windows](https://github.com/moonbingbing/openresty_windows)


Download OpenResty_windows binary package
=========
You can go to release, direct download the compiled package.  
Unzip and run :

    nginx.exe -c your_nginx.conf
Done!

Build prerequisites
=========
If you want to build it yourself, you can  refer to the following steps.    
Before you build Openresty on windows, I strongly suggest you to build nginx follow this article: [Building nginx on the Win32 Platform with Visual C](http://nginx.org/en/docs/howto_build_on_win32.html). You do not need to download the source code of nginx ,PCRE ,zlib or OpenSSL, all source codes are ready.

Build OpenResty
=========

1. __download requirements__
    *    Download Openssl source, put binary and headersinto `3rd_party_libs/openssl-1.0.1j/openssl`
    *    Download zlib source
    *    Download PostgreSQL client binary and source
    *    Download pcre library (should use 8.31)
    *    Extract them into `3rd_party_libs`
    *    Download and extract Openresty release into this directory, **be sure not to overwrite any existing files!**
    You can check the directory structure with this project.

2.   __prepare__
    *    Copy all files in `nginx-*` into `src`
    *    Start Your_Visual_Studio_Path\VC\vcvarsall.bat   
    *    cd into `LuaJIT-2.1-20140805\src` (or other version) and run *msvcbuild.bat*
    *    Run  C:\mingw\msys\1.0\msys.bat in the above command window
    *    cd to the project dir in the above command window

3. __set environment variables__
    *    export LUAJIT_LIB=LuaJIT-2.1-20140805/src/
    *    export LUAJIT_INC=LuaJIT-2.1-20140805/src/
    *    export LIBPQ_INC=3rd_party_libs/libpq-9.3.5/
    *    export LIBPQ_LIB=3rd_party_libs/libpq-9.3.5/lib/
    Note that version number may change.
    
4. __run configure script__    
```Shell
  auto/configure --with-cc=cl --with-cc-opt=-DFD_SETSIZE=1024\
  --builddir=objs  --prefix= \
  --add-module=ngx_devel_kit-0.2.19 \
  --add-module=ngx_coolkit-0.2rc1 \
  --add-module=set-misc-nginx-module-0.26 \
  --add-module=ngx_lua-0.9.12 \
  --add-module=echo-nginx-module-0.56 \
  --add-module=headers-more-nginx-module-0.25 \
  --add-module=ngx_postgres-1.0rc4 \
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

5. __some dirty mods__
    *  Find `wchar.h` in your MSVC include path(mostly like *C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\include\wchar.h*), and replace **_off_t** with **long**. A somple file can be found under `dirty/wchar.h`
    *  Find these lines in `src/http/ngx_http_request.h` and comment the macros
```C
    //#if (NGX_HTTP_X_FORWARDED_FOR)
        ngx_array_t                       x_forwarded_for;
    //#endif
```
    *  If you want to manully edit config files of `ngx_lua` and `ngx_postgres`, add lines between **#################################Add** in sample file `ngx_lua-0.9.12/config` and `ngx_postgres-1.0rc4/config`

6. __make__   
  nmake


License
=========
(The MIT License)

Copyright (c) 2012 WenMing <moonbingbing@gmail.com>