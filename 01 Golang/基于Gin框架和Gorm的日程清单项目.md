# 基本介绍

这个项目是我出于学习的目的做练手项目，主要实现的功能比较简单，是日程清单的一些增删改查。整体是一个基于 GIN WEB API 框架的[RESTful](https://blog.csdn.net/MOKEXFDGH/article/details/87378554#RESTful_1165) 设计风格的前后端分离goweb应用，数据库是用GORM 连接mysql数据库。

**后端MVC**：客户端发送请求 -> 服务器触发controller -> 服务器进行model各种操作 -> 服务器根据model数据渲染view -> 服务器回复请求，包含了整个view的HTML -> 客户端重新渲染整个页面，重新计算所有CSS，重新执行所有JS，重新请求所有的资源。

![](https://cdn.nlark.com/yuque/0/2022/png/26705216/1650610016922-d7eedd0e-3bec-4356-bf3a-65bb43012544.png)

url --> controller --> logic --> model  
请求来了 --> 控制器 --> 业务逻辑 --> 模型层的增删改查
```

controller 控制器
func CreateTodo(c *gin.Context) {
	//前端页面填写待办事项 点击提交 会发送请求到这
	//1. 从请求中把数据拿出来
	var todo models.Todo
	err := c.BindJSON(&todo)
	if err != nil {
		return
	}
	//2. 存入数据库
	//3. 返回响应
	err = models.CreateATodo(&todo)
	if err != nil {
		c.JSON(http.StatusOK, gin.H{"error": err.Error()})
	} else {
		c.JSON(http.StatusOK, todo)
	}
}

路由

func SetRouters() *gin.Engine {
	//默认路由
	r := gin.Default()
	//告诉框架引用的静态文件去哪里找
	r.Static("/static", "static")
	//告诉gin框架去哪里找模板文件
	r.LoadHTMLGlob("templates/*")
	r.GET("/", controller.IndexHandler)
	v1Group := r.Group("v1")
	{ //代办事项
		//添加
		v1Group.POST("/todo", controller.CreateTodo)
		//查看所有代办事项
		v1Group.GET("/todo", controller.GetTodoList)
		//修改
		v1Group.PUT("/todo/:id", controller.UpdateATodo)
		//删除
		v1Group.DELETE("/todo/:id", controller.DeleteATodo)
	}
	return r
}
```

# 遇到的困难

**遇到的困难**：对前端的知识不是那么了解，在开发过程中，遇到一些bug，不确定是前端的问题还是后端的，

**如何定位判断是前端的bug还是后端的bug？**

1.通常可以利用抓包工具**wireshark** 来进行分析

(1)传参内容是否正确

如果传参内容不正确，定位为前端的bug。

(2)响应内容是否正确

如果响应内容不正确，为后端bug。

2.前后端bug各有什么样的特殊性质

(1)前端bug特性：界面相关，布局相关，兼容性相关，交互相关。

(2)后端bug特性：数据相关，安全性相关，逻辑性相关，性能相关。

3.定位BUG属于前端还是后端，常用的有以下2种方法：

（1）查看http请求参数和响应结果。

（2）查看后端服务log日志有无错误日志信息。

# RESTful设计风格

REST与技术无关，代表的是一种软件架构风格，REST是Representational State Transfer的简称，中文翻译为“表征状态转移”或“表现层状态转化”。

REST的含义就是客户端与Web服务器之间进行交互的时候，使用HTTP协议中的4个请求方法代表不同的动作。

-   GET用来获取资源
-   POST用来新建资源
-   PUT用来更新资源
-   DELETE用来删除资源。

只要API程序遵循了REST风格，那就可以称其为RESTful API。目前在前后端分离的架构中，前后端基本都是通过RESTful API来进行交互。

![](https://cdn.nlark.com/yuque/0/2022/png/26705216/1649123495769-0854aa86-424d-477b-8d5e-022571592802.png)

Gin框架支持开发RESTful API的开发。

# Gin框架相关

## Gin路由

-   gin 框架中采用的路由库是基于httprouter做的，基本原理就是构造一个路由地址的前缀树
-   httproter会将所有路由规则构造一颗前缀树：压缩前缀树 ：更节省空间的Trie（前缀树）
-   golang的web框架[echo](https://github.com/labstack/echo)和[gin](https://github.com/gin-gonic/gin)都使用了radix tree作为路由查找的算法

## Gin渲染

## Gin中间件

Gin框架允许开发者在处理请求的过程中，加入用户自己的钩子（Hook）函数。这个钩子函数就叫中间件，中间件适合处理一些公共的业务逻辑，比如登录认证、权限校验、数据分页、记录日志、耗时统计等。

定义中间件

Gin中的中间件必须是一个gin.HandlerFunc类型。例如我们像下面的代码一样定义一个统计请求耗时的中间件。
```

// StatCost 是一个统计耗时请求耗时的中间件
func StatCost() gin.HandlerFunc {
	return func(c *gin.Context) {
		start := time.Now()
		c.Set("name", "小王子") // 可以通过c.Set在请求上下文中设置值，后续的处理函数能够取到该值
		// 调用该请求的剩余处理程序
		c.Next()
		// 不调用该请求的剩余处理程序
		// c.Abort()
		// 计算耗时
		cost := time.Since(start)
		log.Println(cost)
	}
}
```

注册中间件

在gin框架中，我们可以为每个路由添加任意数量的中间件。

中间件注意事项

gin默认中间件

gin.Default()默认使用了Logger和Recovery中间件，其中：

Logger中间件将日志写入gin.DefaultWriter，即使配置了GIN_MODE=release。

Recovery中间件会recover任何panic。如果有panic的话，会写入500响应码。

如果不想使用上面两个默认的中间件，可以使用gin.New()新建一个没有任何默认中间件的路由。

gin中间件中使用goroutine

当在中间件或handler中启动新的goroutine时，不能使用原始的上下文（c *gin.Context），必须使用其只读副本（c.Copy()）。

# Gorm框架相关

在使用ORM工具时，通常我们需要在代码中定义模型（Models）与数据库中的数据表进行映射，在GORM中模型（Models）通常是正常定义的结构体、基本的go类型或它们的指针。 同时也支持sql.Scanner及driver.Valuer接口（interfaces）。

### gorm.Model

为了方便模型定义，GORM内置了一个gorm.Model结构体。gorm.Model是一个包含了ID, CreatedAt, UpdatedAt, DeletedAt四个字段的Golang结构体。

![](https://cdn.nlark.com/yuque/0/2022/png/26705216/1649147408853-f97eebdd-90c8-4d77-9d76-88313cfd08f9.png)

  

-   **全功能 ORM**
-   **关联 (拥有一个，拥有多个，属于，多对多，多态，单表继承)**
-   **Create，Save，Update，Delete，Find 中钩子方法**
-   **支持 Preload、Joins 的预加载**
-   **事务，嵌套事务，Save Point，Rollback To to Saved Point**
-   **Context、预编译模式、DryRun 模式**
-   **批量插入，FindInBatches，Find/Create with Map，使用 SQL 表达式、Context Valuer 进行 CRUD**
-   **SQL 构建器，Upsert，锁，Optimizer/Index/Comment Hint，命名参数，子查询**
-   **复合主键，索引，约束**
-   **自动迁移**
-   **自定义 Logger**
-   **灵活的可扩展插件 API：Database Resolver（多数据库，读写分离）、Prometheus…**
-   **每个特性都经过了测试的重重考验**
-   **开发者友好**

# POSTMAN

Postman是google开发的一款功能强大的**网页调试与发送网页HTTP请求**，并能运行测试用例的的Chrome插件。其主要功能包括：

**模拟各种HTTP requests**

从常用的 GET、POST 到 RESTful 的 PUT 、 DELETE …等等。 甚至还可以发送文件、送出额外的 header。

**Collection 功能（测试集合）**

Collection 是 requests的集合，在做完一個测试的時候， 你可以把這次的 request 存到特定的 Collection 里面，如此一來，下次要做同样的测试时，就不需要重新输入。而且一个collection可以包含多条request，如果我们把一个request当成一个test case，那collection就可以看成是一个test suite。通过collection的归类，我们可以良好的分类测试软件所提供的API.而且 Collection 还可以 Import 或是 Share 出來，让团队里面的所有人共享你建立起來的 Collection。

**人性化的Response整理**

一般在用其他工具來测试的時候，response的内容通常都是纯文字的 raw， 但如果是 JSON ，就是塞成一整行的 JSON。这会造成阅读的障碍 ，而 Postman 可以针对response内容的格式自动美化。 JSON、 XML 或是 HTML 都會整理成我们可以阅读的格式

**内置测试脚本语言**

Postman支持编写测试脚本，可以快速的检查request的结果，并返回测试结果

**设定变量与环境**

Postman 可以自由 设定变量与Environment，一般我们在编辑request，校验response的时候，总会需要重复输入某些字符，比如url，postman允许我们设定变量来保存这些值。并且把变量保存在不同的环境中。比如，我們可能会有多种环境， development 、 staging 或 local， 而这几种环境中的 request URL 也各不相同，但我们可以在不同的环境中设定同样的变量，只是变量的值不一样，这样我们就不用修改我们的测试脚本，而测试不同的环境。