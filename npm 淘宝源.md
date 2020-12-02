# mac os

## NPM 淘宝源 安装

`npm install -g cnpm --registry=https://registry.npm.taobao.org`

## Node 权限问题

`sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}  `

