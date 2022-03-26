

## 一、前言
<br>

&emsp;&emsp;前两篇给大家介绍关于时间复杂度和空间复杂度的推导逻辑，我们也通过解决"两数求和"问题正是踏入了算法界的修行。正所谓，一山更比一山难，这次我们遇到的Boss不简单，想要通关就要转动你的小脑筋，时刻留心Boss的"陷阱"。

<br>

## 二、题目介绍
<br>

&emsp;&emsp;boss特点：给你两个非空的链表，表示两个非负的整数。它们每位数字都是按照逆序的方式存储的，并且每个节点只能存储 一位数字。
<br>

&emsp;&emsp;通关要求：请你将两个数相加，并以相同形式返回一个表示和的链表。
<br>

&emsp;&emsp;通关提示：你可以假设除了数字 0 之外，这两个数都不会以 0 开头。
<br>

&emsp;&emsp;通关示例：
<br>

`
示例1：
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807
`

`
示例2：
输入：l1 = [0], l2 = [0]
输出：[0]
`

示例3：
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
`

<br>

![image-20211019212656251](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326175845.png)

<br>

&emsp;&emsp;通关提醒：
<br>

- 每个链表中的节点数在范围 [1, 100] 内
- 0 <= Node.val <= 9
- 题目数据保证列表表示的数字不含前导零

<br>

## 三、题目解读
<br>

&emsp;&emsp;打蛇打七寸，要解决这个Boss，那就要先找到它的弱点，通过Boss描述，我们会发现它存在以下特点：
<br>

- 链表中的元素都是逆序的，如数值123，则存储在链表中的为：3 - 2 -1

- 链表中每个元素只能存储一位数，如果元素值相加超过10时，Boss会进化，将值进位给前面的数值，依次类推。如上图中两个链表第二位元素值分别为4、6，它们相加后会得到10，此时Boss进化，保留0，并向前进位1，所以第三位元素相加则为：3+4+1 = 8。


<br>

## 四、通关方式一：人海战术
<br>

&emsp;&emsp;掌握了Boss的特点，我们能想到的第一个办法就是人海战术，技术Boss再强大，我们也能通过大量的人数来慢慢磨去他的血量，最终，将其击败，下面来看看具体方案：
<br>

```
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // 链表头元素
        ListNode head = null;
        // 链表最后一个元素
        ListNode tail = null;
        int carry = 0;
        // 如果链表还存在值，则进行两数相加(只要Boss还存活，复活后继续砍)
        while (l1 != null || l2 != null) {
            int l1Val = l1 == null ? 0 : l1.val;
            int l2Val = l2 == null ? 0 : l2.val;
            // 获取链表对应元素之和，如果超过10则进位
            int sum = l1Val + l2Val + carry;
            // 判断是否需要进位
            carry = sum / 10;
            // 设置链表头元素
            if (head == null) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail.next = new ListNode(sum % 10);
                // 将tail永远指向链表的最后一个元素，用于进行下一轮操作
                tail = tail.next;
            }
            // 如果链表还存在值，则进行将元素指向下一个，用于剩下计算
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        // 如果最后一个元素需要进位，则将则tail指向进位的数值
        if (carry > 0) {
            tail.next = new ListNode(carry);
        }
        return head;
    }
```
<br>

&emsp;&emsp;果不其然，纵使Boss再强，也耐不过人海战术的无赖，最终Boss还是倒在了强大的玩家脚下。但是，此时又有人抱怨了，通关这个Boss需要花费这么多玩家，玩家人数不够时，能否有其他的方案，只需要少数玩家就可以通关的？
<br>

&emsp;&emsp;这时候，足智多谋的小诚想到了一个方案，我们可以借鉴车轮战的思想，将指定人数划分为不同的攻关小分队，不断的去跟Boss磨血量，这样最终也可以将Boss通关，大家也都赞成小诚的想法，下面具体看看如何实现吧。

<br>

## 五、通关方式二：车轮战(递归思想)
<br>

```
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // 获取链表中的值
        int l1Val = l1 == null ? 0 : l1.val;
        int l2Val = l2 == null ? 0 : l2.val;
        // 获取链表对应元素之和，如果超过10则进位
        int sum = l1Val + l2Val;
        ListNode node = new ListNode(sum % 10);
        int carry = sum / 10;
        if (l1.next != null || l2.next != null || carry > 0) {
            l1 = l1.next != null ? l1.next : new ListNode(0);
            //这里的new ListCode(0)是因为此时的l1已经完了，l2还没有完，还得用l2的数和l1相加，此时就一直用l1指向一个值为0的节点就可以了。
            l2 = l2.next != null ? l2.next : new ListNode(0);
            l1.val = l1.val + carry;
            // 采用递归思想
            node.next = addTwoNumbers(l1, l2);
        }
        return node;
    }
```

<br>

![image-20211019230949109](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326184805.png)

## 六、总结经验

<br>

&emsp;&emsp;上面的两种方案我们都可以解决这个Boss，那它们之间的时间复杂度会存在什么差别？下面来一起推导它们的时间复杂度吧！
<br>

### 时间复杂度的推导
<br>

&emsp;&emsp;通过上面两个方案的代码，我们会发现随着链表元素的增加(问题规模的变大)，我们需要循环或者递归的次数也会增多，它们的关系是f(n) = n，通过大O计法的推导方式，我们可以得出这两种方案的时间复杂度为：O(max(m,n))。
<br>

&emsp;&emsp;说明：max(m,n)表示的是取这两个链表中长度最长的值为输入规模。

<br>

### 算法思想在实际业务中的运用
<br>

&emsp;&emsp;上面的案例中，第一种方案用到了最常见的while循环，这个思想在平常的业务中体现最多的就是数组或者链表键元素的比较，排序(如：冒泡、选择排序等)。
<br>

&emsp;&emsp;第二种案例我们采用了递归的思想解决问题，在平常的业务中，这个思想最常见的用途就是查询系统的菜单功能，菜单是层层嵌套的，正好符合了递归的思想。


<br>

## 七、写在最后
<br>

&emsp;&emsp;问题可以是有穷的，但是解决问题的办法确实无穷的，刷leetcode的目的不仅仅是为了应付面试，更多的是为了培养我们遇到问题、解决问题的思考方式。
<br>

