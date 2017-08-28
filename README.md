# player

> a vue player

## 使用方法

### 开发预览

``` bash
### 安装依赖
npm install

### 运行服务
npm run dev
### 该服务运行后将在 http://localhost:8080 此时会自动打开浏览器，由于是nwjs项目，关闭浏览器即可

```

``` json
// 在nwjs根目录下创建 package.json 文件，文件内容如下：
{
  "name": "我的音乐播放器",
  "main": "http://localhost:8080",
  "node-remote": "http://localhost",
  "nodejs": true,
  "window": {
    "width": 1020,
    "height": 600,
    "frame": false,
    "icon": "/icon.png"
  },
  "devDependencies": {
    "express": "^4.15.4",
    "musicmetadata": "^2.0.5"
  }
}
// 创建完成后，先执行 npm install 安装依赖，完成后双击nwjs目录下的 nw.exe 即可使用
```
### 打包编译

``` bash
# pack
npm run build
```
### 编译完成后，会在目录下生产 dist/ 文件夹，此时我们把 dist文件夹和index.html 这两个文件复制到nwjs根目录下
``` json
// 在nwjs根目录下创建 package.json 文件，文件内容如下：
{
  "name": "我的音乐播放器",
  "main": "index",
  "node-remote": "http://localhost",
  "nodejs": true,
  "window": {
    "width": 1020,
    "height": 600,
    "frame": false,
    "icon": "/icon.png"
  },
  "devDependencies": {
    "express": "^4.15.4",
    "musicmetadata": "^2.0.5"
  }
}
// 创建完成后，先执行 npm install 安装依赖，完成后双击nwjs目录下的 nw.exe 即可使用
```
 此时已经是可以直接执行的一个程序，至于nw更换图标和制作安装包的方法，自行百度咯，相关教程很多~

