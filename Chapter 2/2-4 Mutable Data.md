# 2.4 Mutable Data(�ɱ�����)

we need strategies to help us structure large systems so that they will be _modular_, that is, so that they can be divided "naturally" into coherent parts that can be separately developed and maintained.
ģ�黯���ֳ�һ���еĸ������֣�ÿ�����ֶ�����Զ����Ŀ�����ά��

One powerful technique for creating modular programs is to introduce new kinds of data that may change state over time.
����ģ�黯��������һ�����͵��������ͣ�������������������ʱ��ı仯���仯

a single data object can represent something that evolves independently of the rest of the program.
�������ݶ�����Ա�ʾ���������ಿ���޹صĶ���

<font color=#FF0000  size=5  face="����">��*1���洢���ã�״̬������</font>
**����������״̬�ģ�����һ�κ͵��ö��ζ���һ���Ľ����**

 numbers, Booleans, tuples, ranges, and strings --- are all types of _immutable_ objects. While names may change bindings to different values in the environment during the course of execution, the values themselves do not change.
���֡�����ֵ��Ԫ�顢��Χ���ַ���---���ǲ��ɱ��������͡���ִ�й�����, ���ƿ��ܻ�󶨻����в�ͬ��ֵ, ����Щֵ��������ġ�

<font color=#FF0000  size=5  face="����">��*2�����֡�������Ԫ�顢Ranges���ַ������ǲ��ɱ����
��*3���仯��ֻ�ǰ󶨹�ϵ�������������ֵ�������仯
��*4���ɱ��������ͣ������ڳ�������й����У��ı������������ֵ�ı���</font>

## 2.4.1 Local State���ֲ�״̬��
<font color=#FF0000  size=5  face="����">��5*��һ����������ӵ���Լ��ľֲ�״̬��ʹ�ô��оֲ�״̬��nonlocal���ĺ��������Ǿ���ʵ�ֿɱ���������</font>

We will do so by creating a function called `withdraw`, which takes as its argument an amount to be withdrawn. If there is enough money in the account to accommodate the withdrawal, then `withdraw` should return the balance remaining after the withdrawal. Otherwise, `withdraw`should return the message `'Insufficient funds'`. For example, if we begin with $100 in the account, we would like to obtain the following sequence of return values by calling withdraw:

����ͨ����������`withdraw`�ĺ�����ʵ����������Ҫȡ���Ľ����Ϊ����������˻������㹻��Ǯ��ȡ����`withdraw`Ӧ�÷���ȡǮ֮���������`withdraw`Ӧ�÷�����Ϣ`'Insufficient funds'`�����磬����������˻��е�`$100`��ʼ������ϣ��ͨ������`withdraw`���õ���������У�
```
>>> withdraw(25)
75
>>> withdraw(25)
50
>>> withdraw(60)
'Insufficient funds'
>>> withdraw(15)
35
```
Observe that the expression `withdraw(25)`, evaluated twice, yields different values. This is a new kind of behavior for a user-defined function: it is non-pure. Calling the function not only returns a value, but also has the side effect of changing the function in some way, so that the next call with the same argument will return a different result. All of our user-defined functions so far have been pure functions, unless they called a non-pure built-in function. They have remained pure because they have not been allowed to make any changes outside of their local environment frame!

�۲���ʽ`withdraw(25)`��**��ֵ�����Σ������˲�ͬ��ֵ������һ���û����庯��������Ϊ�����ǷǴ����������ú�������������һ��ֵ��ͬʱ������һЩ��ʽ�޸ĺ����ĸ����ã�ʹ������ͬ�������´ε��÷��ز�ͬ�Ľ����**���������û�����ĺ�������ĿǰΪֹ���Ǵ��������������ǵ����˷Ǵ����ڽ������������Ծ��Ǵ���������Ϊ���ǲ�**�������޸��κ��ھֲ�����֮֡��Ķ�����**

pure because they have not been allowed to make any changes outside of their local environment frame!
<font color=#FF0000  size=5  face="����">��*6����������ִ���ڼ䲻�����޸��κ��ھֲ�����ջ֮֡��Ķ���</font>


<font color=#FF0000  size=5  face="����">��*7���Ǵ��������˸ı�ֲ�������ջ֮֡�⣬����ı��������ֲ�ջ֮֡��Ķ���</font>


is a higher-order function that takes a starting balance as an argument. The function `withdraw` is its return value.
`make_withdraw`�����Ǹ��߽׺�����������ʼ�����Ϊ������`withdraw`���������ķ���ֵ��


```
>>> withdraw = make_withdraw(100)

```

An implementation of `make_withdraw` requires a new kind of statement: a `nonlocal` statement. When we call `make_withdraw`, we bind the name `balance` to the initial amount. We then define and return a local function, `withdraw`, which updates and returns the value of `balance` when called.

`make_withdraw`��ʵ����Ҫ�����͵���䣺
**`nonlocal`��䡣�����ǵ���`make_withdraw`ʱ�����ǽ�����`balance`�󶨵���ʼֵ�ϡ�֮�����Ƕ��岢�����˾ֲ�������`withdraw`�����ڵ���ʱ���²�����`balance`��ֵ��**

```
>>> def make_withdraw(balance):
        """Return a withdraw function that draws down balance with each call."""
        def withdraw(amount):
            nonlocal balance                 # Declare the name "balance" nonlocal
            if amount > balance:
                return 'Insufficient funds'
            balance = balance - amount       # Re-bind the existing balance name
            return balance
        return withdraw

```

The novel part of this implementation is the `nonlocal` statement, which mandates that whenever we change the binding of the name `balance`, the binding is changed in the first frame in which `balance` is already bound. Recall that without the `nonlocal` statement, an assignment statement would always bind a name in the first frame of the environment. The `nonlocal` statement indicates that the name appears somewhere in the environment other than the first (local) frame or the last (global) frame.
`nonlocal`��䣬����ʲôʱ�������޸�������`balance`�İ󶨣��󶨶�����`balance`֮ǰ�Ѿ��󶨵ĵ�һ��֡���޸ġ�����һ�£���û��`nonlocal`��������£���ֵ������ǻ��ڻ����ĵ�һ��֡�а����ơ�`nonlocal`�����������Ƴ����ڻ����в��ǵ�һ�����ֲ���֡���������һ����ȫ�֣�֡�������ط���
<font color=#FF0000  size=5  face="����">��*8��nonlocal���ı�������������㺯�����ڲ㺯��֮�䡣</font>


![](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nonlocal_def.png)

Our definition statement has the usual effect: it creates a new user-defined function and binds the name `make_withdraw` to that function in the global frame.
�����µ��û����庯�������ҽ�����`make_withdraw`��ȫ��֡�а󶨵��Ǹ�������

Next, we call `make_withdraw` with an initial balance argument of `20`.
���棬����ʹ�ó�ʼ��������`20`������`make_withdraw`��

```
>>> wd = make_withdraw(20)

```

This assignment statement binds the name `wd` to the returned function in the global frame.
�����ֵ��佫����`wd`�󶨵�ȫ��֡�еķ��غ����ϣ�

![](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nonlocal_assign.png)

The returned function, (intrinsically) called _withdraw_, is associated with the local environment for the _make_withdraw_ invocation in which it was defined. The name `balance` is bound in this local environment. Crucially, there will only be this single binding for the name `balance` throughout the rest of this example.
**����`balance`�󶨺�ֻ����һ���󶨣�**

Next, we evaluate an expression that calls _withdraw_ on an amount `5`.

```
>>> wd(5)
15

```

The name `wd` is bound to the _withdraw_ function, so the body of _withdraw_ is evaluated in a new environment that extends the environment in which _withdraw_ was defined. 
**����`wd`�󶨵���`withdraw`�����ϣ���Ϊÿ�ε���һ����������������µ�ջ֡������`withdraw`�ĺ��������µĻ�������ֵ�����µĻ�����`withdraw`�������ڵĻ�������չ������**

![](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nonlocal_call.png)

The assignment statement in _withdraw_ would normally create a new binding for `balance` in _withdraw_'s local frame. Instead, because of the `nonlocal` statement, the assignment finds the first frame in which `balance`was already defined, and it rebinds the name in that frame. If `balance` had not previously been bound to a value, then the `nonlocal` statement would have given an error.

**`withdraw`�ĸ�ֵ���ͨ����`withdraw`�ľֲ�֡��Ϊ`balance`�����µİ󶨡�����`nonlocal`��䣬��ֵ�����ҵ���`balance`����λ�õĵ�һ֡�������������°����ơ�**

<font color=#FF0000  size=5  face="����">��*9��nonlocal��⣨�Լ�����⣩����Ϊ������withdraw������������withdraw�ľֲ�ջ֡�б�������Ϊbalance����һ���µİ󶨣�����Ϊnonlocal��䣬balance֮ǰ�󶨹��������ҵ���֮ǰbalance�󶨵��Ǹ���һ֡��Ȼ����֮ǰ�Ǹ��ط����°󶨡�</font>


By virtue of changing the binding for `balance`, we have changed the _withdraw_ function as well. The next time _withdraw_ is called, the name `balance` will evaluate to `15` instead of `20`.
ͨ���޸�`balance`�󶨵���Ϊ������Ҳ�޸���`withdraw`�������´�`withdraw`���õ�ʱ������`balance`����ֵΪ`15`������`20`��

When we call `wd` a second time,
�����ǵڶ��ε���`wd`ʱ��

```
>>> wd(3)
12

```

we see that the changes to the value bound to the name `balance` are cumulative across the two calls.

![img/nonlocal_recall.png](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nonlocal_recall.png)

<font color=#FF0000  size=5  face="����">��*10����ͼ���Կ��������ڶ��ε���withdraw����ʱ���ִ�����һ���µľֲ�ջ֡��amount�󶨵�3����һ�ε��ö������ľֲ�ջ֡��amount�󶨵�5�����ǣ�balance�İ�һֱ������make_withdraw����������ʱ������ľֲ�ջ֡�У���û����Ϊwithdraw�ĵ��ö��ı䡣�������make_withdraw�ٴε��ã����ᴴ��������֡��banlance����󶨵�����´�����֡��</font>


By introducing `nonlocal` statements, we have created a dual role for assignment statements. Either they change local bindings, or they change nonlocal bindings. In fact, assignment statements already had a dual role: they either created new bindings or re-bound existing names. The many roles of Python assignment can obscure the effects of executing an assignment statement. It is up to you as a programmer to document your code clearly so that the effects of assignment can be understood by others.

<font color=#FF0000  size=5  face="����">��*11����ֵ��������2�н�ɫ
1��������һ���µı�������
2������󶨣����°�һ���µı�����
</font>

## 2.4.2 The Benefits of Non-Local Assignment���Ǿֲ���ֵ�ĺô���
If _make_withdraw_ is called again, then it will create a separate frame with a separate binding for `balance`.
���`make_withdraw`�ٴε��ã����ᴴ��������֡��banlance����󶨵�����´�����֡��

```
>>> wd2 = make_withdraw(7)

```

 The name `wd` is still bound to a _withdraw_ function with a balance of `12`, while `wd2` is bound to a new _withdraw_ function with a balance of `7`.
 ����`wd`�Ծɰ󶨵����Ϊ`12`��`withdraw`�����ϣ���`wd2`�󶨵������Ϊ`7`���µ�`withdraw`�����ϡ�

![](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nonlocal_def2.png)

wd�ľֲ�ջ֡��wd2�ľֲ�ջ֡��ȫ�ǲ�һ���ġ�wd2�����´�����һ��make_withdraw��ջ֡��

Finally, we call the second _withdraw_ bound to `wd2`:
������ǵ��ð󶨵�`wd2`�ϵĵڶ���`withdraw`������
```
>>> wd2(6)
1

```

This call changes the binding of its nonlocal `balance` name, but does not affect the first _withdraw_ bound to the name `wd` in the global frame.
��������޸��˷Ǿֲ�����`balance`�İ󶨣����ǲ�Ӱ����ȫ��֡�а󶨵�����`wd`�ĵ�һ��`withdraw`��

![img/nonlocal_call2.png](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nonlocal_call2.png)

**��ͼ�п��Կ�����balance�İ�������һ���ֲ������У�������֮ǰ���Ǹ�����������˵wd2�ǵ���make_withdraw�����󴴽���һ���µľֲ�������**
<font color=#FF0000  size=5  face="����">��12*�����ⲿ�ĺ���make_withdraw���µ��ã��ᴴ����һ���Լ���ջ֡��
</font>



## 2.4.3 The Cost of Non-Local Assignment���Ǿֲ���ֵ�Ĵ��ۣ�

Previously, our values did not change; only our names and bindings changed. When two names `a` and `b`were both bound to the value `4`, it did not matter whether they were bound to the same `4` or different `4`'s. As far as we could tell, there was only one `4` object that never changed.
֮ǰ�����ǵ�ֵ��û�иı䣬���������ǵ����ƺͰ󶨷����˱仯������������`a`��`b`�󶨵�`4`��ʱ�����ǰ󶨵�����ͬ��`4`���ǲ�ͬ��`4`������Ҫ������˵��ֻ��һ��`4`���󣬲�����������ı䡣

However, functions with state do not behave this way. When two names `wd` and `wd2` are both bound to a _withdraw_ function, it _does_ matter whether they are bound to the same function or different instances of that function. Consider the following example, which contrasts the one we just analyzed.
���ǣ�����״̬�ĺ������������ġ�����������`wd`��`wd2`���󶨵�`withdraw`����ʱ�����ǰ󶨵���ͬ�������Ǻ�����������ͬʵ�����ͺ���Ҫ�ˡ�������������ӣ���������֮ǰ�������Ǹ������෴��

```
>>> wd = make_withdraw(12)
>>> wd2 = wd
>>> wd2(1)
11
>>> wd(1)
10

```

In this case, calling the function named by `wd2` did change the value of the function named by `wd`, because both names refer to the same function. The environment diagram after these statements are executed shows this fact.
���ͨ��`wd2`���ú������޸�����Ϊ`wd`�ĺ�����ֵ����Ϊ�������ƶ�ָ����ͬ�ĺ�������Щ���ִ��֮��Ļ���ͼʾչʾ���������

![img/nonlocal_corefer.png](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/nonlocal_corefer.png)

The key to correctly analyzing code with non-local assignment is to remember that only function calls can introduce new frames. Assignment statements always change bindings in existing frames. In this case, unless _make_withdraw_ is called twice, there can be only one binding for `balance`.

<font color=#FF0000  size=5  face="����">��13*����ȷ��������nonlocal��ֵ����Ĺؼ���
1��ֻ�к������û�����µ�ջ֡
2����ֵ���ֻ�ı��Ѵ���ջ֡�İ󶨹�ϵ��
</font>


**�������`make_withdraw`���������Σ�`balance`����ֻ��һ���󶨡�**

## 2.4.4 Lists(�б�)
All of these _mutation operations_ change the value of the list; they do not create new list objects.
�б��ǿɱ���������

<font color=#FF0000  size=5  face="����">��*14��is �� is not�ж��Ƿ�Ϊͬһ������
���������ҽ������ڴ��е�λ����ͬʱΪͬһ������</font>
 Python includes two comparison operators, called `is` and `is not`, that test whether two expressions in fact evaluate to the identical object. Two objects are identical if they are equal in their current value, and any change to one will always be reflected in the other. Identity is a stronger condition than equality.

Python �����������Ƚ������������`is`��`is not`���������������ʽʵ�����Ƿ���ֵΪͬһ�����������������ĵ�ǰֵ��ȣ�����һ������ĸı�ʼ�ջ�Ӱ����һ������ô����������ͬһ����������Ǹ�������Ը�ǿ��������

```
>>> suits is nest[0]
True
>>> suits is ['heart', 'diamond', 'spade', 'club']
False
>>> suits == ['heart', 'diamond', 'spade', 'club']
True

```

The final two comparisons illustrate the difference between `is` and `==`. The former checks for identity, while the latter checks for the equality of contents.
**���������Ƚ�չʾ��`is`��`==`������ǰ�߼����ݣ������߼�����ݵ�����ԡ�**


<font color=#FF0000  size=5  face="����">**��*15��List comprehensions.**���б��Ƶ�ʽ��</font>
A list comprehension uses an extended syntax for creating lists, analogous to the syntax of generator expressions.
���Ƶ�ʽʹ����չ�﷨�������б������������ʽ���﷨���ơ�
``` python
a = [(x,y) for x in range(10) for y in range(10) if x % 2 == 0 if y % 2 != 0]
```

Our mutable list is a dispatch function, just as our functional implementation of a pair was a dispatch function. It checks the input "message" against known messages and takes an appropriate action for each different input. Our mutable list responds to five different messages. The first two implement the behaviors of the sequence abstraction. The next two add or remove the first element of the list. The final message returns a string representation of the whole list contents.

���ǵĿɱ��б��Ǹ����Ⱥ�������������ż�Եĺ���ʽʵ��Ҳ�Ǹ����Ⱥ�������������롰��Ϣ���Ƿ�Ϊ��֪��Ϣ�����Ҷ�ÿ����ͬ������ִ����Ӧ�Ĳ��������ǵĿɱ��б����Ӧ�����ͬ����Ϣ��ǰ����ʵ�������г������Ϊ����������������ӻ�ɾ���б�ĵ�һ��Ԫ�ء�������Ϣ���������б����ݵ��ַ�����ʾ��
<font color=#FF0000  size=5  face="����">��ôͨ������ʵ���б�</font>
```
>>> def make_mutable_rlist():
        """Return a functional implementation of a mutable recursive list."""
        contents = empty_rlist
        def dispatch(message, value=None):
            nonlocal contents
            if message == 'len':
                return len_rlist(contents)
            elif message == 'getitem':
                return getitem_rlist(contents, value)
            elif message == 'push_first':
                contents = make_rlist(value, contents)
            elif message == 'pop_first':
                f = first(contents)
                contents = rest(contents)
                return f
            elif message == 'str':
                return str(contents)
        return dispatch

```

We can also add a convenience function to construct a functionally implemented recursive list from any built-in sequence, simply by adding each element in reverse order.
����Ҳ�������һ�����������������κ��ڽ������й�������ʽʵ�ֵĵݹ��б�ֻ��Ҫ�Եݹ�˳�����ÿ��Ԫ�ء�
```
>>> def to_mutable_rlist(source):
        """Return a functional list with the same contents as source."""
        s = make_mutable_rlist()
        for element in reversed(source):
            s('push_first', element)
        return s

```

In the definition above, the function `reversed` takes and returns an iterable value; it is another example of a function that uses the conventional interface of sequences.
������Ķ����У�����`reversed`���ܲ����ؿɵ���ֵ������ʹ�����еĽӿ�Լ������һ��ʾ����

At this point, we can construct a functionally implemented lists. Note that the list itself is a function.
������ǿ��Թ��캯��ʽʵ�ֵ��б�Ҫע���б�����Ҳ�Ǹ�������

```
>>> s = to_mutable_rlist(suits)
>>> type(s)
<class 'function'>
>>> s('str')
"('heart', ('diamond', ('spade', ('club', None))))"

```

In addition, we can pass messages to the list `s` that change its contents, for instance removing the first element.
���⣬���ǿ������б�`s`������Ϣ���޸��������ݣ������Ƴ���һ��Ԫ�ء�

```
>>> s('pop_first')
'heart'
>>> s('str')
"('diamond', ('spade', ('club', None)))"

```

In principle, the operations `push_first` and `pop_first` suffice to make arbitrary changes to a list. We can always empty out the list entirely and then replace its old contents with the desired result.

## 2.4.5 Dictionaries���ֵ䣩
 A dictionary contains key-value pairs, where both the keys and values are objects. The purpose of a dictionary is to provide an abstraction for storing and retrieving values that are indexed not by consecutive integers, but by descriptive keys.
 �ֵ�����˼�ֵ�ԣ����м���ֵ�������Ƕ����ֵ��Ŀ�����ṩһ�ֳ������ڴ���ͻ�ȡ�±겻�����������������Եļ���ֵ��
 
 Looking up values by their keys uses the element selection operator that we previously applied to sequences.
 ���ǿ���ʹ��Ԫ��ѡ�����������ͨ��������ֵ������֮ǰ�����������С�

```
>>> numerals['X']
10

```

A dictionary can have at most one value for each key. Adding new key-value pairs and changing the existing value for a key can both be achieved with assignment statements.
�ֵ��ÿ�������ֻ��ӵ��һ��ֵ������µļ�ֵ�Ի����޸�ĳ����������ֵ������ʹ�ø�ֵ���������ɡ�

```
>>> numerals['I'] = 1
>>> numerals['L'] = 50
>>> numerals
{'I': 1, 'X': 10, 'L': 50, 'V': 5}
```
Notice that `'L'` was not added to the end of the output above. Dictionaries are unordered collections of key-value pairs.
Ҫע�⣬`'L'`��û����ӵ����������ĩβ���ֵ�������ļ�ֵ�Լ��ϡ�

Dictionaries do have some restrictions:

*   A key of a dictionary cannot be an object of a mutable built-in type.
*   There can be at most one value for a given key.
<font color=#FF0000  size=5  face="����">��*16��Լ����
1��key�����ǲ��ɱ����
2��ÿһ��keyֻ����һ��value</font>

Dictionaries also have a comprehension syntax analogous to those of lists and generator expressions. Evaluating a dictionary comprehension yields a new dictionary object.
<font color=#FF0000  size=5  face="����">��*17���ֵ��Ƶ�ʽ����
�ֵ�Ҳӵ���Ƶ�ʽ�﷨�����б��Ƶ�ʽ�����������ʽ���ơ�����ֵ��Ƶ�ʽ������µ��ֵ����</font>


```
>>> {x: x*x for x in range(3,6)}
{3: 9, 4: 16, 5: 25}
```
**Implementation.** We can implement an abstract data type that conforms to the dictionary abstraction as a list of records, each of which is a two-element list consisting of a key and the associated value.
���ǿ���ʵ��һ�������������ͣ�����һ����¼���б����ֵ����һ�¡�ÿ����¼��������Ԫ�ص��б�����������ص�ֵ��
```
def make_dict():
    """Return a functional implementation of a dictionary."""
    records = []

    def getitem(key):
        for k, v in records:
            if k == key:
                return v

    def setitem(key, value):
        for item in records:
            if item[0] == key:
                item[1] = value
                return
        records.append([key, value])

    def dispatch(message, key=None, value=None):
        if message == 'getitem':
            return getitem(key)
        elif message == 'setitem':
            setitem(key, value)
            #print(records)
        elif message == 'keys':
            return tuple(k for k, _ in records)
        elif message == 'values':
            return tuple(v for _, v in records)

    return dispatch

d = make_dict()
print(d('setitem', 3, 9))
print(d('setitem', 4, 16))
print(d('getitem', 3))
print(d('keys'))
print(d('values'))
```
