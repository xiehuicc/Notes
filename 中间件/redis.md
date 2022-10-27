### 1.  redis 5 中数据结构

1. **String**  (simple Dynamic String 简称 sds)

   **应用场景**

   储存key-value 键值对

2. **list** （列表）

   > 底层是链表结构，list的插入和删除操作非常块，时间复杂度为O(1)，不像数组结构插入、删除操作需要移动数据。

   **应用场景**

   - 消息队列： lpop 和 rpush (或者反过来，lpush和rpop)能实现队列的功能；
   - 朋友圈的点赞列表、评论列表、排行榜：lpush命令和lrange命令能实现最新列表的功能，每次通过lpush命令往列表插入新的元素，然后通过lrange命令读取最新的元素列表。