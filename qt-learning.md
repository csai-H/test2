### Qt三大核心机制
**信号槽**
- 信号槽的五种连接方式
- connect(sender, signal, receiver, slot, 连接方式) // 五个参数

**元对象系统**
- 元对象系统分为三大类：QObject类、Q_OBJECT宏、元对象编译器MOC。对标准C++语言进行扩展，提供了信号槽机制、属性系统、动态翻译等特性。
- QObject类：是所有使用元对象系统的类的基类，提供了信号槽机制、元对象系统等功能
- Q_OBJECT宏：必须在一个类的开头插入宏Q_OBJECT,这样这个类才可以使用元对象系统的特性
- 元对象编译器MOC：用于处理Q_OBJECT宏，生成元对象代码，为每个QObject子类提供必要代码实现元对象系统特性。

**事件模型**

### Qt信号与槽的本质是什么？
- 回调函数。信号或是传递值，或是传递动作变化：槽函数响应信号或是接收值，或者根据动作变化来做出对应操作。

### 信号槽的四种写法和五种连接方式
信号与槽是对象间通信所采用的机制，隐藏了复杂的底层实现，发射信号的对象并不需要知道Qt是如何找到槽函数的。函数connect()有一种成员函数形式，还有多种静态函数形式。一般使用静态函数形式。
- 用宏：connect(sender, SIGNAL(signal()), receiver, SLOT(slot())); // 如果信号和槽带有参数，要注明参数类型
- 用函数指针：connect(sender, &Sender::signal, receiver, &Receiver::slot); // 不需要注明参数类型，信号名称唯一，不存在参数不同的其他同名的信号，用函数指针。
- 用重载函数指针qOverload。connect(sender, &Sender::signal, receiver, qOverload<bool>(&receiver::slot));
- 用Lambda表达式：connect(sender, &Sender::signal, this, [=](){ // 不需要注明参数类型
    // 槽函数体
});
- 用QMetaObject::connectSlotsByName()函数：connectSlotsByName(this); // 槽函数的名称必须是on_对象名_信号名()的形式