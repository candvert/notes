## Item 2: Prefer const, enum and inline to \#define
```sh
	应该叫“优先选择编译器而不是预编译器”
	如果#define ASPECT_RATIO 1.653，那么ASPECT_RATIO不会进入symbol table，那么出错信息会指向1.653，这样会不容易找到错误
	
	“the enum hack”：
	class GamePlayer {
	private:
		enum { NumTurns = 5 };
		int scores[NumTurns];
	}
	“the enum hack”使NumTurns成为一个symbolic name，而且它有些方面更像#define而不是const，比如可以对const取地址，但不能对enum取地址。好的编译器不会为整数类型的const分配内存，但有些编译器可能这么做，而enum不会导致不必要的内存分配

	function-like macros有太多弊端，而使用内联函数，可以获得宏的效率以及常规函数的所有可预测行为和类型安全性
```
## Item 3: Use const Whenever possible
```cpp
// 如果要使迭代器指向的内容无法修改，需要const_iterator
std::vector<int> vec;
const std::vector<int>::iterator iter = vec.begin();
*iter = 10;
++iter;    // error! iter is const

std::vector<int>::const_iterator cIter = vec.begin();
*cIter = 10;    // error! *cIter is const
++cIter;


// 让函数返回const可以减少使用者的错误
class Rational { ... };
const Rational operator*(const Rational& lhs, const Rational& rhs);
if (a * b = c) ...    // oops, meant to do a comparison!
```