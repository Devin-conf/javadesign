#一 java 设计模式


### 1 、通过对节课内容的学习，了解设计原则的重要性。

### 2 、掌握七大设计原则的具体内容。

### 内容定位

### 学习设计原则，学习设计模式的基础。在实际开发过程中，并不是一定要求所有代码都遵循设计原

### 则，我们要考虑人力、时间、成本、质量，不是刻意追求完美，要在适当的场景遵循设计原则，体现的

### 是一种平衡取舍，帮助我们设计出更加优雅的代码结构。

### 开闭原则

### 开闭原则（Open-ClosedPrinciple,OCP）是指一个软件实体如类、模块和函数应该对扩展开放，

### 对修改关闭。所谓的开闭，也正是对扩展和修改两个行为的一个原则。强调的是用抽象构建框架，用实

### 现扩展细节。可以提高软件系统的可复用性及可维护性。开闭原则，是面向对象设计中最基础的设计原

### 则。它指导我们如何建立稳定灵活的系统，例如：我们版本更新，我尽可能不修改源代码，但是可以增

### 加新功能。

### 实现开闭原则的核心思想就是面向抽象编程，  在spring 源码中， 过滤器和拦截器都是采用这种思想
### 比如实现一个可扩展的验证接口，如何实现

1、定义一个可以扩展验证接口

```java
public interface IMyValidate {
    
    void myValudate();
}

```

2.自定义两个自定义的验证类

```java
/**
 * @auther: liudaping
 * @description: TODO
 * @date: 2021-05-31
 * @since 1.0.0
 */
public class MyValidate implements IMyValidate {

    @Override
    public void myValudate() {
        System.out.println("this is my validate-11--");
    }
}

```

```java
/**
 * @auther: liudaping
 * @description: TODO
 * @date: 2021-05-31
 * @since 1.0.0
 */
public class MyValidate2 implements IMyValidate {
    
    @Override
    public void myValudate() {
        System.out.println("this is my validate-22--");
    }
}

```
3.验证框架，用list容器加载， spring里面是用chain


```java
import com.google.common.collect.Lists;

import java.util.List;

/**
 * @auther: liudaping
 * @description: TODO
 * @date: 2021-05-31
 * @since 1.0.0
 */
public  class OpenCloseValidate {

    List<IMyValidate> iMyValidateList = Lists.newArrayList();

    public void validate(){
        systemValidate1();
        systemValidate2();
        systemValidate3();
        for (IMyValidate iMyValidate : iMyValidateList ) {
            iMyValidate.myValudate();
        }
    }


    public void systemValidate1(){
        System.out.println("this is sys validate-11--");

    }

    public void systemValidate2(){
        System.out.println("this is sys validate-22--");

    }

    public void systemValidate3(){
        System.out.println("this is sys validate-33--");

    }


    public void setiMyValidateList(List<IMyValidate> iMyValidateList) {
        this.iMyValidateList = iMyValidateList;
    }
}


```

4. 模拟调用
```java

import com.google.common.collect.Lists;

import java.util.List;

/**
 * @auther: liudaping
 * @description: TODO
 * @date: 2021-05-31
 * @since 1.0.0
 */
public class Invoke {


    public static void main(String[] args) {

        OpenCloseValidate openClose = new OpenCloseValidate();

        //spring bean 加载
        List<IMyValidate> iMyValidates = Lists.newArrayList(new MyValidate(),new MyValidate2());
        //spring set 注入
        openClose.setiMyValidateList(iMyValidates);
        //调用
        openClose.validate();


    }
}


```



### 依赖倒置原则

### 依赖倒置原则（DependenceInversionPrinciple,DIP）是指设计代码结构时，高层模块不应该依

### 赖底层模块，二者都应该依赖其抽象。抽象不应该依赖细节；细节应该依赖抽象。通过依赖倒置，可以

### 减少类与类之间的耦合性，提高系统的稳定性，提高代码的可读性和可维护性，并能够降低修改程序所



### 什么是依赖

### 什么是上层模块和下层模块

### 什么是抽象和细节

1 汽车是抽象， 宝马是细节  
2 终端是抽象，人 机 环是细节  
3 群组是抽象，分类别群组是细节


### 作用 
1. 可以降低类间的耦合性。
2. 可以提高系统的稳定性。
3. 可以减少并行开发引起的风险。
4. 可以提高代码的可读性和可维护性

### 实现方式
使用接口或者抽象类的目的是制定好规范和契约，而不去涉及任何具体的操作，把展现细节的任务交给它们的实现类去完成。所以我们在实际编程中只要遵循以下4点，就能在项目中满足这个规则。

1. 每个类尽量提供接口或抽象类，或者两者都具备。
2. 变量的声明类型尽量是接口或者是抽象类。
3. 任何类都不应该从具体类派生。
4. 使用继承时尽量遵循里氏替换原则

```java
public class Tom{
public void studyJavaCourse(){
System.out.println("Tom在学习Java的课程");
}

public void studyPythonCourse(){
System.out.println("Tom在学习Python的课程");
}
}

```

### 来调用一下：

```java
public static void main(String[]args){
Tom tom=new Tom();
tom.studyJavaCourse();
tom.studyPythonCourse();
}
```




### 学习增加课程场景

### 来我们优化代码，创建一个课程的抽象ICourse接口：

```java
public interface ICourse{
void study();
}
```


### 然后写JavaCourse类：
```java
public class JavaCourse implements ICourse{
@Override
public void study(){
System.out.println("Tom在学习Java课程");
}
}
```


### 再实现PythonCourse类：

```java
public class PythonCourse implements ICourse{
@Override
publicvoidstudy(){
System.out.println("Tom在学习Python课程");
}
}

```


### 修改Tom类：

```java
public class Tom{
    
public void study(ICourse course){
course.study();
}
}

```


### 来看调用：

```java
public static void main(String[]args){
Tom tom = new Tom();
tom.study(newJavaCourse());
tom.study(newPythonCourse());
}

```


### 我们这时候再看来代码，Tom的兴趣无论怎么暴涨，对于新的课程，我只需要新建一个类，通过传

### 参的方式告诉Tom，而不需要修改底层代码。

### 大家要切记：以抽象为基准比以细节为基准搭建起来的架构要稳定得多，因此大家在拿到需求之后，

### 要面向接口编程，先顶层再细节来设计代码结构。



### 单一职责原则

### 单一职责（SimpleResponsibilityPinciple，SRP）是指不要存在多于一个导致类变更的原因。假

### 设我们有一个Class负责两个职责，一旦发生需求变更，修改其中一个职责的逻辑代码，有可能会导致

### 另一个职责的功能发生故障。这样一来，这个Class存在两个导致类变更的原因。如何解决这个问题呢？

### 我们就要给两个职责分别用两个Class来实现，进行解耦。后期需求变更维护互不影响。这样的设计，

### 可以降低类的复杂度，提高类的可读性，提高系统的可维护性，降低变更引起的风险。总体来说就是一

### 个Class/Interface/Method只负责一项职责。

### 接下来，我们来看代码实例，还是用课程举例，我们的课程有直播课和录播课。直播课不能快进和

### 快退，录播可以可以任意的反复观看，功能职责不一样。还是先创建一个Course类：

```java
public class Course{
public void study(StringcourseName){
if("直播课".equals(courseName)){
System.out.println("不能快进");
}else{
System.out.println("可以任意的来回播放");
}
}
```


### 看代码调用：

```java
publicstaticvoidmain(String[]args){
Coursec ourse=new Course();
course.study("直播课");
course.study("录播课");
}

```


### 从上面代码来看，Course类承担了两种处理逻辑。假如，现在要对课程进行加密，那么直播课和录

### 播课的加密逻辑都不一样，必须要修改代码。而修改代码逻辑势必会相互影响容易造成不可控的风险。

### 我们对职责进行分离解耦，来看代码，分别创建两个类ReplayCourse和LiveCourse：

### LiveCourse类：
```java
public class LiveCourse{
public void study(StringcourseName){


System.out.println(courseName+"不能快进看");
}
}

```



### ReplayCourse类：

```java

public class ReplayCourse{
public void study(StringcourseName){
System.out.println("可以任意的来回播放");
}
}
```


### 调用代码：

```java
public static void main(String[]args){
LiveCourse liveCourse =new LiveCourse();
liveCourse.study("直播课");

ReplayCoursere playCourse=new ReplayCourse();
replayCourse.study("录播课");
}

```


### 业务继续发展，课程要做权限。没有付费的学员可以获取课程基本信息，已经付费的学员可以获得

### 视频流，即学习权限。那么对于控制课程层面上至少有两个职责。我们可以把展示职责和管理职责分离

### 开来，都实现同一个抽象依赖。设计一个顶层接口,创建ICourse接口：

```java


public interface ICourse{

```
//获得基本信息
String getCourseName();
```
```
//获得视频流
byte[]getCourseVideo();
```
//学习课程
voidstudyCourse();
//退款
voidrefundCourse();
}


```

### 我们可以把这个接口拆成两个接口，创建一个接口ICourseInfo和ICourseManager：

### ICourseInfo接口：

```java

public interface ICourseInfo{
String getCourseName();
byte[] getCourseVideo();
}

 ICourseManager接口：

public interface ICourseManager{
void studyCourse();
void refundCourse();
}


```



### 下面我们来看一下方法层面的单一职责设计。有时候，我们为了偷懒，通常会把一个方法写成下面

### 这样：

```java


private void modifyUserInfo(StringuserName,Stringaddress){
userName="Tom";
address="Changsha";
}

```

### 还可能写成这样：

```java

private void modifyUserInfo(StringuserName,String...fileds){
userName="Tom";
// address="Changsha";
}
private void modifyUserInfo(StringuserName,Stringaddress,booleanbool){
if(bool){

```
}else{
```
```
}
```
userName="Tom";
address="Changsha";
}

```


### 显然，上面的modifyUserInfo()方法中都承担了多个职责，既可以修改userName,也可以修改

### address，甚至更多，明显不符合单一职责。那么我们做如下修改，把这个方法拆成两个：

```java

private void modifyUserName(StringuserName){
userName="Tom";
}
private void modifyAddress(Stringaddress){
address="Changsha";
}

```



### 这修改之后，开发起来简单，维护起来也容易。但是，我们在实际开发中会项目依赖，组合，聚合

### 这些关系，还有还有项目的规模，周期，技术人员的水平，对进度的把控，很多类都不符合单一职责。

### 但是，我们在编写代码的过程，尽可能地让接口和方法保持单一职责，对我们项目后期的维护是有很大


### 帮助的。

### 接口隔离原则

### 接口隔离原则（InterfaceSegregationPrinciple,ISP）是指用多个专门的接口，而不使用单一的

### 总接口，客户端不应该依赖它不需要的接口。这个原则指导我们在设计接口时应当注意一下几点：

### 1 、一个类对一类的依赖应该建立在最小的接口之上。

### 2 、建立单一接口，不要建立庞大臃肿的接口。

### 3 、尽量细化接口，接口中的方法尽量少（不是越少越好，一定要适度）。

### 接口隔离原则符合我们常说的高内聚低耦合的设计思想，从而使得类具有很好的可读性、可扩展性

### 和可维护性。我们在设计接口的时候，要多花时间去思考，要考虑业务模型，包括以后有可能发生变更

### 的地方还要做一些预判。所以，对于抽象，对业务模型的理解是非常重要的。下面我们来看一段代码，

### 写一个动物行为的抽象：

### IAnimal接口：

```java


public interface IAnimal{
void eat();
void fly();
void swim();
}

### Bird类实现：

public class Bird implements IAnimal{
@Override
public void eat(){}
@Override
public void fly(){}
@Override
public void swim(){}
}

### Dog类实现：

public class Dog implements IAnimal{
@Override
public void eat(){}
@Override
public void fly(){}
@Override
public void swim(){}
}

```



### 可以看出，Bird的swim()方法可能只能空着，Dog的fly()方法显然不可能的。这时候，我们针对

### 不同动物行为来设计不同的接口，分别设计IEatAnimal，IFlyAnimal和ISwimAnimal接口，来看代码：

### IEatAnimal接口：

```java

public interface IEatAnimal{
void eat();
}

### IFlyAnimal接口：

public interface IFlyAnimal{
void fly();
}

### ISwimAnimal接口：

public interface ISwimAnimal{
void swim();
}

### Dog只实现IEatAnimal和ISwimAnimal接口：

public class Dog implements ISwimAnimal,IEatAnimal{
@Override
public void eat(){}
@Override
public void swim(){}
}
```

### 思考一些 什么是高内聚 低耦合




### 迪米特法则

### 迪米特原则（LawofDemeterLoD）是指一个对象应该对其他对象保持最少的了解，又叫最少知

### 道原则（LeastKnowledgePrinciple,LKP），尽量降低类与类之间的耦合。迪米特原则主要强调只和


### 朋友交流，不和陌生人说话。出现在成员变量、方法的输入、输出参数中的类都可以称之为成员朋友类，

### 而出现在方法体内部的类不属于朋友类。

### 现在来设计一个权限系统，TeamLeader需要查看目前发布到线上的课程数量。这时候，TeamLeader

### 要找到员工Employee去进行统计，Employee再把统计结果告诉TeamLeader。接下来我们还是来看

### 代码：

### Course类：

```java


public class Course{
}

### Employee类：

public class Employee{
public void checkNumberOfCourses(List<Course>courseList){
System.out.println("目前已发布的课程数量是："+courseList.size());
}
}

### TeamLeader类：

public class TeamLeader{
public void commandCheckNumber(Employeeemployee){

List<Course>courseList=newArrayList<Course>();
for(inti= 0 ;i< 20 ;i++){
courseList.add(newCourse());
}
employee.checkNumberOfCourses(courseList);
}
}

### 测试代码：

public static void main(String[]args){
TeamLeader teamLeader=new TeamLeader();
Employeeem ployee=new Employee();
teamLeader.commandCheckNumber(employee);
}
```



### 写到这里，其实功能已经都已经实现，代码看上去也没什么问题。根据迪米特原则，TeamLeader

### 只想要结果，不需要跟Course产生直接的交流。而Employee统计需要引用Course对象。TeamLeader

### 和Course并不是朋友


### 下面来对代码进行改造：

### Employee类：

```java

public class Employee{

public void checkNumberOfCourses(){
List<Course>courseList=newArrayList<Course>();
for(inti= 0 ;i< 20 ;i++){
courseList.add(newCourse());
}
System.out.println("目前已发布的课程数量是："+courseList.size());
}
}

### TeamLeader类：

public class TeamLeader{

public void commandCheckNumber(Employeeemployee){
employee.checkNumberOfCourses();
}
}
```



### 再来看下面的类图，Course和TeamLeader已经没有关联了。


### 里氏替换原则

### 里氏替换原则（LiskovSubstitutionPrinciple,LSP）是指如果对每一个类型为T 1 的对象o 1 ,都有

### 类型为T 2 的对象o 2 ,使得以T 1 定义的所有程序P在所有的对象o 1 都替换成o 2 时，程序P的行为没

### 有发生变化，那么类型T 2 是类型T 1 的子类型。

### 定义看上去还是比较抽象，我们重新理解一下，可以理解为一个软件实体如果适用一个父类的话，

### 那一定是适用于其子类，所有引用父类的地方必须能透明地使用其子类的对象，子类对象能够替换父类

### 对象，而程序逻辑不变。根据这个理解，我们总结一下：

### 引申含义：子类可以扩展父类的功能，但不能改变父类原有的功能。

### 1 、子类可以实现父类的抽象方法，但不能覆盖父类的非抽象方法。

### 2 、子类中可以增加自己特有的方法。

### 3 、当子类的方法重载父类的方法时，方法的前置条件（即方法的输入/入参）要比父类方法的输入

### 参数更宽松。

### 4 、当子类的方法实现父类的方法时（重写/重载或实现抽象方法），方法的后置条件（即方法的输

### 出/返回值）要比父类更严格或相等。

### 是可以看出是支持重载的


### 在前面讲开闭原则的时候埋下了一个伏笔，我们记得在获取折后时重写覆盖了父类的getPrice()方

### 法，增加了一个获取源码的方法getOriginPrice()，显然就违背了里氏替换原则。我们修改一下代码，

### 不应该覆盖getPrice()方法，增加getDiscountPrice()方法：


### 使用里氏替换原则有以下优点：


### 1 、约束继承泛滥，开闭原则的一种体现。

### 2 、加强程序的健壮性，同时变更时也可以做到非常好的兼容性，提高程序的维护性、扩展性。降低

### 需求变更时引入的风险。

### 先映入一个反里式替换原则的


```java

public class Test3 {
 
    public static void main(String[] args) {
        ClassA classA = new ClassA();
        System.out.println(classA.compare(10, 20)); // false
 
        // 按照里氏替换原则，子类对象替换出现父类对象的地方，观察程序运行行为有没有发生变化
        ClassB classB = new ClassB();
        System.out.println(classB.compare(10, 20)); // true
    }
 
}
 
class ClassA {
    public boolean compare(int a, int b) {
        return a > b;
    }
}
 
class ClassB extends ClassA {
    // 无意中重写了父类中已实现的方法， 将父类方法的行为都改变了。
    @Override
    public boolean compare(int a, int b) {
        return a < b;
    }
}

```

### 当出现父类对象的时候，我将它替换为子类对象，compare()方法的行为已经发生了变化，故违反了里氏替换原则。

### 优化

```java

public class Test4 {
 
    public static void main(String[] args) {
        ClassA classA = new ClassA();
        System.out.println(classA.compare(10, 20)); // false
 
        // 按照里氏替换原则，子类对象替换出现父类对象的地方，观察程序运行行为有没有发生变化
        ClassB classB = new ClassB();
        System.out.println(classB.compare(10, 20)); // false
    }
 
}
 
class ClassA {
    public boolean compare(int a, int b) {
        return a > b;
    }
}
 
class ClassB extends ClassA {
    // 通过新增方法来扩展新功能
    public boolean compare2(int a, int b) {
        return a < b;
    }
}


```




### 合成复用原则

### 合成复用原则（Composite/AggregateReusePrinciple,CARP）是指尽量使用对象组合(has-a)/

### 聚合(contanis-a)，而不是继承关系达到软件复用的目的。可以使系统更加灵活，降低类与类之间的耦

### 合度，一个类的变化对其他类造成的影响相对较少。

### 继承我们叫做白箱复用，相当于把所有的实现细节暴露给子类。组合/聚合也称之为黑箱复用，对类

### 以外的对象是无法获取到实现细节的。要根据具体的业务场景来做代码设计，其实也都需要遵循OOP

### 模型。还是以数据库操作为例，先来创建DBConnection类：

````java

public class DBConnection{
publicStringgetConnection(){
return"MySQL数据库连接";
}
}

### 创建ProductDao类：

public class ProductDao{
private DBConnectiondbConnection;
public void setDbConnection(DBConnectiondbConnection){
this.dbConnection=dbConnection;
}
public void addProduct(){
String conn=dbConnection.getConnection();
System.out.println("使用"+conn+"增加产品");
}
}

````



### 这就是一种非常典型的合成复用原则应用场景。但是，目前的设计来说，DBConnection还不是一

### 种抽象，不便于系统扩展。目前的系统支持MySQL数据库连接，假设业务发生变化，数据库操作层要

### 支持Oracle数据库。当然，我们可以在DBConnection中增加对Oracle数据库支持的方法。但是违

### 背了开闭原则。其实，我们可以不必修改Dao的代码，将DBConnection修改为abstract，来看代码：

```java

public abstract class DBConnection{
public abstract String getConnection();
}

### 然后，将MySQL的逻辑抽离：

public class MySQLConnection extends DBConnection{
@Override
public StringgetConnection(){
return"MySQL数据库连接";
}
}


### 再创建Oracle支持的逻辑：

public class OracleConnection extends DBConnection{
@Override
publicStringgetConnection(){
return"Oracle数据库连接";
}
}

```



### 具体选择交给应用层，来看一下类图：

### 设计原则总结

### 学习设计原则，学习设计模式的基础。在实际开发过程中，并不是一定要求所有代码都遵循设计原

### 则，我们要考虑人力、时间、成本、质量，不是刻意追求完美，要在适当的场景遵循设计原则，体现的

### 是一种平衡取舍，帮助我们设计出更加优雅的代码结构。

# 二 时间复杂度  空间复杂度
我们想要知道一个算法的「时间复杂度」，
很多人首先想到的的方法就是把这个算法程序运行一遍，那么它所消耗的时间就自然而然知道了。

因此，另一种更为通用的方法就出来了：「 大O符号表示法 」，即 T(n) = O(f(n))

我们先来看个例子：

for(i=1; i<=n; ++i)
{
   j = i;
   j++;
}    

通过「 大O符号表示法 」，这段代码的时间复杂度为：O(n) ，为什么呢?

在 大O符号表示法中，时间复杂度的公式是： T(n) = O( f(n) )，其中f(n) 表示每行代码执行次数之和，而 O 表示正比例关系，这个公式的全称是：算法的渐进时间复杂度。

我们继续看上面的例子，假设每行代码的执行时间都是一样的，我们用 1颗粒时间 来表示，那么这个例子的第一行耗时是1个颗粒时间，第三行的执行时间是 n个颗粒时间，第四行的执行时间也是 n个颗粒时间（第二行和第五行是符号，暂时忽略），那么总时间就是 1颗粒时间 + n颗粒时间 + n颗粒时间 ，即 (1+2n)个颗粒时间，即： T(n) = (1+2n)*颗粒时间，从这个结果可以看出，这个算法的耗时是随着n的变化而变化，因此，我们可以简化的将这个算法的时间复杂度表示为：T(n) = O(n)

为什么可以这么去简化呢，因为大O符号表示法并不是用于来真实代表算法的执行时间的，它是用来表示代码执行时间的增长变化趋势的。

所以上面的例子中，如果n无限大的时候，T(n) = time(1+2n)中的常量1就没有意义了，倍数2也意义不大。因此直接简化为T(n) = O(n) 就可以了。

常见的时间复杂度量级有：   

常数阶O(1)  
对数阶O(logN)  
线性阶O(n)  
线性对数阶O(nlogN)  
平方阶O(n²)  
立方阶O(n³)  
K次方阶O(n^k)  
指数阶(2^n)  
上面从上至下依次的时间复杂度越来越大，执行的效率越来越低。

下面选取一些较为常用的来讲解一下（没有严格按照顺序）：

常数阶O(1)
无论代码执行了多少行，只要是没有循环等复杂结构，那这个代码的时间复杂度就都是O(1)，如：

int i = 1;  
int j = 2;   
++i;  
j++;   
int m = i + j;  
上述代码在执行的时候，它消耗的时候并不随着某个变量的增长而增长，那么无论这类代码有多长，即使有几万几十万行，都可以用O(1)来表示它的时间复杂度。

线性阶O(n)
这个在最开始的代码示例中就讲解过了，如：

for(i=1; i<=n; ++i)
{
   j = i;
   j++;
}  
这段代码，for循环里面的代码会执行n遍，因此它消耗的时间是随着n的变化而变化的，因此这类代码都可以用O(n)来表示它的时间复杂度。

对数阶O(logN)
还是先来看代码：

int i = 1;  
while(i<n)  
{  
    i = i * 2;  
}  
从上面代码可以看到，在while循环里面，每次都将 i 乘以 2，乘完之后，i 距离 n 就越来越近了。我们试着求解一下，假设循环x次之后，i 就大于 2 了，此时这个循环就退出了，也就是说 2 的 x 次方等于 n，那么 x = log2^n
也就是说当循环 log2^n 次以后，这个代码就结束了。因此这个代码的时间复杂度为：O(logn)

线性对数阶O(nlogN)  
线性对数阶O(nlogN) 其实非常容易理解，将时间复杂度为O(logn)的代码循环N遍的话，那么它的时间复杂度就是 n * O(logN)，也就是了O(nlogN)。

就拿上面的代码加一点修改来举例：

for(m=1; m<n; m++)  
{   
    i = 1;  
    while(i<n)  
    {  
        i = i * 2;  
    }  
}  
平方阶O(n²)  
平方阶O(n²) 就更容易理解了，如果把 O(n) 的代码再嵌套循环一遍，它的时间复杂度就是 O(n²) 了。
举例：  

for(x=1; i<=n; x++)  
{   
   for(i=1; i<=n; i++)  
    {  
       j = i;  
       j++;  
    }  
}  
这段代码其实就是嵌套了2层n循环，它的时间复杂度就是 O(n*n)，即 O(n²)
如果将其中一层循环的n改成m，即：

for(x=1; i<=m; x++)  
{  
   for(i=1; i<=n; i++)  
    {  
       j = i;  
       j++;  
    }  
}  
那它的时间复杂度就变成了 O(m*n)

立方阶O(n³)、K次方阶O(n^k)  
参考上面的O(n²) 去理解就好了，O(n³)相当于三层n循环，其它的类似。  

除此之外，其实还有 平均时间复杂度、均摊时间复杂度、最坏时间复杂度、最好时间复杂度 的分析方法，有点复杂，这里就不展开了。  

二、空间复杂度
既然时间复杂度不是用来计算程序具体耗时的，那么我也应该明白，空间复杂度也不是用来计算程序实际占用的空间的。

空间复杂度是对一个算法在运行过程中临时占用存储空间大小的一个量度，同样反映的是一个趋势，我们用 S(n) 来定义。

空间复杂度比较常用的有：O(1)、O(n)、O(n²)，我们下面来看看：

空间复杂度 O(1)
如果算法执行所需要的临时空间不随着某个变量n的大小而变化，即此算法空间复杂度为一个常量，可表示为 O(1)
举例：

int i = 1;   
int j = 2;  
++i;  
j++;  
int m = i + j;  
代码中的 i、j、m 所分配的空间都不随着处理数据量变化，因此它的空间复杂度 S(n) = O(1)  

空间复杂度 O(n)  
我们先看一个代码：  

int[] m = new int[n]  
for(i=1; i<=n; ++i)  
{  
   j = i;  
   j++;  
}  
这段代码中，第一行new了一个数组出来，这个数据占用的大小为n，这段代码的2-6行，虽然有循环，但没有再分配新的空间，因此，这段代码的空间复杂度主要看第一行即可，即 S(n) = O(n)




