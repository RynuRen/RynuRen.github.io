---
layout: post
title:  Pandas, Numpy
published: false
date:   2022-11-10
description: 
toc: true
comments: true
tags:
 - python
---
---
# 6. Package and Module
## 



Differences between numpy.random and random.random in Python
https://stackoverflow.com/questions/7029993/differences-between-numpy-random-and-random-random-in-python

For , the main difficulty is that it is not thread-safe - that is, it's not safe to use if you have many different threads of execution, because it's not guaranteed to work if two different threads are executing the function at the same time. If you're not using threads, and if you can reasonably expect that you won't need to rewrite your program this way in the future, should be fine. If there's any reason to suspect that you may need threads in the future, it's much safer in the long run to do as suggested, and to make a local instance of the numpy.random.Random class. As far as I can tell, is thread-safe (or at least, I haven't found any evidence to the contrary)
numpy.random.seed()
numpy.random.seed()
random.random.seed()

The library contains a few extra probability distributions commonly used in scientific research, as well as a couple of convenience functions for generating arrays of random data. The library is a little more lightweight, and should be fine if you're not doing scientific research or other kinds of work in statistics.
numpy.random
random.random

Performance difference between numpy.random and random.random in Python
https://stackoverflow.com/questions/57220804/performance-difference-between-numpy-random-and-random-random-in-python

Answer depends of the needs :
- Cryptography / security : secrets
- Scientific Research : numpy
- Common Use : random



2차원 np array 마스크 적용
https://stackoverflow.com/questions/38193958/how-to-properly-mask-a-numpy-2d-array


{% highlight python linenos %}
#
{% endhighlight python %}

---
# Reference
SHIN JINHYO : [Jupyter Notebook 테마 변경하는 방법](https://realblack0.github.io/2020/05/13/jupyter-notebook-themes.html)
{: style="text-align: right"}