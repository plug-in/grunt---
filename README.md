#grunt安装教程

>熟练应用grunt工具，可为以后的开发做基础准备，节省人力；

##一、需要的工具及环境（node.js+grunt+插件库）

node.js、grunt、grunt插件库三者之间的关系如下：

1、node.js有一个npm的命令包，所以只有正确安装node.js后才可以执行npm命令；

2、而grunt是运行在node.js环境下的一个工具，因此必须先安装node.js;

3、grunt构建工具中的less插件是grunt工具中众多插件中的一个插件，一定在grunt安装后，在此基础上安装less插件即
可在命令窗口中执行lessc命令（例如命令行：lessc nav/page.less>nav/page.css）;

grunt的插件库：https://github.com/gruntjs/；这里面有n多个grunt的插件，可以选自己需要的来用


综上所述：node.js、grunt、lessc是相互依存的关系；


##二、grunt安装流程及配置

第一次制作时步骤如下：

在安装grunt之前需要先安装node.js;

2.1、安装node.js

	下载地址：http://nodejs.org/

此时命令行中输入npm回车；如出现npm不是可执行命令提示，说明你未成功安装node.js

解决方法：1）卸载node，重新安装；
2）检查环节变量，找到带有npm的环境变量，在其后添加：node.js字样；重启机器;

2.2、安装grunt工具步骤：

1）首先安装Grunt的命令行接口

在命令窗口输入如下命令

	npm install -g grunt-cli

2）安装grunt-init（是一个用于自动创建项目的脚手架工具）

在命令窗口输入如下命令

	npm install -g grunt-init

这样，会把grunt-init命令植入到你的系统路径，从而允许你在任何目录中都可以运行它。

3）安装和使用grunt-init模板

	下载地址：https://github.com/clientlab/grunt-init-gruntfile.git

	（这个是sos组和我们组共同维护的一个grunt-init模板）

3.1）下载下来的模板名为grunt-init-gruntfile-master.zip；首先解压文件并更名为：**gruntfile**

目录如下：

	gruntfile/
	gruntfile/README.md
	gruntfile/template.js
	gruntfile/root/

3.2）打开你的“C:\Users\你的用户名”这个目录（例如C:\Users\100142）；执行如下命令

		mkdir grunt-init

这是会出现一个”grunt-init“文件夹；

3.3）把模板拷贝到”grunt-init“文件夹下；

3.4）在与你项目外（例如dsp项目外）,按Shift+鼠标右键，打开右键菜单，选择“在此处打开命令窗口(W)”，打开命令行，执行如下程序：

	grunt-init gruntfile


如果执行正确，命令窗口中会出现一些提示文字（这相当于grunt插件选择，如果提到你需要的插件,回答Y即可，下面是以”web开发“需要的插件选择为例）：


1、Grunt项目的名称：（这里我们开发的是dsp项目即输入dsp即可） dsp

2、Grunt项目的详细描述：（可以简单写一些描述，最好用英文）the website plug-in library

3、是否需要创建一个 package.json 文件？ Y

4、是否需要使用清理文件的任务？Y

5、是否需要使用文件复制的任务？ Y

6、是否需要使用查找替换点位符的过滤器任务？ N

7、是否需要使用合并文件的任务？ Y

8、是否需要使用生成JSDoc的任务？N

9、是否需要使用代码压缩的任务？ N

10、是否需要使用javascript语法检查的任务？ Y

11、是否需要使用QUnit的单元测试任务？N

12、是否需要使用Less任务？ Y

13、是否需要使用CSS语法检查任务？Y

回车，再看项目文件夹中会出现两个文件，Gruntfile.js和package.json


以上的工作完成，你已经拥有了一个我们要用到的的grunt的全部；


##开始配置:Gruntfile.js和package.json


###1、Gruntfile.js

打开Gruntfile.js文件，按如下设置修改；

    module.exports = function(grunt) {
    
    grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    clean: {//清除插件的配置
      dist: ['dist/path']
    }, 
    less: {//less插件的配置
      development:{
    options: {
      rootpath:'',
      paths: ["src"],
      cleancss:true,
      ieCompat:true,
      strictUnits:true,//验证所使用的单位
    },
    files: {
      "path/css/base.css": "path/less/base.less",
      "path/css/dsp.css": "path/less/dsp.less"
    }
      },
    },
      jshint: {
      all: ['path/dsp.js']
    },
      copy: {//复制插件的配置
    dist: {
      files: [ 
    {
      expand: true,
      cwd: 'src',
      src:['path/**'],
      dest:'dist/'
    }
      ]
    }
      },
     
      });
      grunt.loadNpmTasks('grunt-contrib-clean');//清除插件
      grunt.loadNpmTasks('grunt-contrib-less');//less转换为css的插件
      grunt.loadNpmTasks('grunt-contrib-jshint');//检查js语法检查插件
      grunt.loadNpmTasks('grunt-contrib-copy');//复制插件
     
    
      // Default task.
      grunt.registerTask('default', ['clean','less','jshint','copy']);//任务请求顺序列表；插件名称排列的位置决定着执行顺序；
    
    };

ps:一般我们会建一个src文件夹（用来存放源文件，即我们日常维护的文件包），grunt会给我们构建出一个dist文件夹（用来分发到各个项目中的文件；）

以上是基础设置，在以后使用时可以根据需求自己修改；

###2、package.json（一组和Gruntfile相对应的插件信息）
    {
      "name": "plug-in",
      "description": "the website plug-in library",
      "devDependencies": {
	    "grunt": "~0.4.1",
	    "grunt-contrib-clean": "~0.4.0",//清除插件及版本号
	    "grunt-contrib-copy": "~0.4.0",//复制插件及版本号
	    "grunt-contrib-less": "^0.10.0",//less插件及版本号
	    "grunt-contrib-jshint": "^0.8.0"//js语法检查插件及版本号
      }
    }

以上两个文件的配置会根据需求的不同而不同，我们一般用到的插件是clean、copy、less等；

##配置结束后，需要根据配置文件安装插件

执行命令：

    npm install

执行后可以看到文件中生成了一个”node_modules“，因为文件较大一般会在项目文件外执行；

这个命令只需要在最初安装时执行一次（这相当于你安装了一个软件，只需安装一次，以后就可以直接拿来用了），以后可以直接用grunt命令构建项目即可

在以后的工作中会频繁的用到如下命令

    grunt

好了，一个完整的grunt构建配置就完成了~so easy吧~
