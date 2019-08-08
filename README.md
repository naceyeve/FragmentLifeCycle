# FragmentLifeCycle

> 情景：   在一个Activity中依次添加多个功能Fragment，观察Fragment在添加和回退时的生命周期

依次添加顺序：
第一个Fragment: LeftFragment
第二个Fragment: RightFragment(观察对象)（没有addToBackStack(null)）
第三个Fragment: AnotherFragment(观察对象)（添加addToBackStack(null)）
第四个Fragment:ThirdRightFragment

LeftFragment已添加
RightFragment: onAttach
RightFragment: onCreate
RightFragment: onCreateView
RightFragment: onActivityCreated
RightFragment: onStart
RightFragment: onResume

此时RightFragment 可见

---
添加 AnotherFragment
AnotherRightFragment: onAttach
AnotherRightFragment: onCreate
RightFragment: onPause
RightFragment: onStop
RightFragment: onDestroyView

RightFragment 隐藏
TIP: 无论是否addToBackStack(null)压栈操作,RightFragment 都只执行到onDestroyView

AnotherRightFragment: onCreateView
AnotherRightFragment: onActivityCreated
AnotherRightFragment: onStart
AnotherRightFragment: onResume

AnotherRightFragment 可见

---
添加 ThirdRightFragment
AnotherRightFragment: onPause
AnotherRightFragment: onStop
AnotherRightFragment: onDestroyView

AnotherRightFragment 不可见
ThirdRightFragment 可见
---

Back键回退:
AnotherRightFragment: onCreateView
AnotherRightFragment: onActivityCreated
AnotherRightFragment: onStart
AnotherRightFragment: onResume

AnotherRightFragment 可见
TIP：添加addToBackStack(null)，Fragment出栈会依次调用onCreateView方法

再回退：

RightFragment: onDestroy
RightFragment: onDetach

RightFragment 销毁
AnotherRightFragment 依旧可见

TIP：未添加addToBackStack(null)，Fragment出栈会调用onDestroy方法

再回退：

AnotherRightFragment: onPause
AnotherRightFragment: onStop
AnotherRightFragment: onDestroyView
AnotherRightFragment: onDestroy
AnotherRightFragment: onDetach

AnotherRightFragment 销毁
APP退出


> 总结  ：
1，Fragment 未addToBackStack(null)时，replace也是执行到onDestroyView，不会执行onDestroy，只有在回退到该Fragment时，才执行onDestroy
2，Fragment添加addToBackStack(null)，重新回到运行状态时，会从onCreateView开始执行。




