---
title: Redis Mass Insert Function In Python
layout: post
---

One of the fastest and easiest approaches to adding large amounts of data to your Redis instance is by doing the following

For each data item:

  * Format your data insertion operation as shown below

        'SET', 'KEY0', 'VALUE0'

        'INCR', 'SOME_KEY'

  * Encode the above using Redis Protocol discussed [here](http://redis.io/topics/mass-insert). Write it to a text file.

  * Run the following command in your terminal against your redis instance

        > cat my_data_file.txt | redis-cli --pipe

While the example shown [here](http://redis.io/topics/mass-insert) is in Ruby. Below, I offer an equivalent code snippet in Python.

```python

import sys


def gen_redis_proto(*args):
    proto = ""
    proto += "*"+str(args.__len__())+"\r\n"
    for arg in args:
        proto += "$"+str(str(arg).__len__())+"\r\n"
        proto += str(arg)+"\r\n"
    return proto


def generate_data_file():
    f = open('redis_data.txt', 'w')
    [f.write(gen_redis_proto("SET", "KEY{0}".format(x), 
                             "VALUE{0}".format(x))) 
     for x in range(0, 4000000)]

generate_data_file()

```