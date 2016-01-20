学习CasperJS
============

[CasperJS][1] is a navigation scripting & testing utility for the [PhantomJS][2]
(WebKit) and [SlimerJS][3] (Gecko) headless browsers, written in Javascript.

## 安装软件

- It works on Ubuntu 14.04 LTS out-of-box.
- Debian 8 needs `/usr/lib/x86_64-linux-gnu/libjpeg.so.8` to run properly.

```yaml
---

- name: install phantomjs and casperjs

  hosts: vps

  tasks:
  - name: 'download phantomjs'
    get_url:
      url: https://github.com/vimagick/learning-casperjs/raw/master/bin/phantomjs-2.0.1
      dest: /usr/local/bin/phantomjs
      sha256sum: '3846b7ea251f8a239e9b8727f05a7c65054ae45997ab06ee29ab7f333d4fda7a'
      mode: 0755

  - name: 'download libjpeg'
    get_url:
      url: https://github.com/vimagick/learning-casperjs/raw/master/lib/libjpeg.so.8
      dest: /usr/lib/x86_64-linux-gnu/libjpeg.so.8
      sha256sum: 'b9de29dab0f1585d1a49b557e88bf3842d84f3115f90cc2c0a370cbcb9058e16'
      mode: 0644
    when: ansible_distribution == 'Debian'

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

> Official phantomjs binary can be download at <https://bitbucket.org/ariya/phantomjs/downloads>.

```bash
# VPS-IP: 1.2.3.4
$ echo -e 'host vps\n  hostname 1.2.3.4\n  user root' >> ~/.ssh/config
$ ssh-copy-id vps
$ pip install ansible
$ echo -e '[vps]\n1.2.3.4' >> /etc/ansible/hosts
$ ansible-playbook phantomjs.yml
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

