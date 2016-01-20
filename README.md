学习CasperJS
============

[CasperJS][1] is a navigation scripting & testing utility for the [PhantomJS][2]
(WebKit) and [SlimerJS][3] (Gecko) headless browsers, written in Javascript.

## 安装软件

操作系统: `Ubuntu 14.04.1 LTS`

```yaml
---

- name: install phantomjs and casperjs

  hosts: vps

  tasks:
  - name: 'download phantomjs'
    get_url:
      url: https://github.com/vimagick/dockerfiles/raw/master/webkit/bin/phantomjs
      dest: /usr/local/bin/phantomjs
      sha256sum: '2110217cf1e2979fa49db82c2e9e7e7f7c787dfa1b885b342804c7958d613461'
      mode: 0755

  - name: 'create directory'
    file:
      dest: /usr/local/casperjs
      state: directory
      mode: 0755

  - name: 'download casperjs'
    get_url:
      url: https://github.com/n1k0/casperjs/archive/master.tar.gz
      dest: /tmp
      force: yes

  - name: 'unarchive casperjs'
    command: tar xzf /tmp/casperjs-master.tar.gz --strip 1
    args:
      chdir: /usr/local/casperjs

  - name: 'patch casperjs'
    command: sed -i '/Warning PhantomJS v2.0 not yet released/s@^@//@' bootstrap.js
    args:
      chdir: /usr/local/casperjs/bin

  - name: 'link casperjs'
    file:
      src: /usr/local/casperjs/bin/casperjs
      dest: /usr/local/bin/casperjs
      state: link

  - name: 'get version'
    shell: phantomjs --version && casperjs --version
    register: result

  - name: 'show version'
    debug: msg="{{ result.stdout_lines }}"
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

