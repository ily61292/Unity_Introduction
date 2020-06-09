#遵循原则
###单一原则
###开闭原则
get set
###依赖倒置原则
1、先了解需求  2、画图  3、写代码
###接口隔离原则
接口里的单一原则
###里氏原则
###合成复用原则
###迪米特法则（最少知识原则）


********************************


#设计模式
###1.单例模式

	1.有且只有一个实例。（组织框架的管理者）
		

###2.工厂模式
作用：实例化一些对象

	给工厂一个任务  工厂产生产品
	1.不关心生产过程


###3.观察者模式
Update 计时器

	1.不断询问

###4.代理模式
delegate   委托

代理：指向方法的指针

###5.策略者模式

根据不同的输入能够得到不同的结果（多态 ）


###6.建造者模式

一层一层建构


###7.中介者模式

两个类之间没有相互引用   通过第三方类（中介者）引用前两个类


###8.门面模式

将各种不同类型的东西聚合 形成特定的功能（用在UI上较多）

###9.组合模式

将相同类型的东西聚合在一起 形成特有的功能


###10.状态者模式

FSM：有限状态机

###WWW
Get请求： 参数在连接里
Post请求： 参数需放在表单里  WWWForm


###零碎知识点

UnityEngine.Envent;  IDraHadler


取模等于取余

按钮点击事件  
btn.onClick.AddListener(new UnityAction(OnClick));

1.Application.dataPath   Assets目录   一般只用在PC上

2.Application.persistentDataPath    缓存地址  用文件系统  可读可写
	
	1、Android：SD卡
	2、IOS:Document
	3、PC： C:\Users\Default\AppData

3.Application.steamingAssetsPath    SteamingAssets目录   该目录下的文件会被打包 


## 五个坐标系
	1、世界坐标系：	以（0,0,0）坐标点为参考的记录的相对位置
	2、物体坐标系：	以父类坐标点为参考的记录的相对位置
	3、相机坐标系：	以相机坐标点为参考的记录的相对位置
	4、视图坐标系：	左下角是（0,0）	右上角是（1,1）
	5、屏幕坐标系：	左下角是（0,0）	右上角是（屏幕的宽，屏幕的高）


## 动画有三种：
	1、关节动画    多个物体组成的一个整体（物体之间有间隙）
		MeshFilter  要显示的模型顶点数据		MeshRender  将MeshFilter筛选出来的顶点数据传递给GPU
	2、蒙皮动画    由顶点将骨骼包围成一个无缝的整体
		SkinMeshRender  显示某个模型  将模型文件传递给GPU
	3、顶点动画    对模型的顶点做动画

### 根据Unity发展历史：
	Legacy：Unity4.0以前的版本   只有关节动画
	Generic：非人型动画	动画不通用
	Humanoid：人型动画   对于人型来说  动画都是通用的   有Avatar（保存映射关系）
	
## 动画代码
	animator.deltaPosition	表示动画片段里的位移
	animator.rootPosition	运用根骨骼的位移

	代码控制动画位移要重写：
			void OnAnimatorMove(){}

## 动画分层
	1、动画控制层
	2、数据模型层
	3、逻辑控制层
	4、位移控制层


##碰撞体之间忽略碰撞
	尤其用在CharacterController之间	
    Physics.IgnoreCollision(collider1, collider2);

##unity资源动态加载
	unity资源类型：
		a) Unity内置的常用资源     fbx\Prefab\jpg
		b) textasset  txt\binaty等，有对应的TextAsset类可直接读写
		c) scriptable object   它是序列化的Object实例，例如把一个Object实例保存成scriptable object，读入进来后就直接在内存中变成那个实例

##unity工作线程（2018以后）
	从Unity2018开始开放了工作线程，可以使用JobSystem做一些耗时的事情。
	工作线程可和主线程并行运行。
	例：集成IJobParallelForTransform接口，并实现Execute()

	主线程和工作线程之间的数据需要使用Native来传递，工作线程只需要读数据，并不需要设置数据，所以需要标记为readonly。
	例：[ReadOnly]public NativeArray<Vector3> position;

##unity脚本编译规则
	unity脚本分运行时脚本和编辑时脚本两大类，运行时脚本会编译进游戏包中，编辑时脚本仅用于编辑器模式下。
	脚本存放的目录决定了它将编译在哪个dll文件中。
	plugins文件夹中非Editor中的脚本将编译在Assembly-CSharp-firstpass.dll中，是最先编译的脚本。
	plugins文件夹中Editor中的脚本将编译在Assembly-CSharp-Editor-firstpass.dll中。
	Assets文件夹中的脚本将编译在Assembly-CSharp.dll文件中。
	Assets/Editor文件夹中的脚本将便已在Assembly-CSharp-Editor.dll文件中，是最后编译的脚本。

#图形学
###OpenGL渲染流程
	顶点着色器 →→ 光栅化 →→ 片段着色器 →→ alpha测试 →→ 模板测试 →→ 深度测试 →→ Blend →→ Gbuffer →→ frontBuffer →→frame buffer →→ 显示器
###像素
	RGBA四个通道     RGBA8888  表示每个通道占8bit

###顶点着色器
	1，计算顶点的颜色
	2，将物体坐标系转换到相机坐标系

###光栅化
	将顶点转换成像素

###片段着色器
	已经是像素点
	1，纹理采样    从纹理像素赋给像素
	2，像素和灯光进行计算

###alpha测试
	挑选合格alpha的像素显示

###模板测试
	像素还可以携带模板信息，达到条件的模板才显示。

###深度测试
	符合条件的像素就通过，不然就丢弃。

###Blend
	将当前要渲染的像素和已经渲染出来的像素混合运算。

###GBuffer
	保存 RGBA 模板值 深度值等

###Front Buffer

###Frame Buffer



#Shader2.0
	#Pragma vertex vert  定义一个顶点着色器的入口函数
	#Pragma fragment frag  定义一个片段着色器的入口函数

##语义
	POSITION：获取模型顶点信息
	NORMAL:获取法线信息
	TEXCOORD(n):高精度地从顶点传递信息到片段着色器
	COLOR：低精度地从顶点传递信息到片段着色器
	TANGENT：获取模型切线信息
	SV_POSITION：表示经过“MVP”矩阵已经转化到屏幕坐标的位置
	SV_Target：输出到那个render target


##MVP矩阵
	M:物体坐标系变换到世界坐标系
	V:世界坐标系变换到相机坐标系
	P:将3D坐标系转换成2D屏幕坐标系


##PureMVC
	Proxy=>Model
	Mediator=>View
	Command=>Controller

##Unity协程（Coroutine）
	协程基本功能：
		1.将协程代码由 yield return 语句分割的部分分配到每一帧去执行。
		2.yield return 后的值是等待类(WaitForSeconds、WaitForFixedUpdate)时需要等待相应时间。
		3.yield return 后的值还是协程(Coroutine)时需要等待嵌套部分协程执行完毕才能执行接下来的内容。

	Unity协程调用次序大部分都在Update之后LateUpdate之前。

	协程实现原理（在LateUpdate中实现）：
		1.分帧
			每次 MoveNext() 后会返回 yield return 后的内容，yield return之前的内容都会在 MoveNext() 时执行。
			只要把 MoveNext() 移到每一帧去执行，就可实现分帧。
		2.延时等待
			获取Current值根据其类型判断是或否需要等待，假设Current值是需要等待类型，那就延时到倒计时结束；而Current值是非等待类型，那就不需要等待，直接MoveNext()执行后续的代码。
		3.嵌套等待
			与延时等待完全一致

	协程注意事项：
	通过设置MonoBehaviour脚本的enabled对协程是没有影响的，但如果gameObject.SetActive(false)则已经启动的协程则完全停止了，即使在Inspector把gameObject激活还是没有继续执行。也就说协程虽然是在MonoBehvaviour启动的（StartCoroutine）但是协程函数的地位完全是跟MonoBehaviour是一个层次的，不受MonoBehaviour的状态影响，但跟MonoBehaviour脚本一样受gameObject控制，也应该是和MonoBehaviour脚本一样每帧“轮询” yield 的条件是否满足。注：WaitForSends()受Time.timeScale影响，当Time.timeScale = 0f时，yieldreturn new WaitForSecond(X)将不会满足。
#
	线程、进程和协程的区别
	1、进程有自己独立的堆和栈，即不共享堆也不共享栈，进程由操作系统调度。
	2、线程拥有自己独立的栈和共享的堆，共享堆不共享栈，线程亦由操作系统调度。
	3、协程和线程一样，共享堆不共享栈，协程由程序员调度。
	一个应用程序一般对应一个进程，一个进程一般有一个主线程，还有若干个辅助线程，线程之间是平行的，在线程里面可以开启协程，让程序在特定的时间内运行。

#C#基础
##C#特性
	C#的特性：特性（Attribute）是用于在运行时传递程序中各种元素（比如：类、方法、结构、枚举、组件等）的行为信息的声明性标签。您可以通过使用特性向程序添加声明性信息。特性用于添加元数据，如编译器指令和注释、描述、方法、类等其他信息。
	.NET框架提供了三种定义特性：
	AttributeUsage：自定义特性
	Conditional：定义条件方法 [Conditional("DEBUG")]
	Obsolete：不应被使用的程序实体（过时的）
#C#元数据和反射
	元数据（MetaData）是包含程序以及类型信息的数据，它保存在程序的程序集当中。

	程序在运行的时候，可以查看其他程序集或其本身的元数据，这个行为就是反射（Reflection）。
	可以使用反射动态地创建类的实例，将类绑定到现有对象，或从现有对象中获取类。然后可以调用类的方法或访问其字段和属性。
	反射优点：
	1、反射提高了程序的灵活性和扩展性。
	2、降低耦合性，提高自适应能力。
	3、它允许创建和控制任何类的对象，无需提前硬编码目标类。
	反射缺点：
	1、性能问题：使用反射基本上是一种解释操作，用于字段和方法接入时要远慢于直接代码。因此反射机制主要应用在对灵活性和扩展性要求很高的系统框架上，普通程序不建议使用。
	2、使用反射会模糊程序内部逻辑，程序员希望在源代码中看到程序的逻辑，反射缺绕过了源代码的技术，因而会带来维护的问题，反射代码比相应的直接代码更复杂。

##运算符
	c = a++：先将a赋值给c，再对a进行自增运算；
	c = ++a：先将a进行自增运算，再将a赋值给c；
	c = a--：先将a赋值给c，再对a进行自减运算；
	c = --a：先将a进行自减运算，再将a赋值给c。
##C# 集合（Collection）类
	动态数组（ArrayList）：它代表了可被单独索引的对象的有序集合。
	哈希表（Hashtable）：它使用键来访问集合中的元素。
	排序列表（SortedList）：它可以使用键和索引来访问列表中的项。
	堆栈（Stack）：它代表了一个后进先出的对象集合。
	队列（Queue）：它代表了一个先进先出的对象集合。
	点阵列（BitArray）：它代表了一个使用值1和0来表示的二进制数组。
##泛型（Generic）
	泛型约束：
	public class CacheHepler<T> where T : new () {}
	T:结构 （类型参数必须是值类型，可以指定出Nullable以外的任何值类型）
	T:类    (类型参数必须是引用类型，包括任何类、接口、委托或数组类型)
	T:new() (类型参数必须具有无参数的公共构造函数。当与其它约束一起使用时 new()约束必须最后指定)
	T:<基类名>   (类型参数必须是指定的基类或派生自指定的基类)
	T:<接口名称> (类型参数必须是指定的接口或实现指定的接口。可以指定多个接口约束。约束接口也可以是泛型的。)
##C# 不安全代码
	当一个代码块使用unsafe修饰符标记时，C#允许在函数中使用指针变量。不安全代码或非托管代码是指使用了指针变量的代码块。
#
	指针是值为另一个变量的地址的变量。即，内存位置的直接地址。就像其他变量或敞亮，您必须在使用指针存储其他变量地址之前声明指针。
	int var = 20; int* p = &var;
	p为var的值      (int)p为var的地址
	
	int[] list = {10,100,200};
	fixed(int *ptr = list)
	(int)(ptr+i) 为 list[i] 的地址
		*(ptr+i) 为 list[i] 的值

	若要编译不安全代码需在编译器中设置。

##C# 数据类型
	值类型
	值类型直接包含数据，存储在堆栈中。比如int、char、float、struct。	
#
	引用类型又叫托管类型
	引用类型不包含存储在变量中的实际数据，但他们包含对变量的引用。他们指向一个内存地址。引用类型的变量把实际数据的地址（引用）保存在堆栈中，而实际数据则保存在堆中。
	内置的引用类型有：object对象类型、dynamic动态类型和string字符串类型。
	当一个值类型转换为对象类型时，则被称为装箱；
	当一个对象类型转换为值类型时，则被称为拆箱。
#
	指针类型
	指针类型变量存储另一种类型的内存地址。

#
	值类型和引用类型的区别
	 1. 值类型分配在内存栈上，引用类型分配在托管堆上。当一个值类型的变量赋给另一个值类型的变量时，会执行一次逐字段的复制，而一个引用类型的变量赋给另一个引用类型的变量时，仅仅会复制对象的内存地址。
     2. 基于上一条，多个引用类型的变量可以同时指向同一个对象，对其中的任何一个变量执行操作都会影响到另一个变量引用的对象。而每个值类型的变量都已经包含了自己的对象，所以对值类型对象的操作不会影响到另一个值类型变量。
     3. 值类型包括结构和枚举，他们均间接或直接派生自System.ValueType类；引用类型包括类和接口，他们都派生自System.Object类（这一句是废话，所有的类型都派生自System.Object类，可说可不说）。
     4. 值类型都是隐式密封的，不能将一个值类型作为基类来定义一个新的值类型或者引用类型，也因此值类型中不能包括虚方法（不能被继承，虚方法给谁重写呢）。
     5. 默认情况下，创建一个引用类型的变量时，他会被初始化为null；而创建一个值类型时，他的所有成员都会被初始化为0.
     6. 值类型的变量一旦超过了其作用域，为他分配的内存就会被立即释放；而引用类型则会在托管堆里待一段时间，直到垃圾回收器将其回收。
     7. 由于System.ValueType类重写了Equals方法，所以两个值类型的Equals方法会在两个对象的字段完全匹配的情况下返回true；而引用类型的Equals则会在两个变量引用同一个对象的情况下才返回true。（这一条不重要，不说也无所谓，但是如果被问到自己要有所了解）。

## C# GC
	为什么要使用GC？
		1、提高了软件开发的抽象度；
		2、程序员可以将精力集中在实际的问题上而不用分心来管理内存的问题；
		3、可以使模块的接口更加的激情戏，减小模块间的耦合；
		4、大大减少了内存认为管理不当所带来的Bug；
		5、使内存管理更加高效。
#
	GC(垃圾回收)，以应用程序的root为基础，遍历应用程序在Heap(堆)上动态分配的所有对象，通过识别它们是否被引用来确定哪些对象是已经死亡的、哪些仍需要被使用。已经不再被应用程序的root或者别的对象所引用的对象就是已经死亡的对象，即所谓的垃圾，需要被回收。这就是GC工作的原理。为了实现这个原理，GC有多种算法，比较常见的算法有Reference Counting、Mark Sweep、Copy Collection等等。目前主流的虚拟系统.NET CLR，Java VM和Rotor都是采用的Mark Sweep算法。
#
	.NET的GC机制有两个问题：
		1、GC并不能释放所有的资源。它不能自动释放非托管资源。
		2、GC并不是实时性的，这将会造成系统性能上的瓶颈和不确定性。
#
	GC注意事项：
		1、制管理内存；非托管资源，如文件句柄、GDI资源，数据库连接等还需要用户去管理。
		2、循环引用，网状结构等的实现会变得简单。GC的标志-压缩算法能有效的检测这些关系，并将不再引用的网状结构整体删除。
		3、GC通过从程序的根对象开始遍历来检测一个对象是否可被其他对象访问，而不是用类似于COM中的引用计数方法。
		4、GC在一个独立的线程中运行来删除不再被引用的内存。
		5、GC每次运行时会压缩托管堆。
		6、你必须对非托管资源的释放负责。可以通过在类型中定义Finalizer来保证资源得到释放。
		7、对象的Finalizer被执行的时间是在对象不再被引用后的某个不确定的时间。注意并非和C++中一样在对象超出生命周期时立即执行析构函数。
		8、Finalizer的使用有性能上的代价。需要Finalization的对象不会立即被清除，而需要先执行Finalizer.Finalizer，不是GC执行的线程被调用。GC把每一个需要执行Finalizer的对象放到一个队列中去，然后启动另一个线程来执行所有这些Finalizer，而GC线程继续去删除其他待回收的对象。在下一个GC周期，这些执行完Finalizer的对象的内存才会被回收。
		9、.NET GC使用“代”(generations)的概念来优化性能。代帮助GC更迅速的识别哪些最可能成为垃圾的对象。在上次执行完垃圾回收后新创建的对象为第0代对象。经历了一次GC周期的对象为第1代对象。经历了两次或更多的GC周期的对象为第2代对象。代的作用是为了区分局部变量和需要在应用程序生存周期中一直存活的对象。大部分第0代对象是局部变量。成员变量和全局变量很快变成第1代对象并最终成为第2代对象。
		10、GC对不同代的对象执行不同的检查策略以及优化性能。每个GC周期都会检查第0代对象。大约1/10的GC周期检查第0代和第1代对象。大约1/100的GC周期价差所有的对象。重新思考Finalization的代价：需要Finalization的对象可能比不需要Finalization在内存中停留额外9个GC周期。如果此时它还没有被Finalize，就变成第2代对象，从而在内存中停留更长时间。
#
	GC何时会被调用？
		1.第一代没有足够的内存时被调用。
		2.程序员调用了GC.Collect()时。
		3.系统内存资源不足时。

##C# 实例化
	1.用new关键字实例化一个类
		new关键字用于创建对象和调用构造函数。
	2.用Activator实例化一个类
		Activator用以在本地或从远程创建对象类型，或获取对现有远程对象的引用。其CreateInstance方法创建在程序集中定义的类型的实例。
	3.用Assembly实例化一个类
		Assembly表示一个程序集，它是一个可重用、无版本冲突并且可自我描述的公共语言运行库应用程序构造块。该类可以加载程序集、浏览程序集的元数据和构成部分、发现程序集中包含的类型以及创建这些类型的实例。
##C#Lambda表达式
	C#的Lambda 表达式都使用 Lambda 运算符 =>，该运算符读为“goes to”。语法如下：
	(object argOne, object argTwo) => {; /*Your statement goes here*/}
	函数体多于一条语句的可用大括号括起。

	可以将此表达式分配给委托类型
	delegate int del(int i);
	del myDelegate=x=>{return x*x;};
	int j = myDelegate(5); //j=25

##C#委托和事件的区别
	1.委托是一个类，可以被实例化，通过委托的构造函数可以把方法赋值给委托实例。
	2.触发委托的有2中方式：委托实例.Invoke 和 委托实例（参数列表）
	3.事件可以看作是一个委托类型的变量
	4.事件只能+=、-=不能=。

##readonly和const
	const：一个包含不能修改的值的变量。const是在编译时可被完全计算的表达式，因此不可以用变量来初始化。
	readonly：允许把一个字段设置成常量，可以执行一些运算，可以确定它的初始值。它可以用变量来初始化。readonly是实例成员，所以不同的实例可以有不同的常量值。
#
	区别：
	1.const字段只能在该字段的声明中初始化。
	readonly字段可以在声明或构造函数中初始化。因此根据所使用的构造函数，readonly字段可能具有不同的值。
	2.const字段是编辑时常数，而readonly字段可用于运行时常数。
	3.const默认就是静态的，而readonly如果要设置成静态就必须显示声明。
	4.const对于引用类型的常数，可能的值只能是string和null。
	readonly可以是任何类型。
	

#Lua基础

##Lua循环
	for循环
	for var = exp1, exp2, exp3 do
		循环体
	end
	var从exp1变化到exp2，每次变化以exp3为递增值。exp3是可选的，如果不指定，默认为1。
	for循环的三个表达式在循环开始前一次性求值，以后不在进行求值。
	
	泛型for循环
	a = {"one", "two", "three"}
	for i, v in ipairs(a) do
		print(i, v)
	end

	while循环
	while(condition)
	do
		statements
	end

	repeat..until循环
	repeat
		statements
	until(condition)

##Lua判断
	if 条件 then
		do something
	elseif 条件 then
		do something
	else
		do something
	end

##Lua继承
	Lua元表中的__index元方法可以实现继承关系
#
	Panel.lua(父类):
	Panel = class("Panel");
	function Panel : New()
		local p = {};
		setmetatable(o, self);
		self.__index = self;
		return o;
	end

	需要继承Panel的脚本中(子类):
		--继承语句
	ChildPanel = class("ChildPanel", Panel);
	
	使用时:
	cp = ChildPanel : New();
	
##Lua模块
	Lua中可以把一些公用的代码放在一个文件里，以API接口的形式在其他地方调用，有利于代码的重用和降低代码耦合度，这就是模块。
	模块是由变量、函数等已知元素组成的table，因此创建一个模块就是创建一个table。
#
	require函数
	require函数用来加载模块。
	require（“模块路径”） 或 require “模块路径”
	执行require后会返回一个由模块敞亮或函数组成的table，并且还会定义一个包含该table的全局变量。

##Lua元表（Metatable）
	在Lua Table中我们可以访问对应的key来得到value值，但是却无法对两个table进行操作。
	因此Lua提供了元表（Metatble），允许我们改变table的行为，每个行为关联了对应的元方法。
	有两个很重要的函数来处理元表：
	setmetatable（table，metatable）：对指定table设置元表（metatable），如果元表（metatable）中存在__metatable键值，setmetatable会失败。
	getmetatable（table）：返回对象的元表（metatable）。
#
	__index元方法
	当通过键来访问table的时候，如果这个键没有值，那么Lua就会寻找该table的metatable（假设table中有metatable）中的__index键。如果__index包含一个表格，Lua会在表格中查找相应的键。
	如果__index包含一个函数的话，Lua会调用那个函数，table和键会作为参数传递给函数。

	总结
	Lua查找一个表元素时的规则，如下3个步骤
		1、在表中查找，如果找到，返回该元素，找不到则接续
		2、判断该表是否有元表，如果没有元表，返回nil，有元表则继续
		3、判断元表有没有__index方法，如果__index方法为nil，则返回nil；如果__index方法是一个表，则重复1、2、3；如果__index方法是一个函数，则返回该函数的返回值。
#
	__newindex元方法
	__newindex元方法用来更新表。
	__index元方法用来访问表。
	当你给表的一个缺少的索引赋值，解释器就会查找__newindex元方法；如果存在则调用这个函数而不进行赋值操作。
#
	Lua中的元表可以用来为表添加操作符
#
	__call元方法
	__call元方法在Lua调用一个值时调用。
#
	__tostring元方法
	__tostring元方法用于修改表的输出行为。

##Lua协同程序（coroutine）
	Lua协同程序性与线程比较类似：拥有独立的对战，独立的局部变量，独立的指令指针，同时又与其它协同程序共享全局变量和其它大部分东西。
	Lua协同程序与线程的区别主要在于，一个具有多个线程的程序可以同时运行几个线程，而协同程序却需要彼此协作的运行，在任一指定时刻只有一个协同程序在运行，并且这个正在运行的协同程序只有在明确的被要求挂起的时候才会被挂起。

##Lua文件处理 I/O
	简单模式：拥有一个当前输入文件和当前输出文件，并且提供对这些文件相关的操作。
	完全模式：使用外部的文件句柄来实现。它以一种面向对象的形式，将所有的文件操作定义为文件

