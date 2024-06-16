# 💭 Linux Uninstall Node.js

To completely remove Node.js installed from the [deb.nodesource.com](http://deb.nodesource.com/) package, use `sudo` on Ubuntu or run as root on Debian.

```bash
dev@dev:~$ sudo apt-get purge nodejs && \
   sudo rm -r /etc/apt/sources.list.d/nodesource.list && \
   sudo rm -r /etc/apt/keyrings/nodesource.gpg
```

## 📋 Related Articles
- [Uninstall nodejs Ubuntu & Debian packages](https://github.com/nodesource/distributions#uninstall-nodejs-ubuntu--debian-packages)