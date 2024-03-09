# Docker

## 安装环境

Debian 10

## 安装Docker

```bash
apt update && apt install apt-transport-https ca-certificates curl software-properties-common gnupg2 && curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add - && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" && apt update && apt install docker-ce
```
