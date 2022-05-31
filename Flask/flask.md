## 1.flask框架介绍
- 作者：Armin ronacher
- 诞生时间：2010年
- 组成：werkzueg + jinja2
  - werkzueg：专门用来处理请求相关的内容，比如：地址
  - jinja2：用来做页面渲染处理
  - 额外的扩展包：可以处理数据库的链接，站点管理，flask-cache做缓存处理

## 2.什么是虚拟环境
- 就是一个特殊的文件夹，里面存放着程序运行所需要的各种版本的python解释器，和各种框架的版本

## 3.查看哪些路由(地址)可以访问
- 格式：使用app.url_map, 返回的是app装饰的所有的路由和路径之间的映射关系
- 注：只有被app.url_map包含进来的路由(地址)才能被访问

## 4.app.run()参数问题
- 参数1：host，如果不指定，默认是127.0.0.1
- 参数2：port，如果不指定，默认是5000
- 参数3：debug，调试模式，默认是False
  - 如果设置为True，2个好处
    - 1.如果在运行的过程中，直接改动代码了，不需要重新启动程序，只需要ctrl+s保存即可部署程序
    - 2.如果程序报错会有提示

## 5.在访问路由的时候指定参数
- 格式：@app.route("/<类型:变量名>")
- 常见的参数类型
  - 整数 int
  - 浮点数 float
  - 字符串 path(默认就是path)

## 6.自定义参数类型(自定义转换器)
- 背景：
  - 如果系统提供的int,float等参数类型满足不了需求的时候，我们需要自定义
  - int,float,path可以接受不同的数据类型，是因为系统已经提供好对应的转换器了

- 自定义转换器格式：
  - 1.定义类：继承自BaseConverter
  - 2.重写init方法,接受两个参数
  - 3.初始化父类成员变量，还有子类自己的规则
  - 4.将转换器类，添加到系统默认的转换器列表

## 7.给路由增加其他的访问方式
- 格式：@app.route('路径',methods=['请求方式1','请求方式2',...])
- 常见的请求方式:
  - GET,POST,PUT,DELETE
  - 如果不指定请求方式,那么默认支持的是GET

## 8.视图函数返回响应(字符串)
- 直接返回响应体数据
  - return '字符串'
- 直接返回响应体数据+状态码
  - return '字符串',状态码
- 直接返回响应体数据+状态码+响应头信息
  - return '字符串',状态码,{'key':'value'}

## 9.通过jsonify返回json数据
- 格式：jsonify(dict)
- 简化格式：jsonify(key=value,key2=value2)

## 10.重定向
- 格式：redirect("地址"),地址可以是本地服务器的地址，也可以是其他服务器的地址
- 重定向的代号是302
- 重定向是两次请求

## 11.url_for
- 解释：称为反解析,返回视图函数对应路由地址
- 格式：url_for("视图函数",key=value)
- 常配合redirect使用，传递参数

## 12.request对象参数
- request.data:获取的是非表单(ajax)以post提交的数据
- request.form:获取的表单以post方式提交的数据
- request.args:获取的是问号后面的查询参数
- request.method:获取的请求方式
- request.url:获取请求地址
- request.files:获取的是input标签中type类型为file的文件

## 16.加载app程序运行参数
- 1.从配置类(对象)中加载
  - app.config.from_object(obj)
- 2.从配置文件中加载
  - app.config.from_pyfile(file)
- 3.从环境变量中加载(不常使用,需要依赖配置文件)
  - app.config.from_envvar()
- app.config:表示app程序运行所有的参数信息

## 17.请求钩子
- 解释：当访问正常视图函数的时候，顺带执行的方法
- 常见的请求钩子
  - 1.before_first_request:在处理第一个请求前执行
  - 2.before_request:在每次请求前执行，在该装饰函数中，一旦return，视图函数不再执行
  - 3.after_request:如果没有抛出错误，在每次请求后执行  
    接受一个参数：视图函数做出的响应
    在此函数中可以对响应值在返回之前做最后一步处理
  - 4.teardown_request:在每次请求后执行  
    接受一个参数：用来传递错误信息

## 18.cookie
- 解释：用来保持服务器和浏览器交互的状态的，由服务器设置，存储在浏览器
- 作用：用来做广告推送
- cookie的设置和获取
  - 设置cookie：response.set_cookie(key,value,max_age)
    - max_age:表示cookie在浏览器的存储时间，单位是秒
  - 获取cookie：request.cookies.get("key")

## 19.session
- 解释：服务器和用户来做状态保持的，里面存储的是敏感信息(比如身份证，登录信息).由服务器设置，存储在服务器
- 作用:用来做用户的登陆状态保存
- session的设置和获取
  - 设置session: session[key]=value
  - 获取session: session.get(key)
- 注意点
  - 1. session的存储依赖于cookie
  - 2. 存储在cookie中的sessionID需要加密,需要密钥(SECRET_KEY)

## 20.上下文
- 解释：就是一个容器
- 请求上下文
  - request：封装的是请求相关的数据
  - session：封装的是和用户相关的敏感信息
- 应用上下文(在项目中具体应用)
  - current_app:是app的一个代理对象，可以通过他获取app身上设置的各种属性,主要用在模块化开发中
  - g:一个局部的全局变量,主要用在装饰器中

## 21.Flask_script
- 解释：属于flask的扩展
- 作用：用来动态运行程序，配合flask_migrate做数据库迁移
- 使用格式：
  - 1.安装 pip install flask_script
  - 2.导入Manager类
  - 3.创建对象manager，管理app
  - 4.使用manager启动程序
    - 启动命令：python xxx.py runserver -h(host是ip地址) -p(端口号) -d(调试模式)

## 22.render_template
- 解释：属于jinja2的模板函数
- 好处
  - 1.以后的视图函数,只负责业务逻辑的处理，比如:数据库的增删改查
  - 2.以后数据的展示,全部都由jinja2的模板负责
- 使用格式
  - response = render_template("模板文件")

## 23.模板语法,获取变量
- 在模板中获取视图函数变量
- 格式:
  - {{变量}}
  - 如果字典使用[]获取，需要写成字符串，如果不是字符串，会当成变量对待

## 24.模板语法,分支循环判断
- 模板语法的种类
  - 分支格式
    ```html
    {% if 条件 %}
      语句1
    {% else %}
      语句2  
    {% endif %}
    ```
  - 循环格式
    ```html
    {% for 变量 in 容器%}
      语句
    {% endfor %}
    ```
  - 注释
    ```html
    {# 注释的内容 #}
    ```

## 25.系统字符串过滤器

- 解释：过滤器，用来过滤想要的数据
- 格式：{{ 字符串 | 字符串过滤器 }}
- 常见的字符串过滤器有：
  - title：将每个单词的首字母都大写
  - lower：将每个单词都小写
  - upper：将每个单词都大写
  - reverse：反转
  - ······

## 26.系统列表过滤器

- 解释：过滤器，用来过滤想要的数据
- 格式：{{ 列表 | 列表过滤器 }}
- 常见的字符串过滤器有：
  - first：获取列表第一个元素
  - last：获取列表最后一个元素
  - sum：列表和
  - length：列表长度
  - ······

