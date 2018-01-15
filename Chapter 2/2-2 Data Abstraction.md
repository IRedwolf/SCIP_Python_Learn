# 2.2 ���ݳ���
`a?_compound data_?value`
��������ֵ

---
ģ�黯
ĳ�����ݽṹ��ĳ�ֳ������ݡ�
�ֲ�Ρ��ֽṹ�������⻯ΪС����

`The general technique of isolating the parts of a program that deal with how data are represented from the parts of a program that deal with how those data are manipulated is a powerful design methodology called?_data abstraction_. Data abstraction makes programs much easier to design, maintain, and modify.`
**һ�ּ������������չʾ������ָ�������֡����������ݳ���**

`data abstraction is a methodology that enables us to isolate how a compound data object is used from the details of how it is constructed.`
*���ݳ�����һ�������ۣ��ӣ�ʹ�á����죩�ָ�������ݶ���*

�����ߣ��Է��������

```The basic idea of data abstraction is to structure programs so that they operate on abstract data. That is, our programs should use data in such a way as to make as few assumptions about the data as possible. At the same time, a concrete data representation is defined, independently of the programs that use the data. The interface between these two parts of our system will be a set of functions, called selectors and constructors, that implement the abstract data in terms of the concrete representation. To illustrate this technique, we will consider how to design a set of functions for manipulating rational numbers.```
**����ȥ������󣬶�����ȥ������ôʹ�á�
������⣺д���ʱ���Ȳ�д����ĺ������ݣ������ȶ���һ�����Լ���Ҫʹ�õĹ��ܣ������������ڵ��õ�ʱ����ȥ���Ʒ�������**

ѡ������a.b����������������init��

## 2.2.1���������� rational�����ʣ�
`<numerator>/<denominator>`
`Rational numbers are important in computer science`
`Thus, working with rational numbers should, in principle, allow us to avoid approximation errors in our arithmetic.`

```we would like to keep the numerator and denominator separate for the sake of precision, but treat them as a single unit.```
������һ���Ǿ�ȷ�ģ�����������С��������ǲ���ȷ��
�ڼ�����������������ʱ�򣬸������ڰѷ��Ӻͷ�ĸ����������������������ֵ������ֹ������ʧ��

```
We know from using functional abstractions that we can start programming productively before we have an implementation of some parts of our program. Let us begin by assuming that we already have a way of constructing a rational number from a numerator and a denominator. We also assume that, given a rational number, we have a way of extracting (or selecting) its numerator and its denominator. Let us further assume that the constructor and selectors are available as the following three functions:

make_rat(n, d)?returns the rational number with numerator?`n`?and denominator?`d`.
numer(x)?returns the numerator of the rational number?`x`.
denom(x)?returns the denominator of the rational number?`x`.
```
3����װ��
1���Ѿ����˹����������ķ���
2��ӵ�������һ�ַ���������һ�����������ֽ�Ϊ���Ӻͷ�ĸ
3����������ѡ����Ӻͷ�ĸ�ķ���
make_rat(n,d) �����ӷ�ĸ������������
number(x) �������������ط���
denom(y) �������������ط�ĸ


## 2.2.2Ԫ��
`multiple assignment.`
���ظ�ֵ��
``` python
pair =(1, 2)
a,b = pari
```

```Tuples in Python (and sequences in most other programming languages) are 0-indexed, meaning that the index?`0`?picks out the first element, index?`1`?picks out the second, and so on. One intuition that underlies this indexing convention is that the index represents how far an element is offset from the beginning of the tuple.```

**������0��ʼ��������룩�����ֵ�͵�һ��Ԫ�صľ�����**

getitem����������ȡ��Ԫ��


��һ��Further reading
`Further reading.?The?str_rat?implementation above uses?_format strings_, which contain placeholders for values. The details of how to use format strings and the?format?method appear in the?[formatting strings](http://diveintopython3.ep.io/strings.html#formatting-strings)?section of Dive Into Python 3.`
ʵ�ֵ�str_rat��ʽ�ַ����������������ռλ����ֵ���й����ʹ�ø�ʽ�ַ����͸�ʽ������ϸ�ڽ�������Python 3�ĸ�ʽ���ַ��������С�


## 2.2.3 �������
```In general, the underlying idea of data abstraction is to identify for each type of value a basic set of operations in terms of which all manipulations of values of that type will be expressed, and then to use only those operations in manipulating the data.```
only��������壺�������
**һ��class�����Ժͷ���ֻ�ܱ��Լ���ʵ�������ã����ܱ������ʵ������**

![](https://wizardforcel.gitbooks.io/sicp-in-python/content/img/barriers.png)

**���������ֻ�������ʹ�üӷ����˷�
���ʵ�ּӷ֣��˷������ʵ��make_rat����εõ����ӡ��õ���ĸ��
���ʵ�ַ��ӡ�ʵ�ַ�ĸ��ʵ�������������ʹ��tuple��getitem**

The fewer functions that depend on a particular representation, the fewer changes are required when one wants to change that representation.
**����Խ�٣��Ժ��޸ĵ�ʱ��Ҳ��Խ��**

## 2.2.4 The Properties of Data
```In general, we can think of an abstract data type as defined by some collection of selectors and constructors, together with some behavior conditions. As long as the behavior conditions are met (such as the division property above), these functions constitute a valid representation of the data type.```
һ���������ݣ�ѡ����������������Ϊ����������Ϊ����������ʱ���ܹ���ȷ��ִ����Ϊ��
