# Blaster
服了，写在wiki里面的内容没有了

2025-08-25

做了很多很多东西

首先是动画：先引入了Learning kit，主要是获取资源，学习了如何将骨骼动画转移到目标骨骼上面：UE5.4和之前的版本不一样，UE5.4需要通过给Mesh创建IK_Rig，然后在IK_Rig里面标好骨骼点的链，通过两个IK_Rig和IK_Retargeter来重定向骨骼动画。不过IK_Rig可以自动生成链，IK_Retargeter也能自动匹配，很方便。不过在做重定向前，要把骨骼的造型摆一致，比如都是T型。比较方便的是在IK_Retargeter里面摆。外面也能摆，但没有仔细去研究。

25-09-03

学到了第40节

做了武器组件和装备武器，没有太难理解的点，偏流程化的东西
不过学习了给骨骼加一个socket，然后可以通过这个socket加进去武器来

25-09-10

学到第41节

学了客户端RPC到服务器的用法UFUNCTION(Server, Reliable) 其中Reliable标识可靠传输

客户端调用ServerEquipButtonPressed

实际服务器执行的是ServerEquipButtonPressed_Implementation

再次学习了复制

复制要在GetLifetimeReplicatedProps函数里面写DOREPLIFETIME这样的声明，标识复制的变量

只能在这个函数里面处理，因为

1. DOREPLIFETIME 的作用

它不是“立刻复制变量”。

它的本质是 往 OutLifetimeProps 这个配置表里登记一条规则：
“这个类有个属性要复制，复制时按什么条件来”。

这个登记过程，UE 只会在 类的网络注册阶段 执行一次。
也就是说，它属于类的静态描述信息，而不是运行时逻辑。

2. GetLifetimeReplicatedProps 的特殊角色

引擎会在 Actor 类初始化（Class Default Object，CDO 创建）时调用一次 GetLifetimeReplicatedProps。

这时候引擎收集所有的复制规则，生成一个 FLifetimeProperty 列表，放在类的元信息里。

之后运行时，Server → Client 同步时就用这份规则来决定哪些变量需要复制。
