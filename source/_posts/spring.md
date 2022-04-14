![image-20220412233350542](/home/joe/.config/Typora/typora-user-images/image-20220412233350542.png)

![image-20220412233951272](/home/joe/.config/Typora/typora-user-images/image-20220412233951272.png)

![image-20220412234028635](/home/joe/.config/Typora/typora-user-images/image-20220412234028635.png)

使用xml配置id的好处是解耦合，当类名变化时，只要改id标识就好

![image-20220413112131553](/home/joe/.config/Typora/typora-user-images/image-20220413112131553.png)

就是说我们在执行`new ClassPathXmlApplicationContext("applicationBeanContext.xml")`时就会实例化所有的scope为songleton的对象，在`getBean("userDao")`时才会实例化scope为prototype的对象

![image-20220413115348586](/home/joe/.config/Typora/typora-user-images/image-20220413115348586.png)

重点是无参构造方法

假设我们的业务层现在需要用到UserDao（是singleten的），我们会在service方法里会创建UserDao，没有依赖注入的话，就会是在实例化工厂时就创建了UserDao对象，然后再在service方法里重复创建（且两个是一样的）

就是下面这样

![image-20220413121537930](/home/joe/.config/Typora/typora-user-images/image-20220413121537930.png)

![image-20220413123850417](/home/joe/.config/Typora/typora-user-images/image-20220413123850417.png)所以我们需要使用注入的方法来将UserDao放到UserSerivce里面

注入方法（对象注入）：

- 通过set注入![image-20220413174017205](/home/joe/.config/Typora/typora-user-images/image-20220413174017205.png)

在service里写个setUserDao方法

![image-20220413171920834](/home/joe/.config/Typora/typora-user-images/image-20220413171920834.png)

配置文件里这样写，意思就是通过spring容器创建userService的时候，会调用这个class里的叫"set${name}"的有参方法，参数为ref引用的内容

可以用 p 简写，记得添加个xmlns:p命名空间，但是还是子标签好，因为清楚

![image-20220413174520058](/home/joe/.config/Typora/typora-user-images/image-20220413174520058.png)

![image-20220413174553079](/home/joe/.config/Typora/typora-user-images/image-20220413174553079.png)

- 构造方法注入

  ![image-20220413175350456](/home/joe/.config/Typora/typora-user-images/image-20220413175350456.png)![image-20220413175521634](/home/joe/.config/Typora/typora-user-images/image-20220413175521634.png)

构造注入，name为构造方法的参数

普通属性注入：

- 普通属性set注入（构造注入也行，set更多）

![image-20220413180723763](/home/joe/.config/Typora/typora-user-images/image-20220413180723763.png)

![image-20220413180659483](/home/joe/.config/Typora/typora-user-images/image-20220413180659483.png)

就从楼上的ref变成value就好

- 集合注入（Map，list，properties）：根据补全写就好，不行就现查

  都是打开bean的子标签property

  ![image-20220413183101177](/home/joe/.config/Typora/typora-user-images/image-20220413183101177.png)

![image-20220413183002661](/home/joe/.config/Typora/typora-user-images/image-20220413183002661.png)

![image-20220413183205503](/home/joe/.config/Typora/typora-user-images/image-20220413183205503.png)



分模块开发：

![image-20220413195210310](/home/joe/.config/Typora/typora-user-images/image-20220413195210310.png)

小结（重点）：

![image-20220413195252985](/home/joe/.config/Typora/typora-user-images/image-20220413195252985.png)



Spring API：

![image-20220413195929150](/home/joe/.config/Typora/typora-user-images/image-20220413195929150.png)

![image-20220413200334965](/home/joe/.config/Typora/typora-user-images/image-20220413200334965.png)

getBean的两方法

![image-20220413201112422](/home/joe/.config/Typora/typora-user-images/image-20220413201112422.png)

![image-20220413200857529](/home/joe/.config/Typora/typora-user-images/image-20220413200857529.png)

第一种要强转，但是id唯一

第二种不用强转，但是遇到配置文件多个同class会报错

各有优点，看着用

![image-20220413201524863](/home/joe/.config/Typora/typora-user-images/image-20220413201524863.png)

![image-20220413201748927](/home/joe/.config/Typora/typora-user-images/image-20220413201748927.png)

test1～test4  待截图



![image-20220414004946332](/home/joe/.config/Typora/typora-user-images/image-20220414004946332.png)

![image-20220414010608917](/home/joe/.config/Typora/typora-user-images/image-20220414010608917.png)

![image-20220414010814875](/home/joe/.config/Typora/typora-user-images/image-20220414010814875.png)

![image-20220414010935767](/home/joe/.config/Typora/typora-user-images/image-20220414010935767.png)

![image-20220414105909656](../assets/img/spring/image-20220414105909656.png)