
# Intro to bloom filters
Other than the common, standard data structures we've discussed so far in lectures, which includes `<Linked Lists>`, `<Trees>`, `<Hash Table>`, and `<Graphs>`. **Bloom Filters** are esoteric data structures that possess interesting properties. Named after its creator: **Burton Howard Bloom** in 1970, lets first discuss the basic features of **Bloom Filters**.
- 100% accurate for showing if an element is not the collection, but can't be sure if an element is in the collection. For example, the algorithm will yield two possible results:
  - _Element "A" is not in the set._
  - _Element "B" might be in the set._

# How it Works
- For memory saving, we use a bit array for a single collection. Consider a bit array containing eight bits (one byte) of memory:

  > {0|0|0|0|0|0|0|0}

- Notice that we always want to initialize the array with zeros, which represents an empty list.

- We can even expand this array even further like a four-byte array that contains 32 bits, instead of just eight.


# Basic operations
There are two simple task that bloom filters can do: **Test** and **Add**.
- ### Add

  Now if we were to add numbers to this _bit array_ we first need to apply hashing functions to the number we are about to add. The functions are:

      k1 = (13 - (x % 13)) % 8

      k2 = (3 + 5x) % 8

      etc.


  We could have many hashing functions, here we just have two. In this case, we would need to change the number at index 3 and 6 into "1" from zero if we apply 119 to both hashing functions. Similarly, we get 3 and 7 when 132 is applied to the functions. Notice that at index 3 the element is already "1" so we don't have to change it anymore.

  ![](./imgs/bloomExample.jpg)

  It is nice to mention that it would be nearly impossible to execute "remove" on a bloom filter. Like the example shown above. If we were to remove 132 from the filter, we need to remove both 3 and 7 in theory. However, removing 3 is damaging other information as we no longer have 119 in the filter.


# Why Bloom Filters?
A **Bloom Filter** is unlike any collection set we've seen so far. We don't actually need to store the values themselves into the collection.
- The size of a single **Bloom Filter** is going to be fixed from the moment it's been created. In the example above, it will forever be an 8-bit array that consumes 1 byte of memory. In real practice, we'll need a larger array.
- It always takes **O(1)** to "add" an element to this array, which includes applying the hashing functions and changing the corresponding indexed position from 0 to 1.
- Aside from being memory efficient. With the help of **Bloom Filters**, our algorithms could work much faster. Soon as we find any hash code to be zero, we are sure that number is not in the collection and gets "filtered out" by the **Bloom Filter** In the example above, checking number 143 will get us 5 and 6 from applying the hashing functions. Since at index 5, the element is 0, we can be sure that 5 isn't in the collection. In the case of negative feedback, we don't have to access the disk, and this saves a lot of time.


For searching an element, what we've learned in classes may lead us to have best case of runtime **O(log(N))**. But when bloom filters are attached, we don't have to apply the searching algorithms every time if there's an efficient way to rule out the cases when the prompt number is absent in the collection.

In a real computer, we all have the searching box for "this computer". For searching, it must the ram could be a place where the filter could be placed for quick access. As the cpu interact with the ram, the ram may bring negative or false positive feedbacks and then the cpu decide to access the disk or not. This could potentially save lots of time for execution, even power used could be reduced.

Hashing is extremely useful, not just in bloom filters. Recently I am building a new desktop, as I was downloading the Windows 10 system installing package from the store. I was suggested to use the app "iHasher" to check whether if the files I've downloaded is damaged. The downloading link provides me a hash code for reference, after downloading the file I can use the "iHasher" to compute the hash code for my copy and compare that to the one from the downloading link. The whole package is around 2 gigabyte, and it takes a line of hash code with less than 50 characters to "describe" it. That's way more efficient than comparing the whole package.
