---
layout: post
title: "Fenwick Trees"
categories: misc
---

Fenwick trees are used to solve a rather simple problem, given some array, for example $$[1 ,4 , 6, 2, 4]$$ and two indicies i,j 
find the sum of elements between and including the indices. A straightforward way to solve problems like this is just brute force which takes O(n)
time. However, if we modify the problem more, by querying multiple different sums, we might want to create an array of sums to keep track,
for our example our sum array would be $$[1,5,11,13,17]$$. This also takes O(n) time. We can get the sum of elements from i to j by querying sum[j]-sum[i].
Now, for the final twist, imagine if the original array could change by adding 5 to some element for example. Then, it would take O(n)
to recalculate our sum array. Our goal is to have O(log n) update time, while also having O(log n) sum time. To do this, we use a fenwick tree or a Binary Indexed tree.



  Fenwick trees use binary to quickly navigate the tree to perform update and sum operations. Given an array of index n, our fenwick tree will be of size n+1.
The first element will always be 0, by convention. We assign values to indexes in the following way: Decompose it into powers of 2, with higher powers of 2
coming first. Then, the smallest power of 2 is the number of elements we sum, and the sum of all the other powers of 2 is the index we start summing at.

  For example, if we had the index 2 = 0 + $$2^1$$, we would start at the 0th index, and add the first two elements. Note that for any perfect power,
it sums all the elements up to that power's index. If we had something like $$ 7 = 2^2 + 2^1 + 2^0$$, we would have $$2^2+2^1 = 6$$ for our starting index, and sum the only element there.


  Now, suprisingly, we have an algorithm for summing from 0 to some index, say i with a fenwick tree. We start at the i+1 index in our fenwick tree, and we do the following
iterative process shown below in code.

```python
index = i+1
sum = 0 
while index != 0 :
  sum += fenwick[index]
  index -= index & (-index)
```
What this code does is, it starts at an index and does binary operations to go to its parent. If we take the example of 7, which is the sum of the 6'th element,
the parent would be 6 = $$2^2 + 2^1$$ which sums the fourth and fifth element, and the parent of 6 is $$4 = 0+ 2^2$$ which is the sum of the first four elements, giving us the sum of the first 6th elements.
This process works because of the special way we define elements in the fenwick tree. This process takes at worst O(log n) time, because we are
subtracting a non-zero binary number every time.
 
 For updating, we do a similar thing. Say, we want to add 5 to the 2'nd element. To update our fenwick tree, we simply add instead of subtracting
 our binary operation, i.e
 
```python
index = 2+1

while index <= length(fenwick):
  fenwick[index] += 5
  index += index & (-index)
 ```
 This is all there is to a fenwick tree, and it is relatively simple to implement because of binary operations being simple to express in python.
 Below is my own implementation:
 
 ```python
 class Fenwick():
    def __init__(self,arr):
        self.tree = [0 for x in range(len(arr) + 1)]
        for i in range(len(arr)):
            self.update(i+1,arr[i])
            



    def update(self,index,value):
        while index < len(self.tree):
          self.tree[index] += value
          index += index & (-index)

        pass

    def sum(self,index):
        total = 0
        index = index + 1
        while index != 0 :
          total += self.tree[index]
          index -= index & (-index)
        return total




          
test = Fenwick([3,2,-1,6,5,4,-3,3,7,2,3])
print(test.tree)
print(test.sum(10))
```
 
