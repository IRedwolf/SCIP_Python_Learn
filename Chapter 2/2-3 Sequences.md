# 2.3Sequences�����У�
A sequence is an ordered collection of data values
����ֵ�����򼯺�

there are many kinds of sequences, but they all share certain properties
�����кܶ����͵����У����������й�ͬ������

��*0�����е�2���������ԣ����ȡ�Ԫ��ѡ��
---

Sequences provide a layer of abstraction that may hide the details of exactly which sequence type is being manipulated by a particular program.
���ã������ṩ��һ�������, �����������ض��������������������͵���ϸ��Ϣ��

## 2.3.1 Nested Pairs��Ƕ�׵�ֵ�ԣ�
A standard way to visualize a pair --- in this case, the pair `(1,2)` --- is called _box-and-pointer_ notation. Each value, compound or primitive, is depicted as a pointer to a box. The box for a primitive value contains a representation of that value. For example, the box for a number contains a numeral. The box for a pair is actually a double box: the left part contains (an arrow to) the first element of the pair and the right part contains the second.
���ӻ�ż�Ե�һ����׼���� -- ����Ҳ����ż��`(1,2)`-- �������Ӻ�ָ��Ǻ�
ż�Եĺ���ʵ�������������ӣ���ߵĲ��֣���ͷָ��ģ�����ż�Եĵ�һ��Ԫ�أ��ұߵĲ��ְ����ڶ�����

---
![](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/pair.png)
��python���
```
>>> ((1, 2), (3, 4))
((1, 2), (3, 4))
```
![](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nested_pairs.png)

Our ability to use tuples as the elements of other tuples provides a new means of combination in our programming language
�ڱ���������ṩ��һ���µ�����ֶΣ�ʹ��Ԫ����Ϊ����Ԫ���Ԫ��

---
a method for combining data values satisfies the closure property if the result of combination can itself be combined using the same method. Closure is the key to power in any means of combination because it permits us to create hierarchical structures --- structures made up of parts, which themselves are made up of parts, and so on. We will explore a range of hierarchical structures in Chapter 3\. For now, we consider a particularly important structure.
����ԣ��Լ������Լ����԰������������������������ȥ������νṹ

---
## 2.3.2 Recursive Lists���ݹ��б�
A non-empty sequence can be decomposed into:
*   its first element, and
*   the rest of the sequence.

һ���ǿ����п��Ի���Ϊ��
*   ���ĵ�һ��Ԫ�أ��Լ�
*   ���е����ಿ�֡�

---
These two selectors, one constructor, and one constant together implement the recursive list abstract data type. The single behavior condition for a recursive list is that, like a pair, its constructor and selectors are inverse functions.
������ѡ������һ�����������Լ�һ��������ͬʵ���˳����������͵ĵݹ��б��ݹ��б�Ψһ����Ϊ�����ǣ�����ż�����������Ĺ�������ѡ�������෴�ĺ�����

*   If a recursive list `s` was constructed from element `f` and list `r`, then `first(s)` returns `f`, and `rest(s)` returns `r`.

We can use the constructor and selectors to manipulate recursive lists.

```
>>> counts = make_rlist(1, make_rlist(2, make_rlist(3, make_rlist(4, empty_rlist))))
>>> first(counts)
1
>>> rest(counts)
(2, (3, (4, None)))
```
---
*   ���һ���ݹ��б�`s`��Ԫ��`f`���б�`r`���죬��ô`first(s)`����`f`������`rest(s)`����`r`��

���ǿ���ʹ�ù�������ѡ�����������ݹ��б�

```source-python
>>> counts = make_rlist(1, make_rlist(2, make_rlist(3, make_rlist(4, empty_rlist))))
>>> first(counts)
1
>>> rest(counts)
(2, (3, (4, None)))
```
---

The recursive list can store a sequence of values in order, but it does not yet implement the sequence abstraction. Using the abstract data type we have defined, we can implement the two behaviors that characterize a sequence: length and element selection.

�ݹ��б���԰�˳��洢һϵ��ֵ, ������û��ʵ�����г���ʹ�����Ƕ���ĳ�����������, ���ǿ���ʵ���������е�������Ϊ: ���Ⱥ�Ԫ��ѡ��

```
>>> def len_rlist(s):
        """Return the length of recursive list s."""
        length = 0
        while s != empty_rlist:
            s, length = rest(s), length + 1
        return length

```

```
>>> def getitem_rlist(s, i):
        """Return the element at index i of recursive list s."""
        while i > 0:
            s, i = rest(s), i - 1
        return first(s)

```

Now, we can manipulate a recursive list as a sequence:

����, ���ǿ��Խ��ݹ��б����Ϊһ������:
```
>>> len_rlist(counts)
4
>>> getitem_rlist(counts, 1)  # The second item has index 1
2
```
---

## 2.3.3 Tuples II��Ԫ�飩
Tuples can have arbitrary length, and they exhibit the two principal behaviors of the sequence abstraction: length and element selection. Below, `digits` is a tuple with four elements.
Ԫ���������ĳ��ȣ�����Ҳӵ�����г��������������Ϊ�����Ⱥ�Ԫ��ѡ�������`digits`��һ����Ԫ��Ԫ�顣
```
>>> digits = (1, 8, 2, 8)
>>> len(digits)
4
>>> digits[3]
8
```

Additionally, tuples can be added together and multiplied by integers. For tuples, addition and multiplication do not add or multiply elements, but instead combine and replicate the tuples themselves. That is, the `add`function in the `operator` module (and the `+` operator) returns a new tuple that is the conjunction of the added arguments. The `mul` function in `operator` (and the `*` operator) can take an integer `k` and a tuple and return a new tuple that consists of `k` copies of the tuple argument.
���⣬Ԫ����Ա˴�����Լ���������ˡ�����Ԫ�飬�ӷ��ͳ˷�����������Ԫ����ӻ���ˣ�������Ϻ��ظ�Ԫ�鱾��Ҳ����˵��`operator`ģ���е�`add`�������Լ�`+`�������������������Ԫ�����ӳɵ���Ԫ�顣`operator`ģ���е�`mul`�������Լ�`*`���������������`k`��Ԫ�飬�����غ���Ԫ�����`k`����������Ԫ�顣

��*1���ӷ��ͳ˷�����
---
Ԫ����Լ�������Ԫ��
Ԫ����Գ�������


```source-python
>>> (2, 7) + digits * 2
(2, 7, 1, 8, 2, 8, 1, 8, 2, 8)
```
**Mapping.** A powerful method of transforming one tuple into another is by applying a function to each element and collecting the results.
**ӳ�䡣**map(����������) �������ÿ��Ԫ�ض����������Ĳ������ݸ�������һһ���ú����õ������
ptyhon3���ص���map���ͣ�����ͨ��tuple����listǿ������ת��

```source-python
>>> alternates = (-1, 2, -3, 4, -5)
>>> tuple(map(abs, alternates))
(1, 2, 3, 4, 5)
```

The `map` function is important because it relies on the sequence abstraction: we do not need to be concerned about the structure of the underlying tuple; only that we can access each one of its elements individually in order to pass it as an argument to the mapped function (`abs`, in this case).

---
��*2��map�����ǳ���Ҫ����Ϊ�����������г������ǲ���Ҫ���ĵײ�Ԫ��Ľṹ��ֻ��Ҫ�ܹ���������ÿ��Ԫ�أ��Ա㽫����Ϊ������������ӳ��ĺ����У�������abs����
---

## 2.3.4 Sequence Iteration�����еĵ�����
Mapping is itself an instance of a general pattern of computation: iterating over all elements in a sequence.
һ��һ�������������Ԫ�أ�ֱ���������������Ԫ��
Python has an additional control statement to process sequential data: the `for`statement.
Python ��ӿ�������������������ݣ�`for`���

---
Consider the problem of counting how many times a value appears in a sequence. We can implement a function to compute this count using a `while` loop.
���Ǽ���һ��ֵ�������г����˶��ٴε����⣬�����ǿ���ʹ��`while`ѭ��ʵ��һ���������������������

```
>>> def count(s, value):
        """Count the number of occurrences of value in sequence s."""
        total, index = 0, 0
        while index < len(s):
            if s[index] == value:
                total = total + 1
            index = index + 1
        return total

```

```
>>> count(digits, 8)
2

```

The Python `for` statement can simplify this function body by iterating over the element values directly, without introducing the name `index` at all. `For` example (pun intended), we can write:

Python `for`������ͨ��ֱ�ӵ���Ԫ��ֵ������������壬��ȫ����Ҫ����`index`�����ǿ���д�ɣ�
```
>>> def count(s, value):
        """Count the number of occurrences of value in sequence s."""
        total = 0
        for elem in s:
            if elem == value:
                total = total + 1
        return total

```

```
>>> count(digits, 8)
2
```
��*3��ѭ����while��䣬������for���
---
---

A `for` statement consists of a single clause with the form:

for �������

```
for <name> in <expression>:
    <suite>

```

A `for` statement is executed by the following procedure:

`for`��䰴�����¹�����ִ�У�

1.  Evaluate the header `&lt;expression&gt;`, which must yield an iterable value.
<expression>�������һ���ɵ�����ֵ
2.  For each element value in that sequence, in order:
    1.  Bind `&lt;name&gt;` to that value in the local environment.
�������ÿһ��ֵ���󶨵�name�ֲ�������
    2.  Execute the `&lt;suite&gt;`.
Ȼ��ִ��suite

An important consequence of this evaluation procedure is that `&lt;name&gt;` will be bound to the last element of the sequence after the `for` statement is executed.

��*4�������ֵ���̵�һ����Ҫ����ǣ���for���ִ�����֮��name��󶨵����е����һ��Ԫ���ϡ�
---

**Sequence unpacking�����н����.**
A common pattern in programs is to have a sequence of elements that are themselves sequences, but all of a fixed length. `For` statements may include multiple names in their header to "unpack" each element sequence into its respective elements. For example, we may have a sequence of pairs (that is, two-element tuples),
�����е�һ������ģʽ�ǣ����е�Ԫ�ر���������У����Ǿ��й̶��ĳ��ȡ�`for`������ͷ���а���������ƣ���ÿ��Ԫ�����С��⹹��Ϊ����Ԫ�ء����磬����ӵ��һ��ż�ԣ�Ҳ���Ƕ�Ԫ�飩�����У�
```
>>> pairs = ((1, 2), (2, 2), (2, 3), (4, 4))

```

and wish to find the number of pairs that have the same first and second element.

```
>>> same_count = 0

```

The following `for` statement with two names in its header will bind each name `x` and `y` to the first and second elements in each pair, respectively.
��*5�������`for`����ͷ�������������ʣ��Ὣÿ������`x`��`y`�ֱ�󶨵�ÿ��ż�Եĵ�һ���͵ڶ���Ԫ���ϡ�
---

```
>>> for x, y in pairs:
        if x == y:
            same_count = same_count + 1

```

```
>>> same_count
2

```

This pattern of binding multiple names to multiple values in a fixed-length sequence is called _sequence unpacking_; it is the same pattern that we see in assignment statements that bind multiple names to multiple values.
**����󶨶�����Ƶ����������ж��ֵ��ģʽ���������н��������ģʽ�������ڸ�ֵ����п����ģ���������ư󶨵����ֵ��ģʽ��ͬ��**
��*6��RangeҲ��һ�����С�
---
**Ranges.** A `range` is another built-in type of sequence in Python, which represents a range of integers. Ranges are created with the `range` function, which takes two integer arguments: the first number and one beyond the last number in the desired range.
`range`����һ�� Python ���ڽ��������ͣ�����ʾһ��������Χ����Χ����ʹ��`range`�����������������������������������÷�Χ�ĵ�һ����ֵ�����һ����ֵ��һ��

---
```
>>> range(1, 10)  # Includes 1, but not 10
range(1, 10)

```

Calling the `tuple` constructor on a range will create a tuple with the same elements as the range, so that the elements can be easily inspected.

�ڷ�Χ�ϵ���`tuple`�������ᴴ���뷶Χ������ͬԪ�ص�Ԫ�飬ʹԪ�����ڲ鿴��
```
>>> tuple(range(5, 8))
(5, 6, 7)
```
If only one argument is given, it is interpreted as one beyond the last value for a range that starts at 0.

���ֻ�ṩ��һ��Ԫ�أ��������Ϊ���һ����ֵ��һ����Χ��ʼ�� 0��
```
>>> tuple(range(4))
(0, 1, 2, 3)

```

Ranges commonly appear as the expression in a `for` header to specify the number of times that the suite should be executed:

```
>>> total = 0
>>> for k in range(5, 8):
        total = total + k

```

```
>>> total
18

```

A common convention is to use a single underscore character for the name in the `for` header if the name is unused in the suite:

��*7��Լ���׳ɣ�������ñ��������»��ߣ�_����ֵ��
---
```
>>> for _ in range(3):
        print('Go Bears!')

Go Bears!
Go Bears!
Go Bears!

```

Note that an underscore is just another name in the environment as far as the interpreter is concerned, but has a conventional meaning among programmers that indicates the name will not appear in any expressions.
�Խ�������˵���»�������һ�����ƣ������ڳ���Ա�о��й̶����壬������������Ʋ�Ӧ�������κα��ʽ�С�

## 2.3.5 Sequence Abstraction�����г���
We have now introduced two types of native data types that implement the sequence abstraction: tuples and ranges. Both satisfy the conditions with which we began this section: length and element selection. Python includes two more behaviors of sequence types that extend the sequence abstraction.
**�����Ѿ�����������ԭ���������ͣ�����ʵ�������г���Ԫ���Range��������������һ�¿�ʼʱ�����������Ⱥ�Ԫ��ѡ��**
``` python
>>> a = (1,2,3)
>>> len(a)             #���еĳ���
3
>>> a[2]               #���е�Ԫ��ѡ��
3
>>> b = range(10)
>>> b[1]               #���е�Ԫ��ѡ��
1
>>> len(b)             #���еĳ���
10
```
**Python �������������������͵���Ϊ��������չ�����г���**

��*8�����е�2����Ϊ����Ա���ԡ���Ƭ
--
**��Ա���ԣ��ж�һ��Ԫ���Ƿ������е��У�in | not in��**
**��Ƭ**
**Membership����Ա���ԣ�.** A value can be tested for membership in a sequence. Python has two operators `in` and `not in` that evaluate to `True` or `False` depending on whether an element appears in a sequence.
���Բ���һ��ֵ�������еĳ�Ա�ԡ�Python ӵ������������`in`��`not in`��ȡ����Ԫ���Ƿ��������г��ֶ���ֵΪ`True`��`False`��

```
>>> digits
(1, 8, 2, 8)
>>> 2 in digits
True
>>> 1828 not in digits
True

```

All sequences also have methods called `index` and `count`, which return the index of (or count of) a value in a sequence.

**�������ж��н���`index`��`count`�ķ��������᷵��������ĳ��ֵ���±꣨������������**
``` python
>>> a = (1,1,1,2,3,4,4,5)
>>> a.count(1)
3
```
**Slicing����Ƭ��.** 
In Python, sequence slicing is expressed similarly to element selection, using square brackets. A colon separates the starting and ending indices. Any bound that is omitted is assumed to be an extreme value: 0 for the starting index, and the length of the sequence for the ending index.
**Python �У�������Ƭ�ı�ʾ������Ԫ��ѡ��ʹ�÷����š�ð�ŷָ�����ʼ�ͽ����±ꡣ�κα߽��ϵ�ʡ�Զ�����������ֵ����ʼ�±�Ϊ 0�������±������г��ȡ�**

```
>>> digits[0:2]
(1, 8)
>>> digits[1:]
(8, 2, 8)
```

## 2.3.6 Strings���ַ�����
The native data type for text in Python is called a string, and corresponds to the constructor `str`.
Python ��ԭ�����ı��������ͽ����ַ�������Ӧ�Ĺ�������`str`��

String literals can express arbitrary text, surrounded by either single or double quotation marks.
�ַ������Ա�������ı����������Ż���˫���Ű�Χ��

```
>>> 'I am string!'
'I am string!'
>>> "I've got an apostrophe"
"I've got an apostrophe"
>>> '����'
'����'

```
Strings satisfy the two basic conditions of a sequence that we introduced at the beginning of this section: they have a length and they support element selection.
�ַ�����������������������������������һ�ڿ�ʼ���ܹ����ǣ�����ӵ�г��Ⱥ�Ԫ��ѡ��

```
>>> city = 'Berkeley'
>>> len(city)
8
>>> city[3]
'k'

```

The elements of a string are themselves strings that have only a single character. A character is any single letter of the alphabet, punctuation mark, or other symbol. Unlike many other programming languages, Python does not have a separate character type; any text is a string, and strings that represent single characters have a length of 1.
�ַ�����Ԫ�ر�����ǰ�����һ�ַ����ַ������ַ�����ĸ���е����ⵥһ�ַ��������ţ������������š�������������������������Python û�е������ַ����ͣ��κ��ı������ַ�������ʾ��һ�ַ����ַ�������Ϊ 1

Like tuples, strings can also be combined via addition and multiplication.
��*10���ַ���Ҳ����ͨ���ӷ��ͳ˷�����ϣ�
---
```
>>> 'Berkeley' + ', CA'
'Berkeley, CA'
>>> 'Shabu ' * 2
'Shabu Shabu '

```

**Membership.** The behavior of strings diverges from other sequence types in Python. The string abstraction does not conform to the full sequence abstraction that we described for tuples and ranges. In particular, the membership operator `in` applies to strings, but has an entirely different behavior than when it is applied to sequences. It matches substrings rather than elements.
�ַ�������Ϊ��ͬ�� Python �������������͡��ַ�������û��ʵ������ΪԪ���Ranges�������������г����ر�أ��ַ�����ʵ���˳�Ա�������`in`�������������ϵ�ʵ�־�����ȫ��ͬ����Ϊ����ƥ�����ַ���������Ԫ�ء�
```
>>> 'here' in "Where's Waldo?"
True

```

Likewise, the `count` and `index` methods on strings take substrings as arguments, rather than single-character elements. The behavior of `count` is particularly nuanced; it counts the number of non-overlapping occurrences of a substring in a string.
��֮���ƣ��ַ����ϵ�`count`��`index`���������Ӵ���Ϊ�����������ǵ�һ�ַ���`count`����Ϊ��ϸ΢�����ͳ���ַ����з��ص��ִ��ĳ��ִ�����
��*11��count����
---
```
>>> 'Mississippi'.count('i')
4
>>> 'Mississippi'.count('issi')
1
```
---
**Multiline Literals.** Strings aren't limited to a single line. Triple quotes delimit string literals that span multiple lines. We have used this triple quoting extensively already for docstrings.
**�����ı���**�ַ������������ڵ����ı����������ŷָ����ַ������Կ�Խ���С������Ѿ����ĵ��ַ�����ʹ�����������š�

```
>>> """The Zen of Python
claims, Readability counts.
Read more: import this."""
'The Zen of Python\nclaims, "Readability counts."\nRead more: import this.'

```

In the printed result above, the `\n` (pronounced "_backslash en_") is a single element that represents a new line. Although it appears as two characters (backslash and "n"), it is considered a single character for the purposes of length and element selection.

��*12] �������ŵĶ����ı���\n����������б�ܼ� n�����Ǳ�ʾ���еĵ�һԪ�ء���Ȼ����ʾΪ�����ַ�����б�ܺ� n�������ڳ��Ⱥ�Ԫ��ѡ���ϱ���Ϊ�ǵ����ַ���
---
``` python
#�����ı���
>>> '''asdfasdf
zxcvzxcv
qweqweqwe
'''
'asdfasdf\nzxcvzxcv\nqweqweqwe\n'
���еط���Ĭ�ϼ����˻��з���
����ǣ�11111111111111\n222222222222222
```

��*13�������ַ�����﷽ʽ��������������
---
``` python
#�����ַ�����("11111111111"
            "22222222222")
����ǣ�1111111111122222222222
```

**String Coercion.** A string can be created from any object in Python by calling the `str` constructor function with an object value as its argument. This feature of strings is useful for constructing descriptive strings from objects of various types.
�ַ������Դ� Python ���κζ���ͨ����ĳ������ֵ��Ϊ��������`str`���캯��������������ַ��������Զ��ڴӶ������͵Ķ����й����������ַ����ǳ�ʵ�á�

```
>>> str(2) + ' is an element of ' + str(digits)
'2 is an element of (1, 8, 2, 8)'

```
��*14��Methods��������
---

```
>>> '1234'.isnumeric()
True
>>> 'rOBERT dE nIRO'.swapcase()
'Robert De Niro'
>>> 'snakeyes'.upper().endswith('YES')
True
```

## 2.3.7 Conventional Interfaces������ӿڣ�

��*15�����м���ģʽ��ö�١�ӳ�䡢ɸѡ�������ģʽ
---
```
 enumerate     map    filter  accumulate
-----------    ---    ------  ----------
naturals(n)    fib    iseven     sum
```

The filter function takes a sequence and returns those elements of a sequence for which a predicate is true
����һ�����У����������ж���Ϊ���Ԫ�ء� ���ص���filter���󣬿���ת��ΪԪ��

��*16��Generator expressions ���������ʽ�����������������ݽṹ�ķ���
---
```
<map expression> for <name> in <sequence expression> if <filter expression>
```
(x*2 for x in range(5) if x%2==0) ���ص���generator����������
list(x*2 for x in range(5) if x%2==0) listǿ��ת�����������б�

``` python
# <map expression> for <name> in <sequence expression> if <filter expression>

a = (x * 2 for x in range(10) if x % 2 == 0)
print(a)            # <generator object <genexpr> at 0x0000000002A69E10>
b = list((x * 2 for x in range(10) if x % 2 == 0))
print(b)            # [0, 4, 8, 12, 16]

def double(x):
    return x * 2

c = list((double(x) for x in range(10) if x % 2 == 0))
print(c)            # [0, 4, 8, 12, 16]

d = list(lambda x : x * 2 for x in range(10) if x % 2 == 0)
print(d)            # ����

e = lambda x : x * 2
f = list(d for x in range(10) if x % 2 == 0)

# �ܽ᣺ʹ�����������ʽ��Ҫ�ȰѺ������������
```

[17*] reduce(����������) 
---
Python includes `reduce` in the `functools` module, which applies a two-argument function cumulatively to the elements of a sequence from left to right, to reduce a sequence to a value.
Python ��`functools`ģ���а���`reduce`�����������е�Ԫ����ȡ��2�������ݸ������õ���ֵ�����е���һ��Ԫ�ش��ݸ�����������ֱ��ȡ������Ԫ�أ��õ�����ֵ��
``` python
reduce(lambda x, y:x*2+y*2, (1,2,3,4))
�ȴ���1��2���ٴ���6��3����󴫵�18��4
```
