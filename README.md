### dict
[美团点评](https://tech.meituan.com/2018/07/27/redis-rehash-practice-optimization.html)

### ziplist
其实是一个超长的字符串

分为
header  entry entry entry   end（255 特殊标识位）

entry会保存1.当前的编码2.当前entry的字符串，3.前一个entry的长度，以便通过指针移动，到达前一个entry,
4.保存当前entry的长度，效果同上


字符串移动 指针如何实现，看下一个



```text
 * The general layout of the ziplist is as follows:
 * <zlbytes><zltail><zllen><entry><entry><zlend>
 *
 * <zlbytes> is an unsigned integer to hold the number of bytes that the
 * ziplist occupies. This value needs to be stored to be able to resize the
 * entire structure without the need to traverse it first.
 * // 保存ziplist所占字节数
 *
 * <zltail> is the offset to the last entry in the list. This allows a pop
 * operation on the far side of the list without the need for full traversal.
 * // <zltail>是列表中最后一个条目的偏移量。这允许在列表的另一端进行弹出操作，而无需完全遍历。
 *
 * <zllen> is the number of entries.When this value is larger than 2**16-2,
 * we need to traverse the entire list to know how many items it holds.
 * // entry的数量，如果超过 2**16-2.我们需要才能知道数目
 * <zlend> is a single byte special value, equal to 255, which indicates the
 * end of the list.
 * // 单字节特殊数组，等于255，表示列表的末尾
```


### c语言中字符串移动
可以通过指针的移动举例
```c
    char *pointer = "https://www.baidu.com";
    pointer+=2;
    printf("%s",pointer);
```
输出是tps://www.baidu.com