# deepin中更新curl

- 从网上直接下载（[curl版本网站地址](https://curl.haxx.se/download/)），比如如下

  ```
  wget http://curl.haxx.se/download/curl-7.60.0.tar.gz
  ```

- 解压到当前目录

  ```
  tar -zxf curl-7.60.0.tar.gz
  ```

- 进入解压后的目录

  ```
  cd curl-7.60.0
  ```

- 然后运行一下命令

  ```
  ./configure
  make
  sudo make install
  ```

- 如果遇到下面的报错

  ```
  curl: (48) An unknown option was passed in to libcurl

  很有可能是curl的版本号和libcurl版本号不匹配导致的，可以运行以下命令进行修复

  sudo ldconfig
  ```

  ​