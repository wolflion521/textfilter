很短但是觉得挺有用的东东
所以单独立了个项目备份一下

USAGE:

    >>> f = DFAFilter()
    >>> f.add("sexy")
    >>> f.filter("hello sexy baby")
    hello **** baby
    
算法：
本文将敏感词替换  这样定义，将敏感词存到某一个列表或者链表中，然后在每一句话里面搜索是否有列表中的词
## 1. 第一种方法
暴力for循环，对敏感词表for循环，是个O(n)的，str的replace不知道是O多少的，两者嵌套，算法会很慢。
缺点：比如敏感词表里面有['a b  m','a0   x  y', 'a1  e  i', 'a2  m  i','a3  x  v','b  m','b1  c']等等。
由于敏感词一般都不是只有一个字，一旦多个字之间相互排列组合，那么就会造成非常长的列表。
## 2. 第二种方法
为了解决这个问题，第二种方法用Back Sorted Mapping. 具体看了代码知道核心的思想是将长长的列表，提前分成多个子表，
这里采用的方式是对每一个字都建一个子的dict，这样搜索的时候，两级搜索，可以空间换时间。
两级的表结构。顶层是一个个单词，下一层是这些单词的所有组合出来的敏感词。
具体的实现，一个list存str,一个set用来在插入新的敏感词的时候做个判断,一个dict用来存各个字的str的index。
## 3. 第三种方法
叫做DFA。从代码来看，是在第二种方法的基础上更进一步。
顶层是字符char




其他细节的补充
* 正则字符串：re.compile(r'^[0-9a-zA-Z]+$')匹配以[]中字符开头的，且可以有一个到多个。^表示匹配以字符串的开始，$匹配字符串的结束。
* 如果if后面跟的是字符串，则只要这个字符串不为空串，python就把它看作True
* 对于字符串进行string.strip()  string.decode('utf-8')   string.lower()     
* 字符串变成unicode。 str = unicode(str)
* 有哪些地方要类型判断？ 用isinstance.
    * 往表里面添加新的关键字的时候，以及filter一句话的时候，一旦不是unicode的就按照utf-8来decode解析。if not isinstance(keyword, unicode):  keyword = keyword.decode('utf-8').lower().strip()。
    * 使用dict结构存储时，初始化dict之前需要后面这个判断。if not isinstance(level, dict):break
* 这是一个转义符，bai\xhh表示二位十六进制，在这里\x00 == 0x00            \x77 == 0x77。代码的作用应该就是看一下res地址对应的启示两个字节的值是否为0x00和0x77，对应于ascii字符的NULL和w
