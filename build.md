# Building Kitty Terminal

```shell
set -x PKG_CONFIG_PATH
set -x CFLAGS
set -x LDFLAGS
set -l LIB_DIRS zlib libpng liblcms2 openssl librsync harfbuzz freetype2 graphite2 brotli
set -l LIB_NAMES zlib libpng lcms2 openssl rsync harfbuzz freetype2 graphite2 libbrotlidec

# add pkg-config path
for f in LIB_DIRS; set -a PKG_CONFIG_PATH /libs/$f/lib/pkgconfig; end

# check for errors
for f in $LIB_NAMES; pkg-config --print-errors $f; end

# register library compiler flags 
for f in $LIB_NAMES; set -a CFLAGS (pkg-config --cflags $f); end
for f in $LIB_NAMES; set -a LDFLAGS (pkg-config --libs $f); end

# go version 1.20 above required
set -a PATH /tools/go/bin

make
python3 -m pip install -r docs/requirements.txt && make docs
make app

cp -r kitty.app /Applications/
```
