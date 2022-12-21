## 创建型模式

关注点是如何创建对象，其核心思想是要**把对象的创建和使用相分离**，这样使得两者能相对独立地变换

### Singleton

单例

提供访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

**abstract**

```java
object Singleton {
    ...
}
```

**implementation**

```kotlin
object Singleton {
    fun service(): Unit {
        println("I am singleton")
        println("I offer some general service")
    }
}

fun demo() {
    Singleton.service()
}
```

### Factory

工厂

不对外暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。

#### Simple Factory

一个Factory返回多个Product子类，新增Product子类时需修改Factory类

**abstract**

```kotlin
interface Product

interface ProductType

interface Factory {
    fun getProduct(productType: ProductType): Product
}
```

**implementation**

```kotlin
enum class OrganismType: ProductType {
    AQUATIC,
    TERRESTRIAL
}

class Aquatic: Organism

class Terrestrial: Organism

//水生生物与陆生生物
object OrganismFactory: Factory {
    override fun getProduct(productType: ProductType): Organism {
        println(productType as OrganismType)
        return when (productType as OrganismType) {
            OrganismType.AQUATIC -> Aquatic()
            OrganismType.TERRESTRIAL -> Terrestrial()
        }
    }
}

fun simpleFactoryDemo() {
    val organism = OrganismFactory.getProduct(OrganismType.TERRESTRIAL)
}
```

#### Factory Method

每个Product子类均有各自对应的Factory

**abstract**

```kotlin
interface Product

interface ProductType

interface Factory {
    fun getProduct(productType: ProductType): Product
}
```

**implementation**

```kotlin
interface Organism: Product

interface OrganismFactories: Factory {
    override fun getProduct(productType: ProductType): Organism
}

class Aquatic: Organism

class Terrestrial: Organism

enum class OrganismType: ProductType {
    AQUATIC,
    TERRESTRIAL
}

object AquaticFactory: OrganismFactories {
    override fun getProduct(productType: ProductType): Organism {
        if (productType as OrganismType != OrganismType.AQUATIC){
            println("wrong type")
        }
        println("aquatic")
        return Aquatic()
    }
}

object TerrestrialFactory: OrganismFactories {
    override fun getProduct(productType: ProductType): Organism {
        if (productType as OrganismType != OrganismType.TERRESTRIAL){
            println("wrong type")
        }
        println("terrestrial")
        return Terrestrial()
    }
}

enum class OrganismType: ProductType {
    AQUATIC,
    TERRESTRIAL
}

fun factoryMethodDemo() {
    val aquatic = AquaticFactory.getProduct(OrganismType.AQUATIC)
    val terrestrial = AquaticFactory.getProduct(OrganismType.TERRESTRIAL)
}
```

#### Abstract Factory

抽象工厂

工厂方法的进一步深化，围绕一个超级工厂创建其他工厂。该超级工厂又称为其他工厂的工厂

当存在多个Product族时使用，类似于简单工厂与工厂方法的结合，从Product族层面看为工厂方法，从某一Product族中的子Product层面看为简单工厂

**abstract**

```kotlin
interface Product

interface ProductType

interface ProductFamilyType

interface AbstractFactory {
    fun getProduct(productFamilyType: ProductFamilyType, productType: ProductType): Product
}
```

**implementation**

```kotlin
interface Substance: Product

interface Organic: Substance

interface Inorganic: Substance

interface SubstanceType: ProductType

enum class SubstanceFamilyType: ProductFamilyType {
    ORGANIC,
    INORGANIC
}

enum class OrganicType: SubstanceType {
    FAT,
    PROTEIN,
    CARBOHYDRATE
}

enum class InorganicType: SubstanceType {
    WATER,
    INORGANICSALT
}

class Water: Inorganic

class InorganicSalt: Inorganic

class Fat: Organic

class Protein: Organic

class Carbohydrate: Organic

//有机物与无机物
object InorganicFactory: AbstractFactory {
    override fun getProduct(productFamilyType: ProductFamilyType, productType: ProductType): Inorganic {
        if(productFamilyType as SubstanceFamilyType!=SubstanceFamilyType.INORGANIC) throw java.lang.Exception()
        val substance = when (productType as InorganicType) {
            InorganicType.WATER -> Water()
            InorganicType.INORGANICSALT -> InorganicSalt()
        }
        println("$productType of $productFamilyType family is produced")
        return substance
    }
}

object OrganicFactory: AbstractFactory {
    override fun getProduct(productFamilyType: ProductFamilyType, productType: ProductType): Organic {
        if(productFamilyType as SubstanceFamilyType!=SubstanceFamilyType.ORGANIC) throw java.lang.Exception()
        val substance = when (productType as OrganicType) {
            OrganicType.FAT -> Fat()
            OrganicType.CARBOHYDRATE -> Carbohydrate()
            OrganicType.PROTEIN -> Protein()
        }
        println("$productType of $productFamilyType family is produced")
        return substance
    }
}

fun abstractFactoryDemo() {
    try {
        OrganicFactory.getProduct(SubstanceFamilyType.ORGANIC,InorganicType.INORGANICSALT)
    } catch (e: Exception) {
        println("wrong productFamilyType or ProductType")
    }
}
```

### 应该不考

#### Builder

建造者

建造者模式（Builder Pattern）使用多个简单的对象一步一步构建成一个复杂的对象，将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。

适用于：

1、需要生成的对象具有复杂的内部结构。 

2、需要生成的对象内部属性本身相互依赖。

> vs. Factory Pattern ：建造者模式更加关注与零件装配的顺序。
>
> vs. Bridge Pattern

**abstract**

```kotlin
interface Product

interface Builder {
    fun build(): Product
}
```

**implementation**

```kotlin
data class Intelligence(val wisdom: Int = 0) : Product

data class Courage(val courageous: Int = 0) : Product

data class Spirit(val intelligence: Intelligence = Intelligence(), val courage: Courage = Courage()) : Product

data class Skelecton(val solidity: Int = 0) : Product

data class Muscle(val strength: Int = 0) : Product

data class Flesh(val skelecton: Skelecton = Skelecton(), val muscle: Muscle = Muscle()) : Product

data class Life(val spirit: Spirit, val flesh: Flesh) : Product

class LifeBuilder: Builder {
    private var spirit: Spirit = Spirit()
    private var flesh: Flesh = Flesh()

    fun soul(wisdom: Int, courageous: Int): LifeBuilder {
        spirit = Spirit(Intelligence(wisdom), Courage(courageous))
        println("intelligence $wisdom")
        println("courageous $courageous")
        println("a beautiful soul was infused")
        return this
    }

    fun body(solidity: Int, strength: Int): LifeBuilder {
        flesh = Flesh(Skelecton(solidity), Muscle(strength))
        println("solidity $solidity")
        println("strength $strength")
        println("a strong body is formed")
        return this
    }

    override fun build(): Life {
        println("spirit is infused with flesh")
        return Life(spirit,flesh)
    }
}
```

#### Prototype

原型

用于创建重复的对象，利用已有的一个原型对象，通过**复制**快速地生成和原型对象一样的实例，同时又能保证性能，当直接创建对象的代价比较大时，则可采用这种模式。

**abstract**

```kotlin
interface Content {
    fun copy(): Content
}

interface Prototype {
    val content: Content
    fun clone(): Content
}
```

**implementation**

```kotlin
//仿生人
class Android: Content {
    override fun copy(): Content {
        println("new android had been manufactured")
        return Android()
    }
}

class AndroidPrototype(override val content: Android) : Prototype {
    override fun clone(): Content {
        return content.copy()
    }
}

fun prototypeDemo() {
    val androidPrototype = AndroidPrototype(Android())
    val nAndroid: Android = androidPrototype.clone() as Android
}
```

## 行为型模式

主要涉及算法和对象间的职责分配，描述一组对象应该如何通过使用对象组合协作来完成一个整体任务。

### Observer vs. MVC（时序图）vs. Reactor（时序图）

#### Observer

观察者

当一个对象被修改时，自动通知依赖它的对象

> vs. Reactor

**abstract**

```kotlin
interface Observer {
    fun update(observableState: ObservableState)
}

interface ObservableState

interface Observable {
    var state: ObservableState
    val observers: ArrayList<Observer>
    fun attachObserver(observer: Observer) {
        observers.add(observer)
    }
    fun updateState(state: ObservableState) {
        this.state = state
    }
    
    fun notifyObservers() {
        observers.forEach { it -> it.update(state) }
    }
}
```

**implementation**

```kotlin
data class LoginState(val on: Boolean = false) : ObservableState

class User(override var state: ObservableState = LoginState(), override val observers: ArrayList<Observer> = ArrayList()) : Observable

class Portrait: Observer {
    override fun update(observableState: ObservableState) {
        when( (observableState as LoginState).on ) {
            true -> println("portrait became light")
            false -> println("portrait become dark")
        }
    }
}

class Friends: Observer {
    override fun update(observableState: ObservableState) {
        when( (observableState as LoginState).on ) {
            true -> println("user is online")
            false -> println("user is offline")
        }
    }
}

fun observerDemo() {
    val user = User()
    user.attachObserver(Friends())
    user.attachObserver(Portrait())
    user.notifyObservers()
    user.updateState(LoginState(true))
}
```

#### Reactor

基于**事件驱动**的设计模式，拥有一个或多个并发输入源，有一个服务处理器和多个请求处理器，服务处理器会同步的将输入的请求事件以多路复用的方式分发给相应的请求处理器

**abstract**

```kotlin
interface Event

interface Reactor {
    val handlers: ArrayList<EventHandler>
    fun dispatch(event: Event)
}

interface EventHandler {
    fun handle(event: Event)
}
```

**implementation**

```kotlin
class ClickedEvent: Event

class EnteredEvent: Event

class ClickedEventHandler: EventHandler {
    override fun handle(event: Event) {
        println("clicked event is handled")
    }
}

class EnteredEventHandler: EventHandler {
    override fun handle(event: Event) {
        println("entered event is handled")
    }
}

class EventDispatcher(override val handlers: ArrayList<EventHandler> = ArrayList()) : Reactor {
    override fun dispatch(event: Event) {
        when (event) {
            is ClickedEvent -> ClickedEventHandler().handle(event)
            is EnteredEvent -> EnteredEventHandler().handle(event)
        }
    }
}

fun reactorDemo() {
    val eventDispatcher = EventDispatcher()
    eventDispatcher.dispatch(ClickedEvent())
    eventDispatcher.dispatch(EnteredEvent())
}
```

#### MVC

- **模型（Model）** 用于封装与应用程序的业务逻辑相关的数据以及对数据的处理方法。“ Model ”有对数据直接访问的权力，例如对数据库的访问。“Model”不依赖“View”和“Controller”，也就是说， Model 不关心它会被如何显示或是如何被操作。但是 Model 中数据的变化一般会通过一种刷新机制被公布。为了实现这种机制， View 或事先在此 Model 上注册，从而，View 可以了解在数据 Model 上发生的改变（[观察者模式](https://zh.m.wikipedia.org/wiki/观察者模式)）或在Controller操控下更新View
- **视图（View）**能够实现数据有目的的显示（理论上，这不是必需的）。在 View 中一般没有程序上的逻辑。为了实现 View 上的刷新功能，View 需要访问它监视的数据模型（Model），因此应该事先在被它监视的数据那里注册。
- **控制器（Controller）**起到不同层面间的组织作用，用于控制应用程序的流程。它处理事件并作出响应。“事件”包括用户的行为和数据 Model 上的改变。

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/MVC-Process.svg/200px-MVC-Process.svg.png)

**abstract**

```kotlin
interface Model {
    fun update()
    fun saveToDatabase()
    fun notifyView(view: View)
}

interface Controller {
    val model: Model
    val view: View
    fun updateModel()
}

interface View {
    fun updateGUI()
}
```

**implementation**

```kotlin
class ViewImpl: View {
    override fun updateGUI() {
        println("gui is updated")
    }
}

class ControllerImpl(override val model: Model = ModelImpl(), override val view: View = ViewImpl()) : Controller {
    override fun updateModel() {
        println("controller react to user behavior")
        model.update()
        model.notifyView(view)
    }
}

class ModelImpl : Model {
    override fun update() {
        println("model is updated")
        saveToDatabase()
    }

    override fun saveToDatabase() {
        println("change to model is saved to database")
    }

    override fun notifyView(view: View) {
        println("notify view")
        view.updateGUI()
    }
}

fun MVCDemo() {
    val controller = ControllerImpl()
    controller.updateModel()
}
```

### Interpreter

解释器

给定一个语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子

常被用在 SQL 解析、符号处理引擎

**abstract**

```kotlin
interface Expression {
    fun interpret(context: String): Boolean
}
```

**implementation**

```kotlin
class AndExpression(val exp1: Expression, val exp2: Expression): Expression {
    override fun interpret(context: String): Boolean {
        return exp1.interpret(context)&&exp2.interpret(context)
    }
}

class OrExpression(val exp1: Expression, val exp2: Expression): Expression {
    override fun interpret(context: String): Boolean {
        return exp1.interpret(context)||exp2.interpret(context)
    }
}

class NotExpression(val exp: Expression): Expression {
    override fun interpret(context: String): Boolean {
        return !exp.interpret(context)
    }
}

class MetaExpression(val data: String): Expression {
    override fun interpret(context: String): Boolean {
        return context.contains(data)
    }
}

fun interpretDemo() {
    val rick = MetaExpression("rick")
    val morty = MetaExpression("morty")
    println(NotExpression(rick).interpret("rick"))
    println(AndExpression(rick,morty).interpret("rick and morty"))
    println(AndExpression(rick,NotExpression(morty)).interpret("rick and morty"))
    // false,true,false
}
```

### Strategy

策略

创建定义一系列的算法的各种策略对象和一个所执行算法随着策略对象改变而改变的 Context 对象，使得算法可以**自由切换**，且可**独立于它的使用者变化**

> vs. State Pattern

**abstract**

```kotlin
interface Strategy {
    fun action()
}

interface StrategyChoice

interface StrategyConsumer {
    var strategy: Strategy
    fun updateStrategy(strategyChoice: StrategyChoice)
    fun action()
}
```

**implementation**

```kotlin
enum class Enemy: StrategyChoice{
    ATTACK,
    MOVE,
    DEFENSE
}
//三种策略
class DefenseStrategy: Strategy {
    override fun action() {
        println("enemy is attacking, take the defense strategy")
    }
}

class MoveStrategy: Strategy {
    override fun action() {
        println("enemy is defensing, take the move strategy")
    }
}

class AttackStrategy: Strategy {
    override fun action() {
        println("enemy is moving, take the attack strategy")
    }
}
//战士根据敌人的动作切换策略
class Fighter(override var strategy: Strategy = DefenseStrategy()) : StrategyContext {
    override fun action(strategyChoice: StrategyChoice) {
        strategy = when (strategyChoice as Enemy) {
            Enemy.ATTACK -> DefenseStrategy()
            Enemy.DEFENSE -> MoveStrategy()
            Enemy.MOVE -> AttackStrategy()
        }
        strategy.action()
        println()
    }
}

fun strategyDemo() {
    val fighter = Fighter()
    fighter.action(Enemy.DEFENSE)
    fighter.action(Enemy.MOVE)
    fighter.action(Enemy.ATTACK)
}
```

### Visitor

访问者

为了访问比较复杂的数据结构，不去改变数据结构，而是把对数据的操作抽象出来，在“访问”的过程中以回调形式**在访问者中处理操作逻辑**。如果要新增一组操作，那么只需要增加一个新的访问者。

**abstract**

```java
interface Element {
    fun accept(visitor: Visitor): Unit {
        println("universal element output")
        visitor.visit(this)
        println("")
    }
}

interface Visitor {
    fun visit(element: Element): Unit {
        println("universal visitor output")
    }
}
```

**implementation**

```kotlin
class Meat: Element

class Fruits: Element

class Vegetable: Element

class Vegetarian: Visitor {
    override fun visit(element: Element) {
        super.visit(element)
        println("I am a vegetarian")
        when(element){
            is Meat -> println("I hate meat")
            is Fruits -> println("I enjoy fruits")
            is Vegetable -> println("I love vegetable")
        }
    }
}

class Carnivore: Visitor {
    override fun visit(element: Element) {
        super.visit(element)
        println("I am a carnivore")
        when(element){
            is Meat -> println("I love meat")
            is Fruits -> println("Fruits is Ok")
            is Vegetable -> println("I hate vegetable")
        }
    }
}

fun demo() {
    val meat: Meat = Meat()
    val fruits: Fruits = Fruits()
    val vegetable: Vegetable = Vegetable()
    val vegetarian: Vegetarian = Vegetarian()
    val carnivore: Carnivore = Carnivore()
    meat.accept(carnivore)
    meat.accept(vegetarian)
    fruits.accept(carnivore)
    fruits.accept(vegetarian)
    vegetable.accept(carnivore)
    vegetable.accept(vegetarian)
}
```

### 应该不考

#### Command

命令模式

数据驱动

以**命令**（Command）的形式包裹请求，并传给**调用对象**（Invoker）。调用对象寻找可以处理该命令的**合适的对象**（Receiver）来执行命令。

**abstract**

```kotlin
interface Command {
    fun execute(receiver: Receiver) {
        receiver.exec()
    }
}

interface Receiver {
    fun exec()
}

interface Invoker {
    val commands: ArrayList<Command>
    fun addCommand(command: Command) {
        commands.add(command)
    }
    fun executeCommands()
}
```

**implementation**

```kotlin
class Gardening: Command

class Cleaning: Command

class Cleaner: Receiver {
    override fun exec() {
        println("The house had been cleaned.")
    }
}

class Gardener: Receiver {
    override fun exec() {
        println("The garden had been taken care of.")
    }

}

class Butler(override val commands: ArrayList<Command>) : Invoker {
    private val gardener = Gardener()
    private val cleaner = Cleaner()
    override fun executeCommands() {
        commands.forEach{ command ->
            when(command) {
                is Gardening -> command.execute(gardener)
                is Cleaning -> command.execute(cleaner)
            }
            println()
        }
        commands.clear()
    }
}

fun commandDemo() {
    val butler = Butler(ArrayList())
    butler.addCommand(Gardening())
    butler.addCommand(Cleaning())
    butler.executeCommands()
}
```

#### State

将各状态对应逻辑分拆到不同的状态类中，以达到**易于拓展新状态**的目的。

存在各种状态State的和一个管理状态切换的 Context 

**abstract**

```kotlin
interface State {
    fun action()
}

interface  Context {
    var currentState: State
    fun action() {
        currentState.action()
    }
}
```

**implementation**

```kotlin
class QianJiSan(override var currentState: State) : Context

class ShieldForm: State {
    override fun action() {
        println("ShieldForm")
        println("defense")
    }
}

class SwordForm: State {
    override fun action() {
        println("SwordForm")
        println("fight")
    }
}

fun stateDemo() {
    val swordForm = SwordForm()
    val shieldForm = ShieldForm()
    val qianJiSan = QianJiSan(swordForm)
    qianJiSan.action()
    qianJiSan.currentState = shieldForm
    qianJiSan.action()
}
```

#### Chain-of-responsibility

责任链

> 使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止。

责任链模式（Chain of Responsibility）是一种处理请求的模式，它让多个处理器都有机会处理该请求，直到其中某个处理成功为止。责任链模式把多个处理器串成链，然后让请求在链上传递

责任链模式（Chain of Responsibility Pattern）为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。这种类型的设计模式属于行为型模式。

在这种模式中，通常每个接收者都包含对另一个接收者的引用。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。

**abstract**

```kotlin
interface Handler {
    val nextHandler: Handler?
    val authority: Priority
    fun handle(priority: Priority)
}

interface Priority
```

**implementation**

```kotlin
enum class Vampire: Priority {
    Baron,
    Vicomte,
    Comte,
    Marquess,
    Duke,
    Infante
}

interface Priest: Handler {
    val name: String
    override val authority: Vampire
    override val nextHandler: Priest?
    override fun handle(priority: Priority) {
        if ((priority as Vampire)<=authority) {
            println("$priority wad purified")
        println()
        } else {
            println("request ${nextHandler?.name?:"god"} to process")
            nextHandler?.handle(priority)
        }
    }
}

//三种神职人员
class Monk(override val nextHandler: Priest, override val name: String = "monk", override val authority: Vampire = Vampire.Vicomte) : Priest {}

class Bishop(override val nextHandler: Priest, override val name: String = "bishop", override val authority: Vampire = Vampire.Marquess) : Priest {}

class Pope(override val name: String = "pope", override val authority: Vampire = Vampire.Duke, override val nextHandler: Priest? = null) : Priest {}


fun chainOfResponsibilityDemo(){
    val pope = Pope()
    val bishop = Bishop(pope)
    val monk = Monk(bishop)
    monk.handle(Vampire.Duke)
    monk.handle(Vampire.Infante)
}
```

#### Iterator

迭代器

提供一种无需对外暴露集合对象的内部表示就能就遍历集合对象元素的方法

分离了集合对象的遍历行为，把遍历元素责任交给迭代器，而不是集合对象

**abstract**

```kotlin
interface Iterator {
    fun hasNext(): Boolean
    fun next(): Any
}

interface Container {
    fun iterator(): Iterator
}
```

**implementation**

```kotlin
class Virtue: Container {
    val virtues: ArrayList<String> = arrayListOf("Chastity","Diligence","Charity","Humility","Patience","Temperance")
    inner class VirTueIterator: Iterator {
        var cur: Int = 0
        override fun hasNext(): Boolean {
            return cur<virtues.size
        }

        override fun next(): String {
            return virtues[cur++]
        }
    }
    override fun iterator(): Iterator {
        return VirTueIterator()
    }
}
```

#### Mediator

中介

用一个中介对象来处理不同类/对象之间的通信，把多方会谈变成双方会谈，以降低多个类之间的通信复杂性。

中介者使各个类不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

**abstract**

```kotlin
interface Mediator {
    val communicators: ArrayList<Communicator>
    fun deliver(message: Message)
}

interface Message

interface Communicator {
    val mediator: Mediator
    fun sendMessage(message: Message, receiver: Communicator)
    fun receiveMessage(message: Message, sender: Communicator) = {}
}
```

**implementation**

```kotlin
data class ChatMessage(val content: String) : Message

class ChatRoom(override val communicators: ArrayList<Communicator> = ArrayList()) : Mediator {
    override fun deliver(message: Message) {
        println((message as ChatMessage).content)
        println()
    }
}

class ChatUser(val name: String, override val mediator: ChatRoom) : Communicator {
    override fun sendMessage(message: Message, receiver: Communicator) {
        println("$name send a message to ${(receiver as ChatUser).name}")
        mediator.deliver(message)
    }
}
```

#### Memento

备忘录

在不破坏封装性的前提下，在某对象之外保存该对象的内部状态，以便在适当的时候恢复对象

**abstract**

```kotlin
interface Originator {
    var states: States
    fun save(): Memonto
    fun load(memonto: Memonto)
}

interface States {}

interface Memonto {
    val states: States
    fun load(): States {return states}
}
```

**implementation**

```kotlin
data class GameStates(val level: Int, val stage: Int): States

class GameArchive(override val states: States) : Memonto

class Game(override var states: States = GameStates(1,1)) : Originator {
    override fun save(): Memonto {
        println("game archive is saved")
        return GameArchive(states)
    }

    override fun load(memonto: Memonto) {
        states = memonto.states
        println("game archive is loaded")
    }
}

fun memontoDemo() {
    val game = Game()
    val archi = game.save()
    game.load(archi)
}
```

#### Template-method

模板方法

一个抽象类/接口公开定义了执行它的方法的模板“骨架”。它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。

**abstract**

```kotlin
abstract class Template {
    protected abstract fun first()
    protected abstract fun second()
    protected abstract fun last()
    final fun steps() {
        first()
        second()
        last()
    }
}
```

**implementation**

```kotlin
class Human: Template() {
    override fun first() {
        println("birth")
    }

    override fun second() {
        println("aging")
    }

    override fun last() {
        println("death")
    }
}

fun templateDemo() {
    val human = Human()
    human.steps()
}
```

## 结构型模式

不仅仅简单地使用继承，而更多地通过组合与运行期的动态组合来实现更好、更灵活的结构与功能

### Adapter

适配器

作为两个不兼容的接口之间的桥梁，将一个类的接口转换成客户希望的另外一个接口以结合了两个独立接口的功能，使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

**abstract**

```kotlin
interface Destination {
    fun func1()
}

interface Source {
    fun func2()
}

interface Adapter: Destination {
    val source: Source
    fun fun1()
}
```

**implementation**

```kotlin
class Wizard: Source {
    override fun func2() {
        println("Attack Power")
    }
}

open class Warrior: Destination {
    override fun func1() {
        println("Attack Damage")
    }
}

class WizardWarrior(override val source: Source) : Warrior(),Adapter {
    override fun func1() {
        super.func1()
        source.func2()
    }
}
```

### Proxy

代理

创建具有某对象的代理对象，以便**代表**改对象向外界提供功能接口。

**abstract**

```java
public interface Original {
   void action();
}

interface Proxy: Original{
    val original: Original
    override fun action(): Unit {
        original.action()
    }
}
```

**implementation**

```kotlin
class HttpService: Original {
    override fun action() {
        println("HTTP Service offered")
    }
}

class EvilHttpService(override val original: HttpService) : Proxy {
    private fun check() :Boolean {
        return (1..2).random()<2
    }
    override fun action() {
        println("HTTP request is censored")
        val youAreAGoodPerson = check()
        if (youAreAGoodPerson) {
            original.action()
        }else {
            println("your access is denied!")
        }
        println()
    }
}

fun demo() {
    val httpService = EvilHttpService(HttpService())
	httpService.action()
}
```

### Decorator

装饰器

作为现有的类的一个**包装**，向一个现有的对象添加新的功能，同时又不改变其结构，装饰和数据源类**实现同一接口**， 从而能在客户端代码中**相互替换**。

**abstract**

```java
interface Original {
    fun action(): Unit
}

interface Decorator: Original{
    val original: Original
    fun before(): Unit {}
    fun after(): Unit {}
    override fun action(): Unit {
        before()
        original.action()
        after()
        println()
    }
}
```

**implementation**

```kotlin
class Mage: Original {
    override fun action() {
        println("I am a mage")
    }
}

class FireMage(override val original: Original): Decorator{
    override fun after() {
        println("A FireMage to be exact")
        println("Incendio")
    }
}

class IceMage(override val original: Original): Decorator{
    override fun after() {
        println("A IceMage to be exact")
        println("freezing charm")
    }
}

fun demo() {
    val fireMage = FireMage(Mage())
    val iceMage = IceMage(Mage())
    fireMage.action()
    iceMage.action()
}
```

> **代理**模式，注重对对象某一功能的流程把控和辅助。 它可以控制对象做某些事，重心是为了借用对象的功能完成某一流程，而非对象功能如何。 
>
> **装饰**模式，注重对对象功能的扩展，它不关心外界如何调用，只注重对对象功能的加强，**装饰**后还是对象本身。

### 应该不考

#### Bridge

桥接

又称为柄体(Handle and Body)模式或接口(Interfce)模式

将抽象接口作为抽象化和实现化之间的桥接结构，把**抽象化与实现化解耦**，使得实体类的功能独立于接口实现类，使得二者可以**独立变化**。避免有多种可能会变化的情况下直接继承带来的子类爆炸问题，提高扩展的灵活性。

**abstract**

```kotlin
interface Bridge {
    fun func()
}

interface Entity {
    val bridge: Bridge
    fun func()
}
```

**implementation**

```kotlin
abstract class Mecha(override val bridge: EnergyCore) : Entity {
    internal abstract fun statement()
    override fun func() {
        statement()
        bridge.func()
        println()
    }
}

//不同机甲型号可搭配不同能源核心，使得二者均易于拓展
class MechaI(core: EnergyCore): Mecha(bridge = core) {
    override fun statement() {
        println("MechaI")
    }
}

class MechaV(core: EnergyCore): Mecha(bridge = core) {
    override fun statement() {
        println("MechaV")
    }
}

class MechaX(core: EnergyCore): Mecha(bridge = core) {
    override fun statement() {
        println("MechaX")
    }
}

interface EnergyCore: Bridge

class SolarEnergyCore: EnergyCore {
    override fun func() {
        println("solar energy is transforming into power")
    }
}

class WindEnergyCore: EnergyCore {
    override fun func() {
        println("wind energy is transforming into power")
    }
}

fun bridgeDemo() {
    MechaI(WindEnergyCore()).func()
    MechaI(SolarEnergyCore()).func()
    MechaV(WindEnergyCore()).func()
    MechaV(SolarEnergyCore()).func()
    MechaX(WindEnergyCore()).func()
    MechaX(SolarEnergyCore()).func()
}
```

#### Composite

组合

又称为**部分整体模式**，依据树形结构来组合对象，创建了一个包含自己对象组的类，把**一组**相似的对象当作一个单一的对象处理。使得用户对单个对象和组合对象的使用具有**一致性**。

**abstract**

```kotlin
interface Composite {
    val composites: ArrayList<Composite>
}
```

**implementation**

```kotlin
interface HTMLElement: Composite {
    val content: String
    fun toHTML() {
        composites.forEach { it ->
            (it as HTMLElement).toHTML()
        }
    }
}

class HTML(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<html>")
        super.toHTML()
        println("</html>")
    }
}

class Body(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<body>")
        super.toHTML()
        println("</body>")
    }
}

class H1(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<h1> $content </h1>")
    }
}

class P(override val content: String = "",override val composites: ArrayList<Composite> = ArrayList()) : HTMLElement {
    override fun toHTML() {
        println("<p> $content </p>")
    }
}

fun compositeDemo() {
    val html = HTML()
    val body = Body()
    body.composites.add(H1("header"))
    body.composites.add(P("a paragraph"))
    body.composites.add(P("another paragraph"))
    html.composites.add(body)
    html.toHTML()
}
```

#### Facade

外观

对外隐藏具有诸多子系统的系统的复杂性，只提供一个可以访问系统的接口

**abstract**

```kotlin
interface Facade {
    val subsystems: ArrayList<Subsystem>
    fun react()
}

interface Subsystem{
    fun react()
}
```

**implementation**

```kotlin
class ForeignAffairsMinistry: Subsystem{
    override fun react() {
        println("The Ministry of Foreign Affairs intervened")
    }
}

class NationalDefenseMinistry: Subsystem{
    override fun react() {
        println("The Ministry of Defense conducts military mobilization")
    }
}

class Government(override val subsystems: ArrayList<Subsystem> = ArrayList()) : Facade {
    private val foreignAffairsMinistry: ForeignAffairsMinistry = ForeignAffairsMinistry()
    private val nationalDefenseMinistry: NationalDefenseMinistry = NationalDefenseMinistry()

    override fun react() {
        if((1..2).random()<2){
            foreignAffairsMinistry.react()
        }else nationalDefenseMinistry.react()
    }
}
```

#### Flyweight

通过工厂方法尝试重用现有的同类对象以减少创建对象的数量，以减少内存占用和提高性能，如果未找到匹配的对象，则创建新对象

在实际应用中，享元模式主要应用于**缓存**

> vs. Singleton
>
> 单例模式是类级别的，一个类**只能有一个**对象实例；
>
> 享元模式是对象级别的，**可以有多个**对象实例，并允许多个变量引用同一个对象实例；

**abstract**

```kotlin
interface FlyWeight

interface FlyWeightEnum

interface FlyWeightFactory {
    val map: Map<FlyWeightEnum,FlyWeight>
    fun getFlyWeight(flyWeightEnum: FlyWeightEnum): FlyWeight
}
```

**implementation**

```kotlin
enum class FaithOfTheSeven: FlyWeightEnum {
    Father,
    Mother,
    Warrior,
    Smith,
    Maiden,
    Crone,
    Stranger
}

class Deity(private val priesthood: FaithOfTheSeven = FaithOfTheSeven.Father): FlyWeight {
    fun answerPrayer() {
        println("$priesthood answered the prayer")
        println()
    }
}

class Church(override val map: HashMap<FlyWeightEnum, Deity> = HashMap()) : FlyWeightFactory {
    override fun getFlyWeight(flyWeightEnum: FlyWeightEnum): Deity {
        if (!map.containsKey(flyWeightEnum)) {
            map[flyWeightEnum] = Deity(flyWeightEnum as FaithOfTheSeven)
            println("$flyWeightEnum was awakened")
        }
        return map.getOrDefault(flyWeightEnum, Deity())
    }

    fun pray(flyWeightEnum: FlyWeightEnum) {
        println("The believers prayed")
        getFlyWeight(flyWeightEnum).answerPrayer()
    }
}
```

# 