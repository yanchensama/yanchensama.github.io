---
title: spring jdbcTemplate
abbrlink: f0c93e9c
date: 2022-04-17 20:36:55
tags:
---

![image-20220412233350542](../assets/img/spring/image-20220412233350542.png)

![image-20220412233951272](../assets/img/spring/image-20220412233951272.png)

![image-20220412234028635](../assets/img/spring/image-20220412234028635.png)

使用xml配置id的好处是解耦合，当类名变化时，只要改id标识就好

![image-20220413112131553](../assets/img/spring/image-20220413112131553.png)

就是说我们在执行`new ClassPathXmlApplicationContext("applicationBeanContext.xml")`时就会实例化所有的scope为songleton的对象，在`getBean("userDao")`时才会实例化scope为prototype的对象

![image-20220413115348586](../assets/img/spring/image-20220413115348586.png)

重点是无参构造方法

假设我们的业务层现在需要用到UserDao（是singleten的），我们会在service方法里会创建UserDao，没有依赖注入的话，就会是在实例化工厂时就创建了UserDao对象，然后再在service方法里重复创建（且两个是一样的）

就是下面这样

![image-20220413121537930](../assets/img/spring/image-20220413121537930.png)

![image-20220413123850417](../assets/img/spring/image-20220413123850417.png)所以我们需要使用注入的方法来将UserDao放到UserSerivce里面

注入方法（对象注入）：

- 通过set注入![image-20220413174017205](../assets/img/spring/image-20220413174017205.png)

在service里写个setUserDao方法

![image-20220413171920834](../assets/img/spring/image-20220413171920834.png)

配置文件里这样写，意思就是通过spring容器创建userService的时候，会调用这个class里的叫"set${name}"的有参方法，参数为ref引用的内容

可以用 p 简写，记得添加个xmlns:p命名空间，但是还是子标签好，因为清楚

![image-20220413174520058](../assets/img/spring/image-20220413174520058.png)

![image-20220413174553079](../assets/img/spring/image-20220413174553079.png)

- 构造方法注入

  ![image-20220413175350456](../assets/img/spring/image-20220413175350456.png)![image-20220413175521634](../assets/img/spring/image-20220413175521634.png)

构造注入，name为构造方法的参数

普通属性注入：

- 普通属性set注入（构造注入也行，set用得更多）

![image-20220413180723763](../assets/img/spring/image-20220413180723763.png)

![image-20220413180659483](../assets/img/spring/image-20220413180659483.png)

就从楼上的ref变成value就好

- 集合注入（Map，list，properties）：根据补全写就好，不行就现查

  都是打开bean的子标签property

  ![image-20220413183101177](../assets/img/spring/image-20220413183101177.png)

![image-20220413183002661](../assets/img/spring/image-20220413183002661.png)

![image-20220413183205503](../assets/img/spring/image-20220413183205503.png)



分模块开发：

![image-20220413195210310](../assets/img/spring/image-20220413195210310.png)

小结（重点）：

![image-20220413195252985](../assets/img/spring/image-20220413195252985.png)



Spring API：

![image-20220413195929150](../assets/img/spring/image-20220413195929150.png)

![image-20220413200334965](../assets/img/spring/image-20220413200334965.png)

getBean的两方法

![image-20220413201112422](../assets/img/spring/image-20220413201112422.png)

![image-20220413200857529](../assets/img/spring/image-20220413200857529.png)

第一种要强转，但是id唯一

第二种不用强转，但是遇到配置文件多个同class会报错

各有优点，看着用

![image-20220413201524863](../assets/img/spring/image-20220413201524863.png)

![image-20220413201748927](../assets/img/spring/image-20220413201748927.png)

关于数据库连接池的spring使用：

手动设置：

![image-20220414111412972](../assets/img/spring/image-20220414111412972.png)

![image-20220414111427984](../assets/img/spring/image-20220414111427984.png)

依赖配置文件：

![image-20220414111450977](../assets/img/spring/image-20220414111450977.png)

![image-20220414111517329](../assets/img/spring/image-20220414111517329.png)

使用spring

![image-20220414111621769](../assets/img/spring/image-20220414111621769.png)

![image-20220414111530547](../assets/img/spring/image-20220414111530547.png)

使用spring+配置文件

![image-20220414112019897](../assets/img/spring/image-20220414112019897.png)

![image-20220414111530547](../assets/img/spring/image-20220414111530547.png)

![image-20220414004946332](../assets/img/spring/image-20220414004946332.png)

![image-20220414010608917](../assets/img/spring/image-20220414010608917.png)

![image-20220414010814875](../assets/img/spring/image-20220414010814875.png)

![image-20220414194613998](../assets/img/spring/image-20220414194613998.png)

![image-20220414114559538](../assets/img/spring/image-20220414114559538.png)

注解使用（记得配置组件扫描才能找到注解）：

![image-20220414112119721](../assets/img/spring/image-20220414112119721.png)

![image-20220414112209798](../assets/img/spring/image-20220414112209798.png)

这个里面的setUserDao 方法是可以不写的，注解的方式 会暴力注入 ，但是官方不推荐

只写Autowired是按类型匹配，有Qualifier会按名称匹配，id是唯一的所以建议加Qualifier  （ReSource(name = "  xxx  ")  不就好了）

![image-20220414193412688](../assets/img/spring/image-20220414193412688.png)

普通属性赋值

![image-20220414193738935](../assets/img/spring/image-20220414193738935.png)

![image-20220414194049130](../assets/img/spring/image-20220414194049130.png)

![image-20220414194539442](../assets/img/spring/image-20220414194539442.png)

这些原始注解只能用在自定义的类上，遇到第三方就束手无策了（总不能去修改人家源吧），还有配置等等的也不能用上

新注解（完善不能替代的那部分）：

![image-20220414201240397](../assets/img/spring/image-20220414201240397.png)

原始注解在类上添加注解，新注解是专门添加一个config类来替换剩下的xml

待截图（config文件加下的两个类）



这个也就从xml文件获取spring容器更新为从config类获取spring容器![image-20220414205534191](../assets/img/spring/image-20220414205534191.png)

现在开始可以纯注解开发了

此外，Spring还集成了Junit，免去了测试类中每次都要生成容器 getBean的麻烦

![image-20220414211137771](../assets/img/spring/image-20220414211137771.png)

1.![image-20220414211607610](../assets/img/spring/image-20220414211607610.png)

junit 也要留着，不要删咯，还有版本太新要jdk11

2.![image-20220414213532303](../assets/img/spring/image-20220414213532303.png)

![image-20220415181947661](../assets/img/spring/image-20220415181947661.png)

集成web：

按之前的方式写完会发现每个servlet的get/post 都要生成一次spring容器，不妙，可以用静态方法或者监听来解决，官方推荐监听

- 自定义：

![image-20220415194120533](../assets/img/spring/image-20220415194120533.png)![image-20220415200052024](../assets/img/spring/image-20220415200052024.png)



![image-20220415194607628](../assets/img/spring/image-20220415194607628.png)

记得Web.xml 里添加配置监听![image-20220415194624239](../assets/img/spring/image-20220415194624239.png)

![image-20220415194717165](../assets/img/spring/image-20220415194717165.png)

然后还有这个applicationContext.xml也是写死的代码，不好，那么我么也可以把他加到servletContext里，来通过servletContext找到这个属性（在web.xml里写，设置个初始化参数）

![image-20220415195647596](../assets/img/spring/image-20220415195647596.png)

![image-20220415195817512](../assets/img/spring/image-20220415195817512.png)

然后发现app这个也是写死的，又要解耦合 ~~有点丧心病狂了~~， 这个通过写个工具类，通过工具类来获得

![image-20220415201752633](../assets/img/spring/image-20220415201752633.png)

然后就可以在get/post方法里使用这个工具类来获得这个app（spring应用上下文）

![image-20220415201506691](../assets/img/spring/image-20220415201506691.png)

- spring提供

好消息是，这繁琐的解耦不用你写，spring提供了

![image-20220415202245667](../assets/img/spring/image-20220415202245667.png)

导包

![image-20220415203223785](../assets/img/spring/image-20220415203223785.png)

配置

![image-20220415203252715](../assets/img/spring/image-20220415203252715.png)

使用

![image-20220415203326709](../assets/img/spring/image-20220415203326709.png)

