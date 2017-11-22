## webpack介绍
- webpack是一个资源的打包工具，分为1.0和2.0版本，可以将 .js,  .css , image等静态资源当做一个模块来进行打包，
- 每一种模块都是有一个对应的 loader来实现
- webpack 1.0版本官网：https://webpack.github.io/docs/usage.html
- webpack 2.0版本官网：https://webpack.js.org/
- 在这个项目中使用webpack 1.14.0

## node环境的安装
- webpack是基于nodejs运行的，所以在安装webpack之前必须先安装nodejs环境,安装步骤如下
    1. 去 https://nodejs.org/en/ 中下载当前操作系统匹配的版本,windows下软件名称通常叫做 node.exe
    2. 双击node.exe一路安装好，由于node.exe已经包含了npm工具，所以npm也能正常使用了
    3. 由于直接使用npm install 安装第三方包是去国外网站上下载，有可能会被墙而安装失败
        + 所以我们要将下载源切换到国内淘宝上因此需要利用 npm install nrm -g安装一个全局的nrm
    4. 安装好nrm以后，在cmd命令面板中输入： nrm use taobao 将下载源切换到淘宝，可以使用 nrm ls 查看当前使用的下载源
    5. 也可安装淘宝提供的类似于npm的工具 cnpm来替代npm安装node包,安装包命令和npm一样，安装cnpm命令： npm install cnpm -g

## webpack的安装
-  在cmd命令行面板中 执行： cnpm install webpack@1.14.0 -g 将webpack1.14.0版本安装为全局就能够在cmd命令行面板中使用webpack指令了 

## webpack中loader介绍
- webpack本身不支持css,less,sass,js,image等相关资源的打包工作的，它仅仅提供了一个基础的框架，
- 在这个框架上借助于相关的loader才可以实现css,less,sass,js,image等相关资源的打包工作

## webpack常用指令
- webpack 入口文件.js 输出文件.js
    + webpack         // 最基本的启动webpack的方法，默认查找名称为 webpack.config.js文件
    + webpack --config webpack.config.js    // 指定配置文件    
    + webpack -p      // 对打包后的文件进行压缩
    + webpack -d      // 提供source map，方便调式代码
## webpack.config.js配置文件的作用
- 如果只在cmd命令面板中输入 webpack指令，后面不跟任何参数的话，则默认查找的是 webpack.config.js文件，
- 在这个文件中可以配置入口文件，输出文件以及相关loader和插件等,以增强webpack的功能
- webpack.config.js文件结构如下:

```javascript
  var htmlwp = require('html-webpack-plugin'); // 导入html-webpack-plugin 包，用来根据模板自动生成index.html,用来实现热刷新
  module.exports={
  entry:'./src/main.js', // 定义打包的入口文件路径
  output:{
  	path:'__dirname/dist', //打包的文件存放目录(注意这里要求路径必须是绝对路径可以用__dirname来获取到当前目录所在的文件路径+当前文件夹名称) 
  	filename:'build.js'  //打包以后生成的文件名称(项目开发中最好就写build.js)
  },
  module:{
  	loaders:[
  		{
            test:/\.css$/,   //表示当前要打包的文件的后缀正则表达式,例:/\.less/等
            loader:'style-loader!css-loader',//此处填写下载打包文件需下载的文件包资源
  		}
  	]
  },
  plugins:[//实现热刷新要导入的代码
        new htmlwp({
          title: '首页',  //生成的页面标题
          filename: 'index.html', //webpack-dev-server在内存中生成的文件名称，自动将build注入到这个页面底部，才能实现自动刷新功能
          template: 'index1.html' //根据index1.html这个模板来生成(这个文件是自己生成的)
        })
```

## webpack相关配置
- 在使用loader之前需要在当前项目目录下打开cmd命令面板，输入: npm init 初始化一个 package.json文件来存放相关的loader包,要查看所需的包有没有下载完成即可查看package.json文件
#### 打包css资源演示

- webpack中使用css-loader和style-loader这两个loader来处理css资源的打包工作，所以使用前必须在项目中先安装这两个包:
    + cnpm i css-loader style-loader --save-dev

#### 在webpack.config.js中进行配置
```javascript
  var htmlwp = require('html-webpack-plugin');
module.exports={
    entry:'./src/main.js',  //指定打包的入口文件
    output:{
        path : __dirname+'/dist',  
        filename:'build.js'
    },
    module:{
        loaders:[
            {
                test:/\.css$/,//所有的.css结尾的文件被此loader处理
                loader:'style-loader!css-loader'//注意style-loader必须放到前面
            },
```
- 在main.js中导入新建的.css文件
- ​
```javascript
  require("/statics/css/site.css");
```
- 在当前目录下执行webpack命令即可.
#### 打包sass资源演示

- webpack中使用sass-loader，css-loader，style-loader来处理.scss文件的打包工作,
- 而sass-loader需要依赖于node-sass所以使用前必须在项目中先安装这些包，
- 要输入的命令是:
        cnpm install node-sass sass-loader css-loader style-loader --save-dev

#### 在webpack.config.js中进行配置
```javascript
     var htmlwp = require('html-webpack-plugin');
module.exports={
    entry:'./src/main.js', 
    output:{
        path : __dirname+'/dist', 
        filename:'build.js'
    },
    module:{
        loaders:[
            {
                test:/\.scss$/,//表示所有的.scss文件被此loader处理
                loader:'style-loader!css-loader!sass-loader'
            },
```
#### 在main.js中导入新建的.scss文件
require('../statics/css/site1.scss')
执行webpack命令即可打包完成,可以在build.js文件中查看刚打包完成的代码

#### 打包less资源演示
- 需要安装的node包有：
	+ css-loader：  编译css
	+ style-loader：编译css
	+ less-loader： 编译less
	+ less:  less-loader的依赖包
	
- 在项目根目录下打开cmd命令面板，输入：
        cnpm install less less-loader style-loader css-loader --save-dev 回车即可完成安装

#### 在webpack.config.js中进行配置
```javascript
     var htmlwp = require('html-webpack-plugin');
module.exports={
    entry:'./src/main.js',  
    output:{
        path : __dirname+'/dist', 
        filename:'build.js'
    },

    module:{
        loaders:[
             {
                test:/\.less$/,//表示所有的.less文件被此loader处理
                loader:'style-loader!css-loader!sass-loader!less-loader'
            },
```
#### 在main.js中导入新建的.less文件
- require('../statics/css/site2.less')
执行webpack命令即可打包完成
#### 打包url()请求的资源
- 需要安装的node包有：
	+ url-loader：打包通过url()方式的请求资源
	+ file-loader: url-loader的依赖loader
	
- 在项目根目录下打开cmd命令面板，输入：
        cnpm install url-loader file-loader --save-dev 回车即可完成安装
#### 在webpack.config.js中进行配置
```javascript
     var htmlwp = require('html-webpack-plugin');
module.exports={
    entry:'./src/main.js',  //指定打包的入口文件
    output:{
        path : __dirname+'/dist',  
        filename:'build.js'
    },
    module:{
        loaders:[
    //注意url可能会请求多个资源,所以在将来的项目中通过url导入的资源扩展名必须配置在这里
             {
                test:/\.(png|gif|jpg|ttf)$/,
                loader:'url-loader?limit=20000'
//loader:'url-loader'这种配置会将图片变成一个base64的字符存储到build.js中,如果图片很大的话,会赵成build.js也会比较大,加载的会导致效率低下,此时可以用limit=20000来限制图片大小,如果图片大小<20k则会把它当做base64字符串存储在 bulid.js中,>20k会单独讲图片存放在磁盘中,讲路径打包到bulid.js中
            },
```
#### 在main.js中导入新建的.less文件
        require('../statics/css/site3.css')
- 在项目根目录下打开cmd命令面板，输入：webpack  回车即可打包完成
- 检查是否成功分两种情况：
    1. 如果打包的图片大小大于配置文件中 url-loader?limit= 中的limit值的话，则会在目录下看到一张单独的一个图片
    2. 如果打包的图片大小小于等于配置文件中 url-loader?limit= 中的limit值的话，则会将图片以base64格式存储在build.js中
- 请按照上述两种情况去验证是否打包成功
### ECMAScript6语法转ECMAScript5语法
- 需要安装的node包有：
	+ babel-core
    + babel-loader
    + babel-preset-es2015
    + babel-plugin-transform-runtime：这个包主要是在打包.vue组件页面中的es6语法需要.
- 在项目根目录下打开cmd命令面板，输入：
        cnpm i babel-core@6.24.0 babel-loader@6.4.1 babel-plugin-transform-runtime@6.23.0 babel-preset-es2015@6.24.0 --save-dev 回车即可完成安装
#### 在webpack.config.js中进行配置
  ```javascript
  var htmlwp = require('html-webpack-plugin'); // 导入html-webpack-plugin 包，用来根据模板自动生成index.html
  module.exports={
  entry:'./src/main.js', // 定义打包的入口文件路径
  output:{
  	path:'__dirname+'/dist',   //打包的文件存放目录(注意这里要求路径必须是绝对路径可以用__dirname来获取到当前目录所在的文件路径+当前文件夹名称)
  	filename:'build.js'  // 打包以后生成的文件名称(项目开发中最好就写build.js)
  },
  module:{
  	loaders:[
  		{
  			// 将当前项目中所有的.js文件都要进行es6转es5操作，node_moudels除外
  			test:/\.js$/,   //表示当前要打包的文件的后缀正则表达式
  			// loader:'babel-loader?presets[]=es2015', //如果写到这里，将来在打包.vue文件的时候会报错，表示先利用css-loader解析.css文件，再调用style-loader打包
  			loader:'babel-loader',
  			exclude:/node_modules/  //node_modules中的所有.js文件不去转换，提高打包性能
  		}
  	]
  },
  babel:{//注意写到loaders外部(只能在webpack1.0中使用)
  	 presets: ['es2015'],  //表示es6转es5
  	 plugins: ['transform-runtime']  //这句代码就是为了解决打包.vue文件不报错
  },
```
- 在main.js中使用es6语法导入site.css
```javascript
import '../statics/css/site.css'
```
- 在项目根目录下打开cmd命令面板，输入：webpack  回车即可打包完成
- 检查build.js文件中，如果出现了类似于 require('../statics/css/site.css'); 但是看不到import '../statics/css/site.css' 表示转换成功
## webpack相关配置
### 利用webpack-dev-server实现热刷新配置
- 我们在修改了代码以后需要不断的重新执行webpack命令重新打包然后回到浏览器刷新页面去查看，这种开发效率很低下，
- 所以这里使用webpack-dev-server当代码更新的时候自动重新打包和刷新浏览器。
- 需要安装的node包有：
    + webpack-dev-server ： webpack开发服务器
    + html-webpack-plugin ：结合webpack在内存中自动生成index.html的入口文件
- 在项目根目录下打开cmd命令面板，输入：
        cnpm install webpack@1.14.0 webpack-dev-server@1.16.0 html-webpack-plugin  --save-dev 回车即可完成安装
- 在package.json文件中配置webpack-dev-server命令
```javascript
     "scripts": {
        "dev":"webpack-dev-server --inline --hot --open --port 4009"
      }
    // 参数说明：
    // inline :自动刷新
    // hot :热加载
    // port 指定监听端口为 5200
    // open : 自动在默认浏览器中打开
    // host： 可以指定服务器的ip，不指定默认为127.0.0.1(localhost)
```

- 复制此代码到package.json文件配置中,切记不能把代码粘贴到方法体内,必须放到方法体外部,这个package.json文件是自动生成的,无需手动创建,如下:
```javascript
{

  "scripts": {
    "dev": "webpack-dev-server --inline --hot --open --port 4009"
  },
  "devDependencies": {
    "css-loader": "^0.28.7",
    "file-loader": "^1.1.5",
    "html-webpack-plugin": "^2.30.1",
    "less": "^2.7.3",
    "less-loader": "^4.0.5",
    "node-sass": "^4.6.1",
    "sass-loader": "^6.0.6",
    "style-loader": "^0.19.0",
    "url-loader": "^0.6.2",
    "vue-loader": "^13.5.0",
    "vue-template-compiler": "^2.5.5",
    "webpack": "^3.8.1",
    "webpack-dev-server": "^2.9.4"
  }
}
```
#### 配置html-webpack-plugin组件

- webpack-dev-server要实现浏览器自动刷新，必须要利用html-webpack-plugin在内存中生成index.html页面才能实现
- html-webpack-plugin 配置步骤：
    1. 在webpack.config.js中加入如下代码：
        + 导入html-webpack-plugin 包,获取到插件对象
```javascript        
  var htmlwp = require('html-webpack-plugin');//注意要写到webpack.config.js的最顶部
        plugins:[///注意最好写到webpack.config.js的最底部
        new htmlwp({
          title: '首页',  //生成的页面标题
          filename: 'index.html', //webpack-dev-server在内存中生成的文件名称，自动将build注入到这个页面底部，才能实现自动刷新功能
          template: 'index1.html' //根据index1.html这个模板来生成(这个文件请自己生成)
        })
    ]
```
- 运行
    + 在cmd中执行npm run dev 命令,即可开启 webpack-dev-server服务器来运行vue项目
    + 这时候可以随便修改一个css样式，就会自动刷新看到效果    
#### 利用webpack解析和打包 .vue组件页面
- Vue项目中的每个页面其实都是一个.vue的文件，这种文件，Vue称之为组件页面，必须借助于 webpack的vue-loader才能使用
- 所以必须安装相关包：
    + vue-loader            ： .vue文件编译loader
    + vue-template-compiler ： .vue模板编译,被vue-loader所依赖
    + babel-plugin-transform-runtime : es6实时转换成es5语法
        
1. 在项目根目录下打开cmd命令面板，输入：
            npm install vue-loader vue-template-compiler babel-plugin-transform-runtime --save-dev 回车即可完成安装
       
2. 在webpack.config.js中添加如下配置（只能在webpack1.0中使用）：
```javascript
babel:{
		 presets: ['es2015'],  
		 plugins: ['transform-runtime']  //这句代码就是为了解决打包.vue文件不报错
	}
```
	
- 在webpack2.0中在webpack.config.js中添加 babel:{}是不认识的，要改成如下方式：
- 在项目根目录下新建 .babelrc文件，内容填写如下：
```javascript
     {
		 presets: ['es2015'],  
		 plugins: ['transform-runtime']  //这句代码就是为了解决打包.vue文件不报错
	}
```

3. 在webpack.config.js中的loaders中增加
```javascript
            {
				// 打包.vue文件
				test:/\.vue$/,   //表示当前要打包的文件的后缀正则表达式
				loader:'vue-loader' //
			}
```
- .vue组件页面的写法结构

    1. <template><div class="tmpl"></div>由于是vue2.0 所以这个里面一定要放一个根元素，可以放vue的指令 v-</template>

    2. <script> export default{data:{}} -> new Vue({data:{}}) 就是导出一个 Vue的实例  </script>

    3. <style scoped></style> 中的样式是当前组件的

- 将.vue中的内容解析编译并且展示在浏览器中

    1. cnpm install vue  --save
    2. 在main.js中编写解析.vue的代码
```javascript    
        // 1.0 导入vue这个包
        import Vue from 'vue';

        // 2.0 导入 App.vue文件
        import App from './App.vue';

        // 3.0 将App中的内容编译解析出来替换index.html中的<div id="app"></div>
        new Vue({
            el:'#app',
            // render:function(create){create(App);}  es5语法
            render:create=>create(App) //es6语法
        });
```
#### 项目中使用的ECMAScript6语法总结
1. 对象的写法
    + es5中对象： {add:add,substrict:substrict}
    + es6中对象： {add,substrict}  注意这种写法的属性名称和值变量是同一个名称才可以简写，否则要想es5那样的写法,例如： {addFun:add}

2. 在对象中的方法的写法
    + es5中对象： {add:function(){},substrict:function(){}}
    + es6中对象： {add(){},substrict(){}}
    
3. 对象的导出写法
- es5两种形式：
    1. module.exports = fucntion (){};
    2. exprots.add =  fucntion (){};

- es6中写法：
```javascript
export default{
            add(){}
        }
    export fucntion add(){} //相当于 将add方法当做一个属性挂在到exports对象
```


4. 对象的导入
- es5: var add  = require('./calc.js');
- es6:如果导出的是：export default{ add(){}}
    + 那么可以通过  import obj from './calc.js'
- 如果导出的是：
    + export fucntion add(){} 
    + export fucntion substrict(){} 
    + export const PI=3.14
- 那么可以通过按需加载 import {add,substrict,PI} from './calc.js'

5. es6中的箭头函数的写法
```javascript
//用箭头函数实现 =>读 goes to
//function(created){ created(App) }//es5的写法
c=c(App);//es6的写法
```
## webpack项目结构中集成vue-router(路由)步骤（Vue2.0写法）
- 1、安装cnpm install vue-loader vue-template-compiler --save-dev和cnpm intall vue --save
- 2、在webpack打包入口js文件中配置如下:
```javascript
   import Vue from 'vue'; //导入vue包
   import VueRouter from 'vue-router;
   Vue.use(VueRouter); //在Vue中使用路由
     //导入路由组件
     import home from "./components/home.vue"
     import shopcar from "./components/shopcar/shopcar.vue"
    //定制路由规则
    var router=new vueRouter({
    routes:[
        {path:"/",redirect:"/home"},//打开页面就把页面转向/home地址
        //redirect方法是把请求地址转向另外一个地址
        {path:"/home",component:home},
        {path:"/shopcar",component:shopcar},
    ]
    });
    //实例化vue对象
    new Vue({
    el:"#app",
    render:c=>c(App),//把App.vue整个文件当做跟组件进行渲染
    router:router//使路由生效
    });
```
## Vue移动组件mint-ui使用
- 官网：http://mint-ui.github.io
1. 利用：npm install mint-ui --save 命令将mint-ui安装到项目中
2. 在main.js中导入mint-ui
```javascript   
import "imnt-ui/lib/style.min.css"//导入mint-ui的样式文件
import mintUI from 'mint-ui'
Vue.use(mintUI)//进行绑定
```
## Vue中使用MUI
- MUI是最接近原生APP体验的高性能前端框架，MUI不依赖任何第三方JS库，压缩后的JS和CSS文件仅有100+K和60+K , 我们项目中主要使用它的css布局，对于js特效没有用到
- MUI官网：http://dev.dcloud.net.cn/mui/
- 在main.js中导入mui相关的css
 import '../statics/mui/css/mui.css'
