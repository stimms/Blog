layout: post
title: Linked lists in C#
authorId: simon_timms
date: 2018-12-10 9:00

---

One of the things you'll learn a lot about if you pursue any sort of formal education in computing science is data structures. However, once I hit the real world then it seemed like for most problems the speed differential between a linked list and a System.Collections.Generic.List wasn't meaningful. List quickly became my go-to and it was very unusual that any other data structure would even cross my mind. This sort of lazy thinking can have a serious effect as the size of input grows. So harkening back to my school days I wanted to talk about the rudimentary data structure that is a linked list.

<!-- more -->

Linked lists contain a series of nodes which wrap the data. 

![A singly linked list node, containing data and a link to the next node](/images/linked_lists/node.png)

A node contains the data for the list and a pointer to the next element in the list. This way any node that you have contains either a link to the next element in the list or a null for the pointer signifying that it is the end of the list.

The list itself contains a pointer to the head of the list. Finding any element in the list is as simple as getting the head from the list and following the pointer until you hit the node you want.

C# provides an implementation of linked list in System.Collections.Generic.LinkedList and the node for this list in System.Collections.Generic.LinkedListNode.

```csharp
var list = new LinkedList<string>();
var head = list.AddFirst("Camel");
list.AddAfter(head, "Dog");
```

Even though this list doesn't use a bunch of contiguous memory like a regular list LinkedList implements ICollection so you can largely treat is as a regular list.

The nice thing about the generic linked lists in C# is that they're actually doubly linked lists. The source code on [reference source](https://referencesource.microsoft.com/#System/compmod/system/collections/generic/linkedlist.cs,df5a6c7b6b60da4f) claims in a comment that its a circular list but I don't believe that is true. What's a doubly linked list? I'm glad you asked!

# Doubly linked list

Each node in a singly linked list has a pointer to the next node in the list. This permits rapid traversal in the forward direction only. In a single linked list getting the previous element is quite difficult and requires iterating through from the head making backwards traversal an O(n) operation.

![A double linked list node, containing data and a link to the next node both forward and backwards](/images/linked_lists/doublenode.png)

Keeping the second link to the previous element means that we can reduce the complexity of a backwards traversal to O(1). It also means it is easy to insert a node before the current one.

The LinkedList in C# is a doubly linked list.

# Usage Scenarios

Right off the top there are some scenarios which aren't great for a linked list. Random access is problematic because to get the 29th element you need to access 28 elements to see which node proceeds them. A collection like an array is great for that because you can jump to the memory offset right away. 

## Random Access

Let's look a what it is like looking up a random element in a LinkedList vs. an Array.

```
     Method |         Mean |       Error |      StdDev |
----------- |-------------:|------------:|------------:|
 LinkedList | 13,045.29 ns | 253.5930 ns | 249.0622 ns |
      Array |     13.88 ns |   0.2890 ns |   0.2704 ns |
```
**Note:** All the benchmarks in this article are done with Benchmarkdotnet and the code is on github at https://github.com/stimms/LinkedListBenchmarks

Wow, so we're talking 1 000x worse performance for a LinkedList in this case. So random access isn't the scenario we want for LinkedLists, Arrays are way better. 

## Insertion

Next let's try a couple of implementation of inserting 10 000 elements into a List. Our first uses the tail to do inserts.

```csharp
[Benchmark]
public void LinkedList()
{
    var list = new LinkedList<int>();
    foreach (var item in data)
    {
        list.AddLast(item);
    }
}
```

```
     Method |      Mean |     Error |    StdDev |
----------- |----------:|----------:|----------:|
 LinkedList | 281.23 us | 5.5022 us | 10.199 us |
       List |  39.14 us | 0.7815 us |  2.017 us |
```

Still quite a bit slower than using a list. Another approach is to keep a handle to the current node around

```csharp
[Benchmark]
public void LinkedList()
{
    var list = new LinkedList<int>();
    LinkedListNode<int> currentNode = new LinkedListNode<int>(data[0]);
    list.AddFirst(currentNode);
    foreach (var item in data.Skip(1))
    {
        currentNode = list.AddAfter(currentNode, item);
    }
}
```

This actually came out worse because added a bunch of overhead assignments and a skip

```
     Method |      Mean |     Error |    StdDev |
----------- |----------:|----------:|----------:|
 LinkedList | 426.95 us | 8.5209 us | 11.375 us |
       List |  38.02 us | 0.7380 us |  1.127 us |
```

So if all you need to do is insert elements into a collection List is a better option. 

## Removing elements

Let's try removing every other elements from a List and a LinkedList. 

```csharp
[Benchmark]
public void LinkedList()
{
    var currentNode = linkedlist_data.First;
    for (int i = 0; i < 10_000 && currentNode != null && currentNode.Next != null; i++)
    {
        if (i % 2 == 0)
        {
            linkedlist_data.Remove(currentNode.Next);
        }
        currentNode = currentNode.Next;
    }
    var result = currentNode.Value;
}

[Benchmark]
public void List()
{
    var result = list_data.Where((item, index) => index % 2 == 0);
}
```

```
     Method |      Mean |     Error |    StdDev |
----------- |----------:|----------:|----------:|
 LinkedList |  1.813 ns | 0.0625 ns | 0.0585 ns |
       List | 25.586 ns | 0.5461 ns | 0.8340 ns |
```

The resulting speedup here is really pretty impressive, 12x for linked lists over regular lists. 


## Conclusion

List are a pretty good general purpose data structure but there are cases where just throwing a list at the problem isn't going to give you a good solution. I was working on [Advent of Code](https://adventofcode.com/2018) this year and there was a problem where solving it using a List took on the order of 2 hours on my machine and using a linked list it dropped to less than 2 seconds. The problem in question dealt with a lot of short hops inside a list (forward 1, back 7, remove 1, forward 1, add 1) which made it ideal for a linked list. The constant copying around for a List added a very significant amount of overhead. 