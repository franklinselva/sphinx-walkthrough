# functions-built-in

## iter

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=../../python/functions-built-in/iter.py) -->
<!-- The below code snippet is automatically added from ../../python/functions-built-in/iter.py -->
```py
#!/usr/bin/env python3
class ObjectList:

	def __init__(self):
		self.index=0

	def __iter__(self):
		return self

	def __next__(self):
		self.index+=1
		if self.index>5:
			raise StopIteration()
		else:
			return self.index

print(">>> for <<<")
for i in ObjectList():
	print(i)

print(">>> for with iter object <<<")
for i in iter(ObjectList()):
	print(i)

print(">>> for with iter object with limit <<<")
for i in iter(ObjectList().__next__,3):
	print(i)


# ---------------------------------------------

class Iterable:

	def __init__(self):
		self.value = 0

	def next(self):
		self.value+=1
		return self.value


print(">>> for with iter object with limit <<<")
for i in iter(Iterable().next,3):
	print(i)

# ----------------------------------------------

print("execute 'next' function over 'ObjectList'")
values = ObjectList()
print (next(values))
print (next(values))
print (next(values))
```
<!-- MARKDOWN-AUTO-DOCS:END -->



## iterable

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=../../python/functions-built-in/iterable.py) -->
<!-- The below code snippet is automatically added from ../../python/functions-built-in/iterable.py -->
```py
#!/usr/bin/env python3
print(">>> all <<<") 
# ---

# True 
print(all([True,True,True]))

# True 
print(all([1,2,3]))

# True 
print(all(["one", "two", "three"]))

# False 
print(all(["one", None, "two", "three"]))

# False 
print(all([1,0,2,3]))



print(">>> any <<<") 
# ---

# True 
print(any([True,False,False]))

# True 
print(any([None,None,3]))

# True 
print(any(["one", "two", "three"]))

# False 
print(all([None, None, None, ]))

# False 
print(any([0,0,0,0]))


print(">>> enumerate <<<")
# ---
print(list(enumerate([100,101,102,103,104,105,106,107], start = 3 )))


print(">>> filter <<<") 
# ---
print(list(filter(lambda x: x>3, [1,2,3,4,5,6,7,8,9,10])))


print(">>> forzenset <<<")
# ---
fs = frozenset([1,1,1,3,3,3,4,5,6,7,7,7])
print(fs)
# method not exists: fs.remove(3)

print(">>> max, min, sum <<<")
# ---
print( max([1,1,1,3,3,3,4,5,6,7,7,7]) )
print( min([1,1,1,3,3,3,4,5,6,7,7,7]) )
print( sum([1,1,1,3,3,3,4,5,6,7,7,7]) )


print(">>> range <<<")
print(list(range(5,10,2)))


print(">>> reversed list: 1,2,3,4,5 <<<")
print(list(reversed([1,2,3,4,5])))


print(">>> sorted list: 5,3,4,2,1 <<<")
print(list(sorted([5,3,4,2,1])))
class Custom(object):
    def __init__(self, name, number):
        self.name = name
        self.number = number
 
    def __cmp__(self, other):
        if hasattr(other, 'number'):
            return self.number.__cmp__(other.number)

roll_list1 = [('Jack', 76), ('Beneth', 78), ('Cirus', 77), ('Faiz', 79)]; roll_list1.sort()
# [(‘Beneth’, 78), (‘Cirus’, 77), (‘Faiz’, 79), (‘Jack’, 76)


print(">>> slice, sub-list <<<")
values = range(1,10)
object_slice = slice(2,5)
print(list(values[object_slice]))


print(">>> zip <<<")
a = [1,2,3]
b = [4,5,6,7,8]
zipped = zip(a,b)
print(list(  zipped ))
```
<!-- MARKDOWN-AUTO-DOCS:END -->


