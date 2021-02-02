---
title:  Linus：使用二级指针删除单向链表的某个节点
author: xjliang
date: 2021-02-02 11:20:10 +0800
categories: [Technology]
tags: [c, pointer, list]
---

2012年，Linus Torvalds 在 slashdot [回答了一些问题](https://meta.slashdot.org/story/12/10/11/0030249/linus-torvalds-answers-your-questions)。在众多问题中，令我感兴趣的一个是 **favorite hack**。他的回答最后如下：

> At the opposite end of the spectrum, I actually wish more people understood the really core low-level kind of coding. Not big, complex stuff like the lockless name lookup, but simply good use of pointers-to-pointers etc. For example, I've seen too many people who delete a singly-linked list entry by keeping track of the "prev" entry, and then to delete the entry, doing something like
>
> if (prev)
> prev->next = entry->next;
> else
> list_head = entry->next;
>
> and whenever I see code like that, I just go "This person doesn't understand pointers". And it's sadly quite common.
>
> People who understand pointers just use a "pointer to the entry pointer", and initialize that with the address of the list_head. And then as they traverse the list, they can remove the entry without using any conditionals, by just doing a "*pp = entry->next".
>
> So there's lots of pride in doing the small details right. It may not be big and important code, but I do like seeing code where people really thought about the details, and clearly also were thinking about the compiler being able to generate efficient code (rather than hoping that the compiler is so smart that it can make efficient code *despite* the state of the original source code).

大意就是说，他希望更多人使用更接近底层的方式进行编程（在我看来应该是 C 语言和 C++）。他举了一个例子：删除单向链表中的某个节点。说到这个知识点，大家在学校或者平常工作中写的应该类似这样：

```c
if (prev)
	prev->next = entry->next;
else
	list_head = entry->next;
```

Linus 提出，写出这段代码的人并不懂得使用指针。真正懂指针的人会使用 **指向节点指针的指针** 来完成这项工作。他希望写代码时能够考虑更多细节，更加关注编译器如何高效生成代码。

我认为我理解指针，但是，如果让我实现一个删除链表节点的方法，我也会使用一个 **prev** 指针来维护当前节点的前一个节点。下面是代码的核心部分：

```c
typedef struct node {
    struct node* next;
    ...
} node;

typedef bool (* remove_fn)(const node * v);

// Removes all nodes from the supplied list for which the 
// supplied remove function returns true.
// Returns the new head of the list.
node* remove_if(node* head, remove_fn rm) {
    for (node *prev = NULL, *curr = head; curr != NULL; ) {
        const node* next = curr->next;
        if (rm(curr)) {
            if (prev) {
                prev->next = next;
            } else {
                head = next;
            }
            free(curr);
        }
        curr = next;
    }
    return head;
}
```

上面这段代码的微妙之处在于处理从列表头删除的任何节点所需的条件。

下面看看 Linus 的思路：

```c
void remove_if(node** head, remove_fn rm) {
    for (node** curr = head; *curr; ) {
        node* entry = *curr; // #1
        if (rm(entry)) {
            *curr = entry->next; // #2
            free(entry);
        } else {
            curr = &entry->next; // #3
        }
    }
}
```

和上面一段代码相比，可以发现：**我们不再需要 prev 指针了，也不需要判断是不是为链表头了**，但是，curr 变成了一个**二级指针**。这正是这段程序的精髓。（注意上面标注的三个地方）

下面分析下这个代码：

- 如果待删除节点**是头节点**，在 for 循环初始化 curr 为 head，entry 就是 \*head（\*curr），可以直接删除它，\#2  处等价于 `*head = *head->next`，这符合删除头节点的要求；
- 如果待删除节点**不是头节点**，对于上面的代码，分两种情况看：
  1. （\#3 处 ）若**不删除**当前节点，curr 保存的就是当前节点 next 指针的地址；
  2. （\#1 处 ）entry 保存了 \*curr，也就是说，在**下一次循环**，entry 就是 **prev->next**；
  3. （\#2 处 ）**删除**节点，`*curr = entry->next;`这样，`prev->next = entry->next; `的逻辑得以实现。

完美！上面的关键在于链表中连接各个节点的是指针，因此通过指针来修改链表是最优雅高效的。

---

参考：

1. [Two star programming (wordaligned.org)](http://wordaligned.org/articles/two-star-programming)
2. [Linus：利用二级指针删除单向链表 | 酷 壳 - CoolShell](https://coolshell.cn/articles/8990.html)