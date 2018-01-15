# �ڶ��½�
## ���������
### 2.1.1������
### 2.1.2��˼�뷽�������ݳ���

**compound data types ������������**

### 2.1.1 �������������Ǳ�������������� ���� ����������ģʽ����������⡣
`functions performed operations and data were operated upon. When we included function values among our data, we acknowledged that data too can have behavior. Functions could be operated upon like data, but could also be called to perform computation.`

function��һ����Ϊ�������õ����ݣ�����Ҳ��������Ϊ��
��������������һ����������Ҳ���Ա�����

`Objects represent information, but also _behave_ like the abstract concepts that they represent. The logic of how an object interacts with other objects is bundled along with the information that encodes the object's value.`
�������ݺ���Ϊ��
**����Ͷ���Ľ������ö����ڵ�ֵ�����ݺͺ�����������ϵ**

`Objects are both information and processes, bundled together to represent the properties, interactions, and behaviors of complex things.`
����������Ҳ�Ǵ�����̣�����������ԡ���������Ϊ


����object������ӵ�����

`The name `date` is bound to a _class_. A class represents a kind of object. Individual dates are called _instances_ of that class, and they can be _constructed_ by calling the class as a function on arguments that characterize the instance.`
class������һ���oebjce
**�����Ķ���boject���������ʵ����instances��**


�ҵ������
����һ����
ʲôʲô���Ƕ���
ĳ������ĳ���ʵ��

`Objects have _attributes_, which are named values that are part of the object. In Python, we use dot notation to designated an attribute of an object.`
����������ԣ������Ƕ����һ���֡�
**���ԣ���������value��**

`> <expression> . <name>`
today.year���
**today�Ǳ��ʽ�����ʽ��ֵ֮����Ƕ���**
������expressioopn.name


`Objects also have _methods_, which are function-valued attributes. Metaphorically, the object "knows" how to carry out those methods. Methods compute their results from both their arguments and their object. For example, The `strftime` method of `today` takes a single argument that specifies how to display a date (e.g., `%A` means that the day of the week should be spelled out in full).`
����Ҳ�з����������Ǻ���������



### 2.1.2ԭʼ��������
`Every object in Python has a _type_.`
python���ÿ��������һ������
`So far, the only kinds of objects we have studied are numbers, functions, Booleans, and now dates.`
���֡����������������ڡ����ϡ��ַ���

```
Native data types have the following properties:
1.  There are primitive expressions that evaluate to objects of these types, called _literals_.
2.  There are built-in functions, operators, and methods to manipulate these objects.
```
��ѭ2������
ԭʼ���ʽ�Զ�����ֵ��������
���ú�����������������

```In fact, Python includes three native numeric types: integers (`int`), real numbers (`float`), and complex numbers (`complex`).```
python3����ֵ���ͣ�������ʵ��������






