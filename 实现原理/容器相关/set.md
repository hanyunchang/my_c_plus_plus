在C++中，set并没有直接基于map来实现。set和map都是基于一种叫做红黑树的平衡二叉搜索树实现的。

然而，在一些具体的实现中，set可能会利用map实现。例如，set可以被视为一个只有关键字的map，即每个元素只包含一个关键字，而没有对应的值。这样，set的一些操作就可以通过调用相应的map函数来完成。

但是，这种实现并不是标准，也不是必须的。在很多C++库的实现中，set和map都是直接基于红黑树实现的，而没有利用map来实现set。


------重要的私有成员-------
在C++中，std::set 的具体实现细节可能会根据不同的编译器或者标准库的实现有所不同。通常情况下，我们不需要关心这些具体的实现细节，因为这些细节被封装在类的私有成员中，我们不应该直接访问这些成员。

在一些常见的C++标准库的实现中，std::set 的私有成员通常会包括一个指向红黑树节点的指针，用于存储set中的元素，以及一个比较对象，用于比较set中的元素。

例如，在GCC的libstdc++中，std::set 的实现如下：

template<typename _Key, typename _Compare, typename _Alloc>
class set
{
  // ...
private:
  typedef _Rb_tree<_Key, _Key, _Identity<_Key>, _Compare, _Alloc> _Rep_type;

  _Rep_type _M_t;  // Red-black tree representing set.
};
在这个实现中，_M_t 是一个红黑树，用于存储set中的元素。_Compare 是一个比较函数对象，用于比较元素。

再次强调，这些都是实现细节，我们在编程时不应该直接访问这些私有成员，而应该使用set提供的公有成员函数来操作set。