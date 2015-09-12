# 学习CasperJS

[CasperJS][1] is a navigation scripting & testing utility for the [PhantomJS][2]
(WebKit) and [SlimerJS][3] (Gecko) headless browsers, written in Javascript.

## 安装软件

操作系统: `Ubuntu 14.04.1 LTS`

```bash
# 安装 phantomjs (2.0.0)
$ wget https://github.com/vimagick/dockerfiles/raw/master/webkit/bin/phantomjs
$ strip phantomjs
$ mv phantomjs /usr/local/bin/
$ phantomjs --version
2.0.0

# 安装 casperjs (latest)
$ wget https://github.com/n1k0/casperjs/archive/master.zip -O casperjs-master.zip
$ unzip casperjs-master.zip
$ mv casperjs-master /usr/local/lib/casperjs
$ sed -i '/Warning PhantomJS v2.0 not yet released/s@^@//@' /usr/local/lib/casperjs/bin/bootstrap.js
$ ln -s /usr/local/lib/casperjs/bin/casperjs /usr/local/bin/
$ casperjs --version
1.1.0-beta3
```

## 快速入门

```bash
$ cat > sample.js << "_EOF_"
var casper = require('casper').create();
casper.start('http://casperjs.org/', function() {
    this.echo(this.getTitle());
});
casper.thenOpen('http://phantomjs.org', function() {
    this.echo(this.getTitle());
});
casper.run();
_EOF_

$ casperjs sample.js
CasperJS, a navigation scripting and testing utility for PhantomJS and SlimerJS
PhantomJS | PhantomJS
```

## 参考资料

- CasperJS [文档][4] [视频][5]
- PhantomJS [文档][6] [视频][7]

[1]: http://casperjs.org/
[2]: http://phantomjs.org/
[3]: http://slimerjs.org/
[4]: http://docs.casperjs.org/en/latest/index.html
[5]: https://www.youtube.com/watch?v=Kefil5tCL9o
[6]: http://phantomjs.org/documentation/
[7]: https://www.youtube.com/watch?v=OqEcn_6GBDI

