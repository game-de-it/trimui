# ツールチェーンのダウンロードとdockerコンテナのビルド
下記サイトを参考  
[https://github.com/shauninman/union-tg5040-toolchain](https://github.com/shauninman/union-tg5040-toolchain)  
以下、docker内での作業  

# ビルド用の環境変数
```
TOOL=/opt/aarch64-linux-gnu/bin/aarch64-linux-gnu-
export CC=${TOOL}gcc
export CXX=${TOOL}g++
export AR=${TOOL}ar
export AS=${TOOL}as
export LD=${TOOL}ld
export RANLIB=${TOOL}ranlib
export STRIP=${TOOL}strip
export READELF=${TOOL}readelf
```

# pythonとopensslのインスト先ディレクトリ作成
```
mkdir -p /mnt/SDCARD/Python-3.11.10
```

# opensslのダウンロードとビルド
```
wget wget https://www.openssl.org/source/openssl-1.1.1w.tar.gz
tar zxvf openssl-1.1.1w.tar.gz
cd openssl-1.1.1w
./Configure linux-aarch64 --prefix=/mnt/SDCARD/Python-3.11.10/openssl
make -j18
make install -j18
cd ..
```

# pythonのダウンロードとビルド
```
wget https://www.python.org/ftp/python/3.11.10/Python-3.11.10.tgz
tar zxvf Python-3.11.10.tgz
cd Python-3.11.10
./configure ac_cv_prog_HAS_HG=/bin/false  ac_cv_prog_SVNVERSION=/bin/false  ac_cv_file__dev_ptmx=no  ac_cv_file__dev_ptc=no  ac_cv_have_long_long_format=yes  ac_cv_working_tzset=yes  ac_cv_func_lchflags_works=no  ac_cv_func_chflags_works=no  ac_cv_func_printf_zd=yes  ac_cv_buggy_getaddrinfo=no  ac_cv_header_bluetooth_bluetooth_h=no  ac_cv_header_bluetooth_h=no --prefix=/mnt/SDCARD/Python-3.11.10 --enable-optimizations --host=aarch64-linux-gnu --build=x86_64-linux-gnu --with-build-python CC=/opt/aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc CXX=/opt/aarch64-linux-gnu/bin/aarch64-linux-gnu-g++  --with-openssl=/mnt/SDCARD/Python-3.11.10/openssl/
make -j18
make install -j18
```

# pipインストールファイルをダウンロード
```
cd /mnt/SDCARD/Python-3.11.10/bin
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

# pythonインストディレクトリのzip化
```
cd /mnt/SDCARD/
zip -r Python-3.11.10.zip Python-3.11.10
mv Python-3.11.10.zip /root/workspace/
dockerを抜ける
```
`union-tg5040-toolchain`ディレクトリに内にある`workspace`ディレクトリ内にPython-3.11.10.zipファイルがある

# CrossMixのSDカードにPythonをコピー
Python-3.11.10.zipを解凍してできた`Python-3.11.10`ディレクトリをSDカードのルートにコピー

# TRIMUI BRICKでCrossMixを起動させてSSHで接続
ユーザ名は`root`、パスワードは`tina`

# pipのインストール
```
cd /mnt/SDCARD/Python-3.11.10/bin
LD_LIBRARY_PATH=/mnt/SDCARD/Python-3.11.10/openssl/lib/ /mnt/SDCARD/Python-3.11.10/bin/python3 get-pip.py
```

# pyxelのインストール
```
LD_LIBRARY_PATH=/mnt/SDCARD/Python-3.11.10/openssl/lib/ /mnt/SDCARD/Python-3.11.10/bin/pip3 install pyxel
```

以上

