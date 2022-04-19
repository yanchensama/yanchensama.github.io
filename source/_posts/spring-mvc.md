---

title: spring mvc
date: 2022-04-15 23:13:58
tags:
---

Spring MVC:

![image-20220415230433853](../assets/img/spring-mvc/image-20220415230433853.png)

![image-20220415230600214](../assets/img/spring-mvc/image-20220415230600214.png)

1.导包：

![image-20220415231531849](../assets/img/spring-mvc/image-20220415231531849.png)

2.配置前端控制器 

![image-20220416003036074](../assets/img/spring-mvc/image-20220416003036074.png)

3.4.创建controller类和视图jsp（return默认forward，源码讲了    也可以写成  redirect:success.jsp   变成重定向）

![image-20220416003230345](../assets/img/spring-mvc/image-20220416003230345.png)

5.编写spring-mvc.xml![image-20220416003728411](../assets/img/spring-mvc/image-20220416003728411.png)



![image-20220416003446397](../assets/img/spring-mvc/image-20220416003446397.png)

这个是框架的底层逻辑，听说面试很喜欢考，不能理解也要硬记

![image-20220416004435957](../assets/img/spring-mvc/image-20220416004435957.png)

![image-20220416004635263](../assets/img/spring-mvc/image-20220416004635263.png)

![image-20220416010326638](../assets/img/spring-mvc/image-20220416010326638.png)看图，php的知识突然就蹦出来了（我依稀记得php也可以访问控制器名在访问方法名)，就悟了，原来是这么用啊，二级地址，控制分发，这才是我心目中的控制器类嘛

![image-20220416005251912](../assets/img/spring-mvc/image-20220416005251912.png)

小知识点:这样写也可以选定只扫描控制器，include 包含，还有种exclude，是不包含![image-20220416010947556](../assets/img/spring-mvc/image-20220416010947556.png)



这个东西，可以通过配置内部资源解析器来缩写成  `return "success"`（源码里的方法，改一改就可以很好用，但可读性也低了）

![image-20220416012434148](../assets/img/spring-mvc/image-20220416012434148.png)

![image-20220416012346195](../assets/img/spring-mvc/image-20220416012346195.png)

![image-20220416012839459](../assets/img/spring-mvc/image-20220416012839459.png)

刚才通过返回字符串跳转是一种方式

还有一种是通过返回ModelAndView跳转

![image-20220416111932454](../assets/img/spring-mvc/image-20220416111932454.png)

这里的ModelAndView由Springmvc容器注入（挺智能）

![image-20220416112315312](../assets/img/spring-mvc/image-20220416112315312.png)

返回字符串的方式加上设置model来返回数据![image-20220416113005143](../assets/img/spring-mvc/image-20220416113005143.png)

当然，旧方法通过设置request域也是可以用的，这个原生的request对象可以通过springmvc容器获得，

用框架了，这个方法就少用了，要解耦，不要直接接触

![image-20220416113712162](../assets/img/spring-mvc/image-20220416113712162.png)

![image-20220416114445193](../assets/img/spring-mvc/image-20220416114445193.png)

也可以通过@ResponseBody注解，来让springmvc把返回字符串不进行url拼接，而是作为要显示的字符串![image-20220416115549057](../assets/img/spring-mvc/image-20220416115549057.png)

![image-20220416115934770](../assets/img/spring-mvc/image-20220416115934770.png)



配置后也可以将类输出成json(需要导包jackson)

![image-20220416181418181](../assets/img/spring-mvc/image-20220416181418181.png)

![image-20220416180919095](../assets/img/spring-mvc/image-20220416180919095.png)

白学环节，

![image-20220416182517828](../assets/img/spring-mvc/image-20220416182517828.png)![image-20220416183601892](../assets/img/spring-mvc/image-20220416183601892.png)

![image-20220416183534185](../assets/img/spring-mvc/image-20220416183534185.png)

![image-20220416184030400](../assets/img/spring-mvc/image-20220416184030400.png)

从请求获得基本类型：![image-20220416185509576](../assets/img/spring-mvc/image-20220416185509576.png)

![image-20220416185526734](../assets/img/spring-mvc/image-20220416185526734.png)

从请求获得pojp类型： **只要参数名和属性名一致就能封进去**

![image-20220416185949792](../assets/img/spring-mvc/image-20220416185949792.png)

从请求获得数组类型：

![image-20220416190318835](../assets/img/spring-mvc/image-20220416190318835.png)

从请求获得集合类型：通过具有userList属性的VO类封装，然后get属性

![image-20220416231515243](../assets/img/spring-mvc/image-20220416231515243.png)

通过VO类（value object）就是封装数据的对象 来接收集合类型的数据  ，**只要参数名和属性名一致就能封进去**

![image-20220416234259746](../assets/img/spring-mvc/image-20220416234259746.png)

![image-20220416234107386](../assets/img/spring-mvc/image-20220416234107386.png)





从请求获得集合类型：ajax发送json类型的数据，可以直接封到List<User>,而不用VO封装

![image-20220416231246292](../assets/img/spring-mvc/image-20220416231246292.png)

![image-20220416224025190](../assets/img/spring-mvc/image-20220416224025190.png)

![image-20220416224100455](../assets/img/spring-mvc/image-20220416224100455.png)

小知识：如果是本地的静态资源文件，如js文件，因为他也是请求，会被分发器拦截，会在控制器类里找对应的地址执行对应的方法，当然会找不到，所以需要添加配置来让它去你的资源路径查找

![image-20220417000558564](../assets/img/spring-mvc/image-20220417000558564.png) 

或者使用这个配置，就是当你找不到对应路径的方法后，使用原始的servelt容器的机制来查找，而那个是可以找到静态资源的

![image-20220417000639286](../assets/img/spring-mvc/image-20220417000639286.png)

中文乱码问题：

![image-20220417001146788](../assets/img/spring-mvc/image-20220417001146788.png)

请求参数名和方法参数名不一致时，使用@requestParam进行绑定

![image-20220417002559820](../assets/img/spring-mvc/image-20220417002559820.png)

![image-20220417003025885](../assets/img/spring-mvc/image-20220417003025885.png)

什么是restful风格？

![image-20220417003318663](../assets/img/spring-mvc/image-20220417003318663.png)

![image-20220417003444349](../assets/img/spring-mvc/image-20220417003444349.png)

类型转换器：

![image-20220417004148346](../assets/img/spring-mvc/image-20220417004148346.png)

自定义转换器date

![image-20220417004822017](../assets/img/spring-mvc/image-20220417004822017.png)

![image-20220417005135200](../assets/img/spring-mvc/image-20220417005135200.png)

![image-20220417005421099](../assets/img/spring-mvc/image-20220417005421099.png)

获得原始Servelt api

![image-20220417010203146](../assets/img/spring-mvc/image-20220417010203146.png)

获得请求头：

![image-20220417010443229](../assets/img/spring-mvc/image-20220417010443229.png)

头里有个特殊的，叫cookie ，有专门的注释来获得

![image-20220417010655469](../assets/img/spring-mvc/image-20220417010655469.png)

文件上传：

![image-20220417011222792](../assets/img/spring-mvc/image-20220417011222792.png)

![image-20220417012129470](../assets/img/spring-mvc/image-20220417012129470.png)

![image-20220417012326262](../assets/img/spring-mvc/image-20220417012326262.png)

![image-20220417012339218](../assets/img/spring-mvc/image-20220417012339218.png)

![image-20220417012539696](../assets/img/spring-mvc/image-20220417012539696.png)

![image-20220417012700155](../assets/img/spring-mvc/image-20220417012700155.png)

多文件上传也一样的

![image-20220417013828513](../assets/img/spring-mvc/image-20220417013828513.png)

![image-20220417014032348](../assets/img/spring-mvc/image-20220417014032348.png)

![image-20220417014803829](../assets/img/spring-mvc/image-20220417014803829.png)

