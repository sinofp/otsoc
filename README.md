# otsoc
https://github.com/rcore-os/rCore/wiki/os-tutorial-summer-of-code

## 200704

- 完成rustlings

    之前本着“实用编程语言都大同小异”的想法，看完rust的[语法概览](https://fasterthanli.me/articles/a-half-hour-to-learn-rust)后断断续续做过几个rustlings。正好这次也要求做它，索性就在今天做完了。

    <details>
    <summary>初见rust的一些感觉（单击以展开）</summary>

    - if let有点像java的try-with-resources，或者说，python的with

    - enum可以只是字面量，也可以像Haskell的ADT一样。rust的错误处理把enum写成返回值，并不把错误当成别的什么东西，感觉挺有趣，但写起来不方便（一定要处理错误，不处理要像java一样一直向上传，当然这样的确安全）

    - 这个match貌似只是代替了频繁的if-else + isinstance 根据某变量是哪种类型来做出不同动作。match x块的判断条件不能用x，有点鸡肋。

    - 个人感觉rust隐式return、?的写法让函数结束位置看起来不够清晰

    - 所有权、借用像是下一代引用，低级语言还是得靠人和内存搏斗，编译不通过主要是因为它

    - Arc是引用计数，这缩写太晦涩了乍一看以为和数学有关

    - iter类似java的stream，同样的变量collect会根据类型不同collect出不一样的结果（比如result包着vec，或者vec包着result）

    - 不知道为啥泛型时impl后面也要接着\<T\>，理论上说只有结构体有\<T\>也可以吧

    - 把方法放在单独的trait里有个问题：名称对不上，想用parse得实现FromStr，这很反直觉。在class-based语言里只要在类中实现parse就好了——kotlin甚至可以让你在类定义之外实现某种方法，这就很方便。
    </details>

    exercises我会commit两次，一次初始一次完成，这样方便看改了哪里。

    之后会着重看所有权、迭代器（主要看看有哪些方法可用）、多线程、错误处理的部分。

## 200705

- 大致读完了《Rust编程之道》二三四五九章

    > 不要过早迷失于细节。略读即可 —— 本书作者

    <details>
    <summary>学到了不少新东西（甚至感觉对C++更理解了）</summary>

    - Rust名字和标志来源于锈菌，惊了

    - 所有权转移就是当位置表达式出现在值上下文中时，内存地址被转移给另外一个位置表达式

    - 引用也被称为借用，&x是x的借用

    - move || xxx 其实是将捕获到的变量所有权转移给匿名函数，以防止某变量在外面被销毁出现悬垂指针。我昨天以为中间的||是或，疑惑了好久这是什么诡异判断……昨天说“着重看多线程”其实就是不明白这啥意思，现在懂了

    - match用法并不少，不过貌似还是没有把match的结果用匿名函数得到布尔值的方法

    - Box\<T\>其实是将T放到堆上的意思，使用时可以自动解引用（实现Deref），像使用T一样（我喜欢这种透明感）

    - &str的目的是在编译器得到确定大小的字符串，为什么可以得到确定大小？因为引用来引用去第一个被引用的总会是固定的字面量，大小可以由它推出来

    - 用let v:() = xxx的报错信息查看xxx的类型——好，简单朴实，但我选择clion转插件

    - 孤儿规则是为了防止某些类型的行为被破坏性改写（像是方法的所有权）

    - 空impl块代表默认实现，冒号表示继承其他trait，trait可以用韦恩图画出

    - 存放在堆上的数据要通过其放在栈上的指针进行访问——长久以来我一直没意识到这一点，思维里直接把栈上的指针忽略了……

    - 智能指针是实现了Drop和Defef的结构体

    - RAII也有一个别名——作用域界定的资源管理——我一直以为RAII真的只是字面意思获取资源时初始化，忽略了析构函数也是RAII的一部分，所以一直不知道一直强调RAII到底有啥意思

    - 内存泄漏不在内存安全的概念，rust也会内存泄漏

    - enum就是带了标签指明成员的union

    - copy👉值语义、move👉引用语义。

    - 标注生命周期不改变引用的生命周期长短，只用于编译器的检查

    - box只在Rust源码内使用——这个才是“魔法”

    - 可以对Option\<T\>使用map，直接对T进行计算，类似的还有map_or等，这些就是“组合子”——哦，原来组合子就是这些啊，那也不遥远么

    - and_then的返回值不像map那样包装了一层Some
    
    - 可以使用type定义类型别名，讲话函数签名（想起了python的type hint）
    </details>

- 做了两道leetcode

    我也不明白意义不明的“15道笨方法学XXX练习题”是个啥意思，既然说可以用算法题代替，就选了leetcode了。放在fifteen-exercises里。

    和昨天做人家挖好空的rustlings不同，自己写更加感受到了什么叫和编译器格斗。以及对着Option使用map真好。rust的map不止可以处理迭代器，有点通用，都是举起包装直接对里面的货物进行操作。我记得Haskell有个fmap，感觉可能是类似的。

## 200706

今天有事

- 看了一部分《Rust编程之道》unsafe的部分
- 做了一道leetcode一道codewars

    主要是leetcode很多题目不适合rust，看看codewars

## 200707

今天有事

- 大致看完《Rust编程之道》第十三章
    
    > 学习 Unsafe Rust 才能对 Safe Rust 有更深的理解 —— 本章小结

    <details>
    <summary>unsafe这章内容可太多了……</summary>

    - unsafe在解引用裸指针、调用unsafe函数、访问修改该可变静态变量、实现unsafe trait、读写union中的字段时不会进行安全检查

    - 用#\[repr(C)\]告诉编译器使用C语言内存布局

    - 解引用优先级低于方法，高于as

    - as_ptr指向的是存放数据堆/栈的指针，引用是对本身的引用

    - 型变（variance）

        Cat是Animal子类型

        - 协变（covariant）：List\<Cat\>是List\<Animal\>子类型

            Rust大部分结构是协变的（感觉其他语言也是）

            Rust的协变以忘记原始生命周期为代价

        - 逆变（contravariant）：List\<Animal\>是List\<Cat\>子类型

        - 协变（invariant）：List\<Cat\>和List\<Animal\>没关系

        - 长生命周期是短生命周期的子类型

        这玩意前所未闻见所未见……

        - 当协变不会引起ub时，可以用协变，否则保证该类型为不变或逆变
    
    - 有时要改变变量声明顺序，方便编译器推导drop顺序

    - ABI包括调用约定、内存布局、处理器指令集、二进制格式——老听说C ABI，在学rust时知道是啥了

    - C中可以用结构体模拟元组，传递更复杂类型可以用Opaque和Box\<T\>对应

    - 还写了和各种其他语言互动，甚至讲了wasm基本概念，这书快赶上百科全书了
    </details>

- 写了一道codewars，两道leetcode