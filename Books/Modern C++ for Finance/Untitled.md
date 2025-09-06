# uniform initialization
_Uniform initialization_ (also called _braced initialization_), used in some of the preceding examples, was also introduced in C++11:

vector<int> v{1, 2, 3};

string s{"This is a vector:"};

This is a useful feature in general that is discussed in [“Uniform Initialization”](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/learning-modern-c/9781098100797/ch01.html#uni_init_section). For further convenience, you will see later that the template parameter can be dropped in certain cases, using another newer feature called _class template argument deduction_, or _CTAD_ for short.