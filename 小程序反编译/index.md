# 小程序反编译

> 记录一下小程序源文件包的获取过程

## 1.所需环境

1. re文件管理器
2. 反编译脚本[`wxappUnpacker`](https://gitee.com/outfile/wxappUnpacker)或[微信小程序反编译自动化工具](https://www.ydxinzuo.cn/download.php?id=1701)
3. `node.js` 安装

## 2.找到小程序的源文件包

> /data/data/com.tencent.mm/MicroMsg/{{一串32位的16进制字符串文件夹}}/appbrand/pkg/

文件夹 会有很多`.wxapkg`类型的文件，都是微信小程序的包

## 3.反编译

```
node .\wuWxapkg.js D:\_-xxxxxxxx_xx.wxapkg  
```

> 如果出现某些`module`未安装可以用`npm install xxx`安装
>
> 如果遇到` __vd_version_info__ is not defined`,修改`wuWxss.js`文件（亲测无效，建议直接使用上文的exe文件）

源文件：

```javascript
function runVM(name,code){
    let wxAppCode={},handle={cssFile:name};
    let vm=new VM({sandbox:Object.assign(new GwxCfg(),{__wxAppCode__:wxAppCode,setCssToHead:cssRebuild.bind(handle)})});
    vm.run(code);
    for(let name in wxAppCode)if(name.endsWith(".wxss")){
        handle.cssFile=path.resolve(frameName,"..",name);
        wxAppCode[name]();
    }
}
```

改为：

```javascript
function runVM(name,code){
    let wxAppCode={},handle={cssFile:name};
    let gg = new GwxCfg();
    let tsandbox ={$gwx:GwxCfg.prototype["$gwx"],__mainPageFrameReady__:GwxCfg.prototype["$gwx"],__wxAppCode__:wxAppCode,setCssToHead:cssRebuild.bind(handle)};
    let vm = new VM({sandbox:tsandbox});
    vm.run(code);
    for(let name in wxAppCode)if(name.endsWith(".wxss")){
        handle.cssFile=path.resolve(frameName,"..",name);
        wxAppCode[name]();
    }
}
```

### 4.调试

1. 打开微信开发者工具，本地调试
2. 关闭`ES6`转`ES5`配置
3. 不校验域名和证书
