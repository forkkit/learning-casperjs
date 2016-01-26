学习CasperJS
============

[CasperJS][1] is a navigation scripting & testing utility for the [PhantomJS][2]
(WebKit) and [SlimerJS][3] (Gecko) headless browsers, written in Javascript.

## 安装软件

- It works on Ubuntu 14.04 LTS out-of-box.
- Debian 8 needs `/usr/lib/x86_64-linux-gnu/libjpeg.so.8` to run properly.

> Official phantomjs binary can be download at <https://bitbucket.org/ariya/phantomjs/downloads>.

```bash
# VPS-IP: 1.2.3.4
$ echo -e 'host vps\n  hostname 1.2.3.4\n  user root' >> ~/.ssh/config
$ ssh-copy-id vps
$ pip install ansible
$ echo -e '[vps]\n1.2.3.4' >> /etc/ansible/hosts
$ wget https://github.com/vimagick/learning-casperjs/raw/master/playbook.yml
$ ansible-playbook playbook.yml
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

- CasperJS: [文档][4] | [视频][5]
- PhantomJS: [文档][6] | [视频][7]

[1]: http://casperjs.org/
[2]: http://phantomjs.org/
[3]: http://slimerjs.org/
[4]: http://docs.casperjs.org/en/latest/index.html
[5]: https://www.youtube.com/watch?v=Kefil5tCL9o
[6]: http://phantomjs.org/documentation/
[7]: https://www.youtube.com/watch?v=OqEcn_6GBDI

