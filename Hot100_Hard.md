### [25. K个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/description/?envType=study-plan-v2&envId=top-100-liked)

#### 题目描述
给你链表的头节点 head ，每 k 个节点一组进行翻转，请你返回修改后的链表。

k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换
示例：

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

#### 核心思路
```
核心思路可以总结为 “先数长度定组数，再逐组反转并精准拼接”，整个过程是迭代进行的。

具体分为三个核心环节：

1、长度预处理（定边界）：先遍历一次链表统计总节点数 n。这样就能通过 while (n >= k) 精确控制循环次数，确保末尾不足 k 个的节点保持原样不动，避免复杂的边界判断。

2、组内反转（断链与转向）：每一轮循环中，利用 pre 和 cur 指针，对当前组内的 k 个节点执行标准的链表反转操作（将 cur.next 指向前驱 pre）。循环结束后，pre 成为该组反转后的新头节点，cur 则指向下一组的第一个节点。

3、组间衔接（接回原链）：这是保证链表不断裂的关键。利用 p0 指针始终指向当前组的前一个节点（上一组的尾结点）：

（1）先找到原组的旧头节点 nxt（即 p0.next），反转后它变成了本组的新尾节点，将其 next 指向 cur（接上后面未反转的部分）。

（2）再将 p0.next 指向 pre（接上反转后的新头节点）。

（3）最后将 p0 移动到 nxt（即本组的尾结点），作为下一组处理时的前驱节点。
```

#### 代码
```python
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        # 统计节点个数
        n = 0
        cur = head
        while cur:
            n += 1
            cur = cur.next
        #p0 始终指向上一组的最后一个节点（也是下一组的前驱）
        p0 = dummy = ListNode(next=head) 
        pre = None
        cur = head

        # k 个一组处理
        while n >= k:
            n -= k
            for _ in range(k):  # 同 92 题
                nxt = cur.next
                cur.next = pre  # 每次循环只修改一个 next，方便大家理解
                pre = cur
                cur = nxt

            # 将反转后的子链表接回原链表
            nxt = p0.next
            nxt.next = cur
            p0.next = pre
            p0 = nxt
        return dummy.next


```
