# sip.js + freeswitch
sip.js 0.11.x
freeswitch 1.8.x

实现了:
- 呼入自动接通
- 挂断
- 呼出

限制:
由于使用`SIP.Web.Simple`实现, 存在较多限制, 比如只能呼出一次, 参考[`SIP.Web.Simple`](https://github.com/onsip/SIP.js/blob/master/src/Web/Simple.js)用`SIP.UA`实现.

ps:
其他参考: [ringcentral-web-phone](https://npm.taobao.org/package/ringcentral-web-phone)和[ringcentral-web-phone.git](https://github.com/ringcentral/ringcentral-web-phone)


## FAQ
- [测试过程中碰到的错误](https://github.com/meilihao/tour_book/blob/master/develop/freeswitch.md)

## Next
**将切换到jssip**, 因为sip.js最新代码与官方例子版本差异很大, 案例难找, api文档不如jssip齐全.

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Run your tests
```
yarn run test
```

### Lints and fixes files
```
yarn run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
