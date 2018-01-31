# 2.5 Object-Oriented Programming
��*0���ࡢ�������̳к͵������
---

��*1�������������ݳ����3����ͬ�㣺��
---

**1��Like abstract data types, objects create an abstraction barrier between the use and implementation of data
�������������һ��, ���������ݵ�ʹ�ú�ʵ��֮�䴴��һ������߽�
2��objects respond to behavioral requests.
������Ӧ��Ϊ����
3��objects have local state that is not directly accessible from the global environment.
����ӵ�оֲ�״̬�����Ҳ���ֱ�Ӵ�ȫ�ֻ�������**

OOP�������ݳ���ʵ�ֳ���

Each object bundles together local state and behavior in a way that hides the complexity of both behind a data abstraction.
ÿ��������һ���������ݳ��󱳺�ĸ����Եķ�ʽ���ֲ�״̬����Ϊ����һ��

Not only do objects pass messages, they also share behavior among other objects of the same type and inherit characteristics from related types.
���󲻽�Ͷ����Ϣ, ���ǻ���ͬһ���͵���������֮�乲����Ϊ, ����������ͼ̳�������

We have seen that an object is a data value that has methods and attributes, accessible via dot notation.
�����Ѿ�����, һ��������һ������ֵ, �����з���������, ��ͨ�����ʾ�����ʡ�

## 2.5.1 Objects and Classes��������ࣩ
A class serves as a template for all objects whose type is that class. Every object is an instance of some particular class.
�������ж����ģ��, ������Ϊ���ࡣÿ��������ĳ���ض����ʵ����

The act of creating a new object instance is known as _instantiating_ the class.
�����������Ϊ��������ʵ����

An _attribute_ of an object is a name-value pair associated with the object, which is accessible via dot notation.
�������������ö������������-ֵ��, ��ͨ�����ʾ����������

instance attributes may also be called _fields_, _properties_, or _instance variables_.
ʵ�����ԣ��ֶΡ����ԡ�ʵ������

The side effects and return value of a method can depend upon, and change, other attributes of the object.
�����ĸ����úͷ����ķ���ֵ�����ڶ�����������ԡ�

## 2.5.2 Defining Classes����Ķ��壩

When a class statement is executed, a new class is created and bound to &lt;name&gt; in the first frame of the current environment. The suite is then executed. Any names bound within the &lt;suite&gt; of a class statement, through def or assignment statements, create or modify attributes of the class.
**�������ִ�к�һ���µ��౻�����������ڵ�ǰ�����ĵ�һ��ջ֡�ϰ󶨵�name��Ȼ��suite���ִ�У���class����ڵ�suite��κ�name�󶨡�ͨ��def���߸�ֵ��䣬���������޸��������**

Classes are typically organized around manipulating instance attributes,
��ͨ��Χ�Ʋ���ʵ����������֯

The class specifies the instance attributes of its objects by defining a method for initializing new objects.
��ͨ�������ʼ���¶���ķ�����ָ�������ʵ������
 
The method that initializes objects has a special name in Python,
��ʼ������ķ����� Python ����һ�����������,

**Identity**
Object identity is compared using the `is` and `is not` operators.
�����֤��ͨ��is �� is notȥ�Ƚ�

As usual, binding an object to a new name using assignment does not create a new object.
**ͨ����ֵ����һ���������°�һ���µı������ǲ��ᴴ��һ������ģ�ֻ�Ƕ�������á�**
New objects that have user-defined classes are only created when a class (such as `Account`) is instantiated with call expression syntax.
**�����Ƕ������Щclass����ͨ�����ñ��ʽ�﷨�����õ�ʱ�򣬲Ż�ȥ��ʼ��һ����Ķ���**


**Methods.** Object methods are also defined by a `def` statement in the suite of a `class` statement.
����.���󷽷�ͨ�� def ��䶨�塣

Each method definition again includes a special first parameter `self`, which is bound to the object on which the method is invoked.
ÿ���������嶼����һ������ĵ�һ������ self, ���󶨵����ø÷����Ķ���
��*2��self���Ѻ����󶨸�self��self�������Լ�����ʲôʱ��󶨣��������õ�ʱ��Ű󶨡�
---

When a method is invoked via dot notation, the object itself (bound to `tom_account`, in this case) plays a dual role. First, it determines what the name `withdraw` means; `withdraw` is not a name in the environment, but instead a name that is local to the `Account` class. Second, it is bound to the first parameter `self` when the `withdraw` method is invoked. The details of the procedure for evaluating dot notation follow in the next section.
������ͨ������ã����������2����ɫ
1�����withdraw��ʲô
2�������÷���ʱ, ���󶨵���һ������ self��


## 2.5.3 Message Passing and Dot Expressions
The central idea in message passing was that data values should have behavior by responding to messages that are relevant to the abstract type they represent. Dot notation is a syntactic feature of Python that formalizes the message passing metaphor. 
��Ϣ�����е�����˼����, ����ֵӦ�þ�����Ӧ����������ʾ�ĳ���������ص���Ϣ����Ϊ�����ʾ���� Python ��һ�־䷨����, ����ʽ������Ϣ���ݵ�������

```
<expression> . <name>
```
**<name>�����Ǹ��򵥵����ƣ���������ֵΪname�ı��ʽ��
���磺��ȷ�ģ�list.append       ����ģ�list.('append')**

Using `getattr`, we can look up an attribute using a string, just as we did with a dispatch dictionary.
ʹ�� "getattr", ���ǿ���ʹ���ַ�����������, ��������ʹ�õ����ֵ�һ����

```
>>> getattr(tom_account, 'balance')
10

```

We can also test whether an object has a named attribute with `hasattr`.
���ǻ����Բ���һ�������Ƿ���о��� hasattr ���������ԡ�

```
>>> hasattr(tom_account, 'deposit')
True
```

**Method and functions.**
 Method��the object is bound to the parameter `self`.
��*3�������self���ˣ��Ǿ���method��û�󶨾���function��Methodָ��һ�������Function�����ʶ���Function
---

function��     `class.��������`
method ��      `obejct.��������`


As an attribute of a class, a method is just a function, but as an attribute of an instance, it is a bound method:
��Ϊ���һ������, ����ֻ��һ������, ����Ϊһ��ʵ��������, ����һ���󶨷���:

``` python
>>> class A:
    def hello():
        pass

>>> A.hello()
>>> type(A.hello)       # �����౾����ã�û�а󶨣�������function
<class 'function'>
>>> type(A().hello)     # ���Ƕ�����ã����ˣ�������Method
<class 'method'>
```

��*4��@staticmethod
---

<font color=#FF0000  size=5  face="����">��̬��������function��û�а��ࡣ</font>
**��̬������һ��ĺ���û���������в���û��һ����class��ء�
��Ϊ����Щ������Ĵ��������У���Ҫ�õ��Ǹ�function�������Ĺ�ϵ��һ����Ҫ�󶨹�ϵ��ֻ�Ǵ�����Ϊһ�����߹��������ʹ�ã�**
<font color=#FF0000  size=5  face="����">��̬�������Ա����е�ʵ�������ã�Ҳ���Ա������</font>
``` python
>>> class B:
    @staticmethod
    def hi(a):
        return a

>>> type(B.hi)
<class 'function'>
>>> type(B().hi)
<class 'function'>
>>> B.hi(1)                     # �����
1
>>> B().hi(2)                   # ʵ������
2
```
��*5��@staticmethod��;(�����й�ϵ�Ĺ��ܵ�������ʱ�ֲ���Ҫʵ���������������:����������������, �����ܷ��ʸ����ʵ��.ע�⣺@staticmethod����ĺ���û��self����)����
---
������һЩ�����й�ϵ�Ĺ��ܵ�������ʱ�ֲ���Ҫʵ�����������������Ҫ�õ���̬����**.** ������Ļ������������޸�����������Ե����õ���̬����**.** �����������ֱ���ú������, ������ͬ������ɢ���ڲ��Ĵ��룬���ά������<meta charset="utf-8">

��������:

```

IND = 'ON'
def checkind():
    return (IND == 'ON')
class Kls(object):
     def __init__(self,data):
        self.data = data
def do_reset(self):
    if checkind():
        print('Reset done for:', self.data)
def set_db(self):
    if checkind():
        self.db = 'new db connection'
        print('DB connection made for:',self.data)
ik1 = Kls(12)
ik1.do_reset()
ik1.set_db()
```

<meta charset="utf-8">���ʹ��@staticmethod���ܰ���صĴ���ŵ���Ӧ��λ����**.**

```

IND = 'ON'
class Kls(object):
    def __init__(self, data):
        self.data = data
    @staticmethod
    def checkind():
        return (IND == 'ON')
    def do_reset(self):
        if self.checkind():
            print('Reset done for:', self.data)
    def set_db(self):
        if self.checkind():
            self.db = 'New db connection'
        print('DB connection made for: ', self.data)
ik1 = Kls(12)
ik1.do_reset()
ik1.set_db()

```


��*6��@classmethod
---
<font color=#FF0000  size=5  face="����">�෽������Method���������ࡣ���Ա����е�ʵ�������ã�Ҳ���Ա�����ã�</font>
**�Ǳ����е�ʵ�����õģ�����״̬������أ�������˵��ʵ����ء�**
**���磺���е������������˻���һ���ģ����Կ�����classmethod��**
``` python
>>> class C:
    @classmethod
    def ci(self, a):
        return a

>>> type(C.ci)
<class 'method'>
>>> type(C().ci)
<class 'method'>
>>> C().ci(1)                   # ʵ������
1
>>> C.ci(2)                     # �����
2
```

��*7��@classmethod�ô�(ֻ���������ж�����ʵ�������еķ���������func2�����ڿ���ʹ���༰������, �������ض���ʵ����ע�⣺@classmethod����ķ�����һ��������cls�������౾��)����
---

``` python
class A(object):
    bar = 1
    def func1(self):  
        print ('foo') 
    @classmethod
    def func2(cls):
        print ('func2')
        print (cls.bar)
        cls().func1()   # ���� foo ����
 
A.func2()               # ����Ҫʵ����
```
<meta charset="utf-8">

�������������дһЩ�������ཻ�������Ǻ�ʵ�������ķ�������ô���أ����ǿ�����������дһ���򵥵ķ���������Щ����������������ɢ�������Ĺ�ϵ���ඨ�������. �������������д�ͻᵼ���Ժ����ά��������:

```

def get_no_of_instances(cls_obj):
    return cls_obj.no_inst
class Kls(object):
    no_inst = 0
    def __init__(self):
        Kls.no_inst = Kls.no_inst + 1
ik1 = Kls()
ik2 = Kls()
print(get_no_of_instances(Kls))

```
@classmethod
����Ҫдһ��ֻ���������ж�����ʵ�������еķ��� ����������÷�������ʵ�������У�������ô��:

ʹ��@classmethodװ�����������෽��**.**

```

class Kls(object):
    no_inst = 0
    def __init__(self):
        Kls.no_inst = Kls.no_inst + 1
    @classmethod
    def get_no_of_instance(cls_obj):
        return cls_obj.no_inst
ik1 = Kls()
ik2 = Kls()
print ik1.get_no_of_instance()
print Kls.get_no_of_instance()

```

���:
2
2
�����ĺô���: ���������ʽ�Ǵ�ʵ�����û��Ǵ�����ã������õ�һ���������ഫ�ݹ���

��*8��Ӣ�Ľ��ͣ���
---

``` python
@classmethod means: when this method is called, we pass the class as the first argument instead of the instance of that class (as we normally do with methods). This means you can use the class and its properties inside that method rather than a particular instance
@staticmethod means: when this method is called, we don't pass an instance of the class to it (as we normally do with methods). This means you can put a function inside a class but you can't access the instance of that class (this is useful when your method does not use the instance).
@classmethod ��ζ��: �����ô˷���ʱ, ���ǽ�����Ϊ��һ����������, �����Ǹ����ʵ�� (����ͨ��ʹ�÷���)������ζ���������ڸ÷�����ʹ���༰������, �������ض���ʵ��
@staticmethod ��ζ��: �����ô˷���ʱ, ���ǲ��Ὣ���ʵ�����ݸ��� (��������ͨ��ʹ�÷�������)������ζ�ſ��Խ�������������, �����ܷ��ʸ����ʵ�� (��������ʹ�ø�ʵ��ʱ, �������)��
```


We can call `deposit` in two ways: as a function and as a bound method. In the former case, we must supply an argument for the `self` parameter explicitly. In the latter case, the `self` parameter is bound automatically.
```
>>> Account.deposit(tom_account, 1001)  # The deposit function requires 2 arguments
1011
>>> tom_account.deposit(1000)           # The deposit method takes 1 argument
2011
```
��*9�����÷���2�֣���
---
1������.����(ʵ���Ķ��󣬲���ֵ)
---
2��ʵ���Ķ���.����(����ֵ)
---


In some cases, there are instance variables and methods that are related to the maintenance and consistency of an object that we don't want users of the object to see or use. They are not part of the abstraction defined by a class, but instead part of the implementation. Python's convention dictates that if an attribute name starts with an underscore, it should only be accessed within methods of the class itself, rather than by users of the class.

��*10���»���
---
**�����»��ߡ� ��ʼ�ĳ�Ա��������������������˼��ֻ����������������Լ��ܷ��ʵ���Щ������**
**��˫�»��ߡ� ��ʼ����˽�г�Ա����˼��ֻ��������Լ��ܷ��ʣ����������Ҳ���ܷ��ʵ�������ݡ�**


## 2.5.4 Class Attributes�������ԣ�
��*11�������ԣ���̬��������������suite���棬����Class���棬��Method���档
---
Some attribute values are shared across all objects of a given class.
**�����ԣ�һЩ���������ж����й���**

Class attributes are created by assignment statements in the suite of a `class` statement, outside of any method definition.
**������suite���棬����Class���棬��Method���档**

class attributes may also be called class variables or static variables.
**������Ҳ���Գ�Ϊ�������̬������**

This attribute can still be accessed from any instance of the class.
**���Ա��������κ�ʵ������**

However, a single assignment statement to a class attribute changes the value of the attribute for all instances of the class.
��*11���������Եĵ�����ֵ������ĸ��������ʵ�������Ե���ֵ��
---
To evaluate a dot expression:

1.  Evaluate the `&lt;expression&gt;` to the left of the dot, which yields the _object_ of the dot expression.
2.  `&lt;name&gt;` is matched against the instance attributes of that object; if an attribute with that name exists, its value is returned.
3.  If `&lt;name&gt;` does not appear among instance attributes, then `&lt;name&gt;` is looked up in the class, which yields a class attribute value.
4.  That value is returned unless it is a function, in which case a bound method is returned instead.

Ϊ��������ʽ��
1.  �������ߵ�`<expression>`���������������Ķ���
2.  `<name>`��Ͷ����ʵ������ƥ�䣻��������Ƶ����Դ��ڣ��᷵������ֵ��
3.  ���`<name>`��������ʵ�����ԣ���ô�������в���`<name>`���������������ֵ��
4.  ���ֵ�ᱻ���أ�������Ǹ���������᷵�ذ󶨷�����

## 2.5.5 Inheritance���̳У�
Two classes may have similar attributes, but one represents a special case of the other.
���������ӵ�����Ƶ����ԣ�����һ����ʾ��һ�������������

base class    ����
subclass      ����

���أ���������ͬ������������ͬ�����������Ϳ��ܲ�ͬ
��д����������ͬ����������Ҳ���ܲ�ͬ

With inheritance, we only specify what is different between the subclass and the base class. Anything that we leave unspecified in the subclass is automatically assumed to behave just as it would for the base class.
**�ڼ̳еĻ�����, ����ָֻ������ͻ���֮��Ĳ�ͬ�������������б������κ�δָ�������ݶ����Զ��ٶ�Ϊ�����һ������Ϊ��**

## 2.5.6 Using Inheritance��ʹ�ü̳У�
We can define this procedure recursively. To look up a name in a class.

1.  If it names an attribute in the class, return the attribute value.
2.  Otherwise, look up the name in the base class, if there is one.
����ͨ����������õ������ƺ����Բ��������ָ���̳С�
1.  ��������д���������Ƶ����ԣ���������ֵ��
2.  ��������л���Ļ����ڻ����в��Ҹ����ơ�

## 2.5.7 Multiple Inheritance�����ؼ̳У�
AsSeenOnTVAccount, CheckingAccount, SavingsAccount, Account, object
[![](https://github.com/wizardforcel/sicp-py-zh/raw/master/img/multiple_inheritance.png)
��*12����̳еĲ���˳��
---
�����������ļ򵥡����Ρ���Python �����ҽ������ƣ�֮�����ϡ���������У�Python ������˳�������ƣ�ֱ���ҵ��˾��и����Ƶ����ԣ���ǰ�࣬��һ��Ĵ������ң�����һ����
---

``` python
class D(A, B, C)
D.__mro__    (D, A, B, C)
```