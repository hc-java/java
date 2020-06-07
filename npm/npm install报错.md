npm install 报错

~~~js
npm ERR! code Z_BUF_ERROR
npm ERR! errno -5
npm ERR! zlib: unexpected end of file

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\HC\AppData\Roaming\npm-cache\_logs\2020-06-05T13_21_18_743Z-debug.log

~~~



解决办法：

清除 npm 缓存

~~~js
npm cache clean --force
~~~

