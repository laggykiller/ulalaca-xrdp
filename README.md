# 麗 -ulalaca-

this xrdp module requires [ulalaca-sessionprojector](https://github.com/unstabler/ulalaca)

# TODO / BUGS

- NOT SUITABLE FOR PRODUCTION USE YET
- handle input events: currently keyboard / mouse input not working
- calculate dirtyRects boundary: mstsc.exe raises error when minimize window

# INSTALLATION
1. fetch xrdp source code
```shell
$ curl -L https://github.com/neutrinolabs/xrdp/releases/download/v0.9.19/xrdp-0.9.19.tar.gz | tar xvz
cd xrdp-0.9.19
```

2. clone ulalaca-xrdp inside xrdp source code
```shell
$ git clone https://github.com/unstabler/ulalaca-xrdp ulalaca
```

3. apply patch
```shell
$ patch -p1 < ulalaca/xrdp-automake.patch
$ patch -p1 < ulalaca/xrdp-encoder-force-use-bgra.patch

# + fix hard-coded socket path
$ vi ulalaca/ulalaca.cpp
```
```diff
         _this->_socket = std::make_unique<UnixSocket>(
-             "/Users/unstabler/ulalaca-projector.socket"
+             "..."
         );
         _this->_socket->connect();

```

4. build & install
```shell
$ ./bootstrap
$ ./configure --enable-pixman PKG_CONFIG_PATH=/usr/local/opt/openssl/lib/pkgconfig:/usr/local/opt/libjpeg-turbo/lib/pkgconf
$ make -j8 
$ make install
```

5. edit /etc/xrdp/xrdp.ini
```diff
 ; Section name to use for automatic login if the client sends username
 ; and password. If empty, the domain name sent by the client is used.
 ; If empty and no domain name is given, the first suitable section in
 ; this file will be used.
 autorun=

 allow_channels=true
 allow_multimon=true
 bitmap_cache=true
-bitmap_compression=true
+bitmap_compression=false
-bulk_compression=true
+bulk_compression=false
 #hidelogwindow=true
 max_bpp=32
 new_cursors=true
 
 ; ...
 
+[Ulalaca]
+name=Ulalaca
+lib=libulalaca.dylib
```

