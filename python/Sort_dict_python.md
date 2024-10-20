 
---
title :  Sort Dictionaries in Python
author : Mark Nielsen  
copyright : October 2024  
---


Sort Dictionaries in Python
==============================

_**by Mark Nielsen
Original Copyright October 2024**_

Just wanted to record how to sort dictionaries in Python in various ways. 

1. [Links](#links)
2. [Sort](#sort)

* * *
<a name=Links></a>Links
-----
* https://docs.python.org/3/howto/sorting.html
* https://www.geeksforgeeks.org/python-sort-python-dictionaries-by-key-or-value/
* https://realpython.com/sort-python-dictionary/


* * *
<a name=sort>Sort</a>
-----

Make a dictionary and execute commands.
Remember in Python 3 dictionaries are ordered.
It can be treated like a list sort of. 

```
   # Create a simple dictionary "d"

print ("Make a dictionary. Keys have order.")
d = {"c":3, "d":1, "a":4, "b":2, }

print ("print keys in order in the dictionary.")
for key1 in d:  print (key1)

  # Create a new list which is iterable, meaning pulling the items
  # from the dictionary is order. 
d2 = dict(sorted(d.items(), key=lambda item: item[1]))

print ("")
print ("print dictionary keys sorted.")
for k in d2:  print (k)

print ("")
print ("print dictionary keys sorted with value.")
for k in d2:  print (k, d2[k])

print ("")
print ("print dictionary keys sorted with value but make a list of keys first.")
keys_temp = list(d2.keys())
for k in keys_temp:  print (k, d2[k])

print ("")
print ("print dictionary keys sorted with value with key list reversed.")
keys_temp2 = list(d2.keys())
keys_temp2.reverse()
for k in keys_temp2:  print (k, d2[k])


print ("")
print ("print dictionary keys sorted with value in reverse by creating reversed dictionary in one step.")
d3 = dict(sorted(d.items(), key=lambda item: item[1], reverse=True))
for k in d3:  print (k, d3[k])


print ("")
print ("print dictionary keys sorted by keys.")
d4 = dict(sorted(d.items()))
for k in d4:  print (k, d4[k])

print ("")
print ("print dictionary keys sorted by keys in reverse.")
d4 = dict(sorted(d.items(), reverse=True))
for k in d4:  print (k, d4[k])

print ("")
print ("print dictionary keys sorted with key with key list.")
keys_temp3 = list(d.keys())
keys_temp3.sort()
for k in keys_temp3:  print (k, d[k])

print ("")
print ("print dictionary keys sorted with key with key list reversed.")
keys_temp4 = list(d.keys())
keys_temp4.sort()
keys_temp4.reverse()
for k in keys_temp4:  print (k, d[k])



```

* Output
```

Make a dictionary. Keys have order.
print keys in order in the dictionary.
c
d
a
b

print dictionary keys sorted.
d
b
c
a

print dictionary keys sorted with value.
d 1
b 2
c 3
a 4

print dictionary keys sorted with value but make a list of keys first.
d 1
b 2
c 3
a 4

print dictionary keys sorted with value with key list reversed.
a 4
c 3
b 2
d 1

print dictionary keys sorted with value in reverse by creating reversed dictionary in one step.
a 4
c 3
b 2
d 1

print dictionary keys sorted by keys.
a 4
b 2
c 3
d 1

print dictionary keys sorted by keys in reverse.
d 1
c 3
b 2
a 4

print dictionary keys sorted with key with key list.
a 4
b 2
c 3
d 1

print dictionary keys sorted with key with key list reversed.
d 1
c 3
b 2
a 4


```