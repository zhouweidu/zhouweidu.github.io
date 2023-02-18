---
title: LeetCode 刷题笔记
date: 2022-09-06 19:10:59
tags:
- LeetCode
categories:
- LeetCode
typora-root-url: LeetCode-刷题笔记
---

注意：一道题目**先思考20-30分钟**如果实在做不出来再看看题解和思路，然后自己试试能不能自己写出代码，题目后面是在LeetCode网站上的题号，前面那个是自动生成的有序列表。

- 2022/4/24 基本上一天四道题

1. 两数之和(1)：太简单没什么说的。

2. 两数相加(2)：应该根据题目现有的条件来做，从题目本身出发，不要一开始就考虑转换为常用的按顺序的十进制加法，**类比二进制加法计算（二进制全加法器）**，直接用两个链表的结点从前往后相加即可。

3. 无重复字符的最长子串(3)：

   用**滑动窗口**思想，两个指针left和right，如果left和right之间没有重复就让right++，因为之前left和right之间无重复，所有如果right++后出现重复一定是现在的right和之前的left到right之间有一个值重复了，以```string.charAt(right)```为标准从left开始遍历，分三种情况（前部、中部、后部）

   （1）a b c a

   （2）a b c b

   （3）a b c c

   从j=left开始遍历（j++），如果遇到这两种重复的情况，让left=j+1即左指针指向重复位置的下一个位置。

4. 寻找两个正序数组的中位数(4)：

   归并排序，将**两个有序数组合并为一个**，for循环遍历合并后的数组，然后分别设置两个指针指向需要排序的两个数组，取两个数组中小的数放入新数组，并且让该数组指针后移，**边界情况**若一个数组指针已经指向末尾，则另外判断次情况，将另一个数组的数全部放入即可。

   算法时间复杂度为O（m+n），只用遍历一次新数组即可。

   <!--more-->

5. 最长回文子串(5)：

   （1）扩展中心：回文子串分两种，奇数长度：abcba可以从中间的a开始向两边扩展，偶数长度：abcabbabb，从[1]和[2]中间开始向两边扩展，下面是扩展的函数

   ```java
   public int expand(String s,int start,int end){
   	while(start>=0&&end<s.length()&&s.charAt(start)==s.charAt(end)){
               start--;
               end++;
           }
      	return end-start-1;//注意这里的减一，比如abcba，到两边a的时候还会start--和end++,所以要减一
   }
   ```

   遍历字符串，从每个点开始扩展，有两种扩展方式，分别为`expand(s,i,i);//abcba`（从c开始扩展）和`len2=expand(s,i,i+1);//abba`（从[1]b和[2]b开始扩展），然后找出最长的那个扩展方式，`start=i-(len-1)/2;end=i+len/2;`,注意这里的start和end。

   （2）动态规划：p（i，j）=p(i+1,j-1)&&s[i]==s[j]所以如果我们想知道 P（i , j）的情况，只需要知道 P（i + 1，j - 1）的情况就可以了，然后向两边扩展即可，这样时间复杂度就少了 O(n)。因此我们可以用动态规划的方法，空间换时间，把已经求出的 P（i，j）存储起来，求 长度为 1 和长度为 2 的 P(i,j) 时不能用上边的公式，例如求p（1，2），带入会出现p（2，1），长度为1和2的回文串需要单独判断。

   ```java
   for (int len = 1; len <= length; len++) //遍历所有的长度
           for (int start = 0; start < length; start++) {
               int end = start + len - 1;
               if (end >= length) //下标已经越界，结束本次循环
                   break;
               P[start][end] = (len == 1 || len == 2 || P[start + 1][end - 1]) && s.charAt(start) == s.charAt(end); //长度为 1 和 2 的单独判断下
               if (P[start][end] && len > maxLen) {
                   maxLen=len;
                   maxPal = s.substring(start, end + 1);
               }
           }
   ```

   时间复杂度：两层循环 O(n²）

   空间复杂度：用二维数组 P*P* 保存每个子串的情况 O(n²)

6. Z字形变换(6)：是一个找规律的题目，可以找出规律来用二维数组模拟

   ![image-20220328225949259](image-20220328225949259.png)

7. 正则表达式匹配(10)：

   给你一个字符串 `s` 和一个字符规律 `p`，请你来实现一个支持 `'.'` 和 `'*'` 的正则表达式匹配。

   - `'.'` 匹配任意单个字符
   - `'*'` 匹配零个或多个前面的那一个元素

   使用动态规划，对匹配的方案进行枚举。我们用 f [i] [j] 表示 s 的前 i 个字符与 p 中的前 j 个字符是否能够匹配.

   如果 p 的第 j 个字符是一个小写字母，那么我们必须在 s中匹配一个相同的小写字母，即

![image-20220407223143316](1-166246313667549.png)

![image-20220407223216513](2.png)

如果我们通过这种方法进行转移，那么我们就需要枚举这个组合到底匹配了 s 中的几个字符，会增导致时间复杂度增加，并且代码编写起来十分麻烦。我们不妨换个角度考虑这个问题：字母 + 星号的组合在匹配的过程中，本质上只会有两种情况：

+ 匹配 s 末尾的一个字符，将该字符扔掉，而该组合还可以继续进行匹配；

+ 不匹配字符，将该组合扔掉，不再进行匹配。

如果按照这个角度进行思考，我们可以写出很精巧的状态转移方程：

![image-20220407223308404](3.png)



![image-20220407223334274](4.png)

其中 matches(x,y) 判断两个字符是否匹配的辅助函数。只有当 y 是 . 或者 x 和 y 本身相同时，这两个字符才会匹配。

8. 盛最多水的容器(11)：

  **双指针**解决，第一次做这题可能不会想到双指针。下面解释双指针算法的过程，并证明其正确性。给定一个数组height[1, 8, 6, 2, 5, 4, 8, 3, 7]，初始时分别有一左一右两个指针位于边界，容纳的水量=两个指针指向的数字中较小值∗指针之间的距离，假设左指针为left右指针为right，不妨设此时height[left]<height[right]，如果移动右指针，那么两个指针的距离一定变小，并且min{height[left],height[right']}<=之前的高度，因为如果此时新的高度比原来的高度大，取height[left]和height[right]的最小值还是之前的height[left]，高度没变但距离变小，所以容器的容积变小，如果新的高度比原来的高度小，则导致高度变小且距离变小，容积也会变小，即无论我们怎么移动右指针，得到的容器的容量都小于移动前容器的容量，也就是说，这个左指针对应的数不会作为容器的边界了，那么我们就可以丢弃这个位置，将左指针向右移动一个位置，此时新的左指针于原先的右指针之间的左右位置，才可能会作为容器的边界。综上，可以得出每次移动指针的时候移动高度较小的那个指针是正确答案，边界条件是left<right。

9. 整数转罗马数字(12)：

   按照题目所给规则可能出现的罗马数字如下

   ![image-20220410224312579](image-20220410224312579.png)

   ![image-20220410225427004](image-20220410225427004.png)

   根据上图求140的罗马数字，我们用来确定罗马数字的规则是：对于罗马数字从左到右的每一位，选择尽可能大的符号值。对于 140，最大可以选择的符号值为C=100。接下来，对于剩余的数字 40，最大可以选择的符号值为 XL=40。因此，140 的对应的罗马数字为 C+XL=CXL。

   采取模拟的思路，根据罗马数字的唯一表示法，为了表示一个给定的整数 num，我们寻找不超过 num 的最大符号值，将 num 减去该符号值，然后继续寻找不超过 num 的最大符号值，将该符号拼接在上一个找到的符号之后，循环直至 num 为 0。最后得到的字符串即为 num 的罗马数字表示。

   编程时，可以建立一个数值-符号对的列表valueSymbols，按数值从大到小排列。遍历 valueSymbols 中的每个数值-符号对，若当前数值 value 不超过 num，则从 num 中不断减去 value，直至 num 小于 value，然后遍历下一个数值-符号对。若遍历中 num 为 0 则跳出循环。valueSymbols对应表和代码如下

   ```java
   int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
   String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
   public String intToRoman(int num) {
       StringBuffer roman = new StringBuffer();
       for (int i = 0; i < values.length; ++i) {
           int value = values[i];
           String symbol = symbols[i];
           while (num >= value) {
               num -= value;
               roman.append(symbol);
           }
           if (num == 0) {
               break;
           }
       }
       return roman.toString();
   }
   ```

10. 三数之和(15)：

   刚开始做这题直接三重循环遍历，会超出时间限制，需要改进算法，看来LeetCode上的题目有些**不能直接模拟**，要想想降低复杂度的做法。「不重复」的本质是什么？我们保持三重循环的大框架不变，只需要保证：

   第二重循环枚举到的元素不小于当前第一重循环枚举到的元素；

   第三重循环枚举到的元素不小于当前第二重循环枚举到的元素。

   也就是说，我们枚举的三元组 (a, b, c) 满足 a≤b≤c，保证了只有(a,b,c) 这个顺序会被枚举到，而 (b, a, c)、(c, b, a) 等等这些不会，这样就减少了重复。要实现这一点，我们可以将数组中的元素从小到大进行排序，随后使用普通的三重循环就可以满足上面的要求。

   同时，**对于每一重循环而言，相邻两次枚举的元素不能相同**，否则也会造成重复。举个例子，如果排完序的数组为

   [0, 1, 2, 2, 2, 3]
    ^  ^  ^
   我们使用三重循环枚举到的第一个三元组为 (0, 1, 2)，如果第三重循环继续枚举下一个元素，那么仍然是三元组 (0, 1, 2)，产生了重复。因此我们需要将第三重循环「跳到」下一个不相同的元素，即数组中的最后一个元素 3，枚举三元组 (0, 1, 3)。

   ![image-20220411225207697](image-20220411225207697.png)

   这个方法就是我们常说的「双指针」，当我们需要枚举数组中的两个元素时，如果我们发现随着第一个元素的递增，第二个元素是递减的，那么就可以使用双指针的方法，将枚举的时间复杂度从 O(N^2) 减少至 O(N)。为什么是 O(N) 呢？这是因为在枚举的过程每一步中，「左指针」会向右移动一个位置（也就是题目中的 b），而「右指针」会向左移动若干个位置，这个与数组的元素有关，但我们知道它一共会移动的位置数为 O(N)，均摊下来，每次也向左移动一个位置，因此时间复杂度为 O(N)。

   双指针看似容易理解，但不同题目下的**边界条件**都不尽相同，边界条件应该是一个难点，需要仔细考虑。

   ```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> list = new ArrayList<>();
    for (int i = 0; i < nums.length; i++) {
        int k = nums.length - 1;
        int target = -nums[i];
        if (i == 0 || nums[i] != nums[i - 1]) {//对于每一重循环相邻两个元素不能相同
            for (int j = i + 1; j < nums.length; j++) {
                if (j == i + 1 || nums[j] != nums[j - 1]) {
                    while (j < k && nums[j] + nums[k] > target) {
                        k--;
                    }
                    // 如果指针重合，一种情况是上一次j<k时（j和k相邻），nums[j] + nums[k] > target
                    // 另一种情况时j不断增加但nums[j]+nums[k]<target，随着 j 后续的增加
                    // 就不会有满足 nums[j] + nums[k] = target 并且 j<k 的值了，可以退出循环
                    if (j == k) {
                        break;
                    }
                    if (nums[j] + nums[k] == target) {
                        List<Integer> integerList = new ArrayList<>();
                        integerList.add(nums[i]);
                        integerList.add(nums[j]);
                        integerList.add(nums[k]);
                        list.add(integerList);
                        // 这里不要写k--，要让k--交由上面的while循环来判断执行
                        // 如果写了k--，在j和k相邻的情况下，下一次循环j会大于k（j比k大1），
                        // while循环和if失效，所以k--要交由while循环来执行
                    }
                }
            }
        }
    }
    return list;
   ```

11. 电话号码的字母组合(17)：

   这道题其实~~直接模拟~~也能做，因为最多只有四位电话号码，直接根据不同的位数电话号码采取一层for，二层for，三层for，四层for遍历也可AC。

   首先使用哈希表存储每个数字对应的所有可能的字母，然后进行回溯操作。

   回溯过程中维护一个字符串，表示已有的字母排列（如果未遍历完电话号码的所有数字，则已有的字母排列是不完整的）。该字符串初始为空。每次取电话号码的一位数字，从哈希表中获得该数字对应的所有可能的字母，并将其中的一个字母插入到已有的字母排列后面，然后继续处理电话号码的后一位数字，直到处理完电话号码中的所有数字，即得到一个完整的字母排列。然后进行回退操作，遍历其余的字母排列。

   回溯算法用于寻找所有的可行解，如果发现一个解不可行，则会舍弃不可行的解。在这道题中，由于每个数字对应的每个字母都可能进入字母组合，因此不存在不可行的解，直接穷举所有的解即可。下面贴上带有回溯的递归函数的代码


```java
// 这个回溯的递归函数借鉴了赫夫曼编码的向左为0向右为1的那个递归函数
public void f(List<String> res, HashMap<Character, String> stringHashMap, String digits, StringBuilder stringBuilder, int index) {
    //用index遍历电话号码，for循环遍历每一个电话号码对应的所有字符
    //  2   3
    // abc def
    if (digits.length() != index) {
        String digitString = stringHashMap.get(digits.charAt(index));
        for (int i = 0; i < digitString.length(); i++) {
            stringBuilder.append(digitString.charAt(i));
            f(res, stringHashMap, digits, stringBuilder, index + 1);
            // 当该递归函数返回进入下一行时，说明已经遍历完电话号码的所有数字，
            // 下一步应该取最后一个电话号码的下一个字符（也就是i++），需要删掉stringBuilder2的末尾一位，
            // 让最后一个电话号码的下一个字符填入进来
            stringBuilder.deleteCharAt(stringBuilder.length() - 1);
        }
    } else {
        res.add(stringBuilder.toString());
    }
}
```

12. 删除链表的倒数第N个结点(19)：

    我的想法是从头节点开始遍历，用于遍历的结点记为indexNode，如果indexNode向后遍历n次为null的话，证明此时indexNode结点即为要删除的结点，看下面这个图可以轻松证明

![image-20220414201955313](image-20220414201955313.png)

​	还需要设置一个preNode作为indexNode的前一个结点，这样才能做到删除操作，除此以外需要注意一个特殊情况即删除的结点是头结点，直接返回head.next即可。

​	一个小tips：处理链表是可以在头结点前面加上一个哑节点dummy，dummy不保存任何信息只是指向原链表的头结点，这样可以有效解决删除链表头相关的特殊情况，可以把有关链表头的特殊情况统一到普通情况的代码中。（不是下面贴的那段代码）

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    if (head.next==null){
        return null;//该链表只有一个结点
    }
    ListNode indexNode=head;
    ListNode preNode=null;
    while (indexNode!=null){
        ListNode temp=indexNode;
        for (int i = 0; i < n; i++) {
            temp=temp.next;
        }
        if (temp==null){
            if (preNode==null){
                //删除的是头节点的情况
                return head.next;
            }
            preNode.next=preNode.next.next;
            break;
        }
        //这里的保存preNode是一个保存链表前一个结点的经典操作
        preNode=indexNode;
        indexNode=indexNode.next;
    }
    return head;
}
```

13. 有效的括号(20)：

​		典型的**栈的应用**，我们遍历给定的字符串 s。当我们遇到一个左括号时，我们会期望在后续的遍历中，有一个相同类型的右括号将其闭合。由于后遇到的左括号要先闭合（**后进先出**），因此我们可以将这个左括号放入栈顶。当我们遇到一个右括号时，我们需要将一个相同类型的左括号闭合。此时，我们可以取出栈顶的左括号并判断它们是否是相同类型的括号。**如果不是相同的类型，或者栈中并没有左括号**，那么字符串 s 无效，返回 False。在遍历结束后，如果栈中没有左括号，说明我们将字符串 s 中的所有左括号闭合，返回 True，否则返回 False。

​		注意到有效字符串的长度一定为偶数，因此如果字符串的长度为奇数，我们可以直接返回 False，省去后续的遍历判断过程。

14. 合并k个升序列表(23):

    前置知识：合并两个有序链表

    - 首先我们需要一个变量 head 来保存合并之后链表的头部，你可以把 head 设置为一个虚拟的头（也就是 head 的 val 属性不保存任何值），这是为了方便代码的书写，在整个链表合并完之后，返回它的下一位置即可。**设置哑结点方便链表操作的代码书写**
    - 我们需要一个指针 tail 来记录下一个插入位置的前一个位置，以及两个指针 aPtr 和 bPtr 来记录 a 和 b 未合并部分的第一位。注意这里的描述，tail 不是下一个插入的位置，aPtr 和 bPtr 所指向的元素处于「待合并」的状态，也就是说它们还没有合并入最终的链表。 当然你也可以给他们赋予其他的定义，但是定义不同实现就会不同。
    - 当 aPtr 和 bPtr 都不为空的时候，取 val 熟悉较小的合并；如果 aPtr 为空，则把整个 bPtr 以及后面的元素全部合并；bPtr 为空时同理。

    ```java
    public ListNode mergeTwoLists(ListNode a, ListNode b) {
        if (a == null || b == null) {
            return a != null ? a : b;
        }
        ListNode head = new ListNode(0);//这里的head就是哑结点
        ListNode tail = head, aPtr = a, bPtr = b;
        while (aPtr != null && bPtr != null) {
            if (aPtr.val < bPtr.val) {
                tail.next = aPtr;
                aPtr = aPtr.next;
            } else {
                tail.next = bPtr;
                bPtr = bPtr.next;
            }
            tail = tail.next;
        }
        tail.next = (aPtr != null ? aPtr : bPtr);
        return head.next;
    }
    ```

    方法一：顺序合并

    我们可以想到一种最朴素的方法：用一个变量 ans 来维护以及合并的链表，第 i 次循环把第 i个链表和 ans 合并，答案保存到 ans 中。

    ```java
    public ListNode mergeKLists(ListNode[] lists){
        ListNode ans=null;
        for (int i = 0; i < lists.length; i++) {
            ans=mergeTwoLists(ans,lists[i]);
        }
        return ans;
    }
    ```

    方法二：分治合并

    考虑优化方法一，用分治的方法进行合并。

![image-20220416111324477](image-20220416111324477.png)

```java
public ListNode merge(ListNode[] lists,int l,int r){
    if (l==r){
        return lists[l];
    }
    if (l>r){
        return null;//本题中其实并不会出现l>r
    }
    int mid=(l+r)/2;
    return mergeTwoLists(merge(lists,l,mid),merge(lists,mid+1,r));
}
public ListNode mergeKLists(ListNode[] lists){
    if (lists==null||lists.length==0){//注意这两个特殊情况的判断
        return null;
    }
    return merge(lists,0,lists.length-1);
}
```

- 长度为0的数组 int[] arr = new int[0]，也称为空数组，虽然arr长度为0，但是依然是一个**对象**

- null数组，int[] arr = null；arr是一个数组类型的**空引用**。

​	方法三：使用优先队列合并

这个方法和前两种方法的思路有所不同，我们需要维护当前每个链表没有被合并的元素的最前面一个，k 个链表就最多有 k 个满足这样条件的元素，每次在这些元素里面选取 val 属性最小的元素合并到答案中。在选取最小元素的时候，我们可以用**优先队列**来优化这个过程。

```java
class Status implements Comparable<Status> {
    int val;
    ListNode ptr;

    public Status(int val, ListNode ptr) {
        this.val = val;
        this.ptr = ptr;
    }

    @Override
    public int compareTo(Status o) {
        return this.val - o.val;
    }
}

PriorityQueue<Status> queue = new PriorityQueue<>();

public ListNode mergeKLists(ListNode[] lists) {
    for (ListNode node :
         lists) {
        if (node != null) {
            queue.offer(new Status(node.val, node));
        }
    }
    ListNode head = new ListNode(0);
    ListNode tail = head;
    while (!queue.isEmpty()) {
        Status f = queue.poll();
        tail.next = f.ptr;
        tail = tail.next;
        if (f.ptr.next != null) {
            queue.offer(new Status(f.ptr.next.val, f.ptr.next));
        }
    }
    return head.next;
}
```

15. 下一个排列(31)：

    整数数组的 **下一个排列** 是指其整数的下一个**字典序更大的排列**，注意到下一个排列总是比当前排列要大，除非该排列已经是最大的排列。我们希望找到一种方法，能够找到一个大于当前序列的新序列，且变大的幅度尽可能小。具体地：

    我们需要将一个左边的「较小数」与一个右边的「较大数」交换，以能够让当前排列变大，从而得到下一个排列。

    同时我们要让这个「较小数」尽量靠右，而「较大数」尽可能小。当交换完成后，「较大数」右边的数需要按照升序重新排列。这样可以在保证新排列大于原来排列的情况下，使变大的幅度尽可能小。

    以排列 [4,5,2,6,3,1] 为例：

    我们能找到的符合条件的一对「较小数」与「较大数」的组合为 2 与 3，满足「较小数」尽量靠右，而「较大数」尽可能小。

    当我们完成交换后排列变为 [4,5,3,6,2,1]，此时我们可以重排「较小数」右边的序列，序列变为 [4,5,3,1,2,6]。

    具体地，我们这样描述该算法，对于长度为 n 的排列 a：

    - 首先从后向前查找第一个顺序对 (i,i+1)，满足 a[i] < a[i+1]。这样「较小数」即为 a[i]。此时 [i+1,n) 必然是下降序列。

    - 如果找到了顺序对，那么在区间 [i+1,n) 中从后向前查找第一个元素 j 满足 a[i] < a[j]。这样「较大数」即为 a[j]。

    - 交换 a[i] 与 a[j]，此时可以证明区间 [i+1,n) 必为降序。我们可以直接使用双指针反转区间 [i+1,n) 使其变为升序，而无需对该区间进行排序。

```java
public void nextPermutation(int[] nums) {
    int index=nums.length-1;
    //从后向前找到第一个不满足从后向前为升序的位置，该位置为index-1（如果index！=0）
    //从后向前为升序意味着该子序列是最大的序列，无法通过交换该子序列的内部获得下一个排列
    //所以需要一直找到一个不满足该条件的位置
    while (index>0&&nums[index]<=nums[index-1]){
        index--;
    }
    int left=0,right=nums.length-1;;
    if (index != 0) {//如果index!=0意味着该数列不是递减数列
        int minTemp = index - 1;
        index = nums.length - 1;
        //在区间 [i+1,n) 中从后向前查找第一个元素 j 满足 a[i] < a[j]。这样「较大数」即为 a[j]。
        while (nums[index] <= nums[minTemp]) {
            index--;
        }
        //交换 a[i] 与 a[j]，此时可以证明区间 [i+1,n) 必为降序，交换后该序列一定比原序列大，但要保证**变大的幅度尽可能小**，所以需要把后面的子序列从小到大排列，这样就是下一个排列
        swap(nums, minTemp, index);
        left = minTemp + 1;
    }
    //使用双指针反转区间 [i+1,n) 使其变为升序，而无需对该区间进行排序。
    while (left<right){
        swap(nums,left,right);
        left++;
        right--;
    }
}
public void swap(int[] nums,int a,int b){
    int temp=nums[a];
    nums[a]=nums[b];
    nums[b]=temp;
}
```

16. 最长有效括号(32)：

    + 方法一：动态规划

      首先要找到转移方程，怎么想转移方程？首先想的时候从已经求出了 `dp[i-1][j-1]` 入手，再加上已知 `s[i]`、`p[j]`，要想的问题就是怎么去求 `dp[i][j]`。

      结合题目，有最长这个字眼，可以考虑尝试使用 动态规划 进行分析。这是一个 最值型 动态规划的题目。

      动态规划题目分析的 4 个步骤：

      - 确定状态

        ​		研究**最优策略的最后一步**

        ​		化为子问题

      - 转移方程

        ​		根据子问题定义得到

      - 初始条件和边界情况

      - 计算顺序

      首先，我们定义一个 int[] dp 数组，其中第 i 个元素表示**以下标为 i 的字符结尾**的最长有效子字符串的长度。注意：如果第i个元素为 `(` 那么s[i] 无法和其之前的元素组成有效的括号对，所以，dp[i] = 0，以 `(` 结尾的子串对应的dp值都为零。下面来推导状态转移方程

      s[i]== '('

      这时，s[i] 无法和其之前的元素组成有效的括号对，所以，dp[i] = 0

      s[i] == ')'

      这时，需要看其前面对元素来判断是否有有效括号对。

      **情况1:**

      s[i - 1] == '('

      即 s[i] 和 s[i - 1] 组成一对有效括号，有效括号长度新增长度2，i位置对最长有效括号长度为 其之前2个位置的最长括号长度加上当前位置新增的2，我们无需知道i-2位置对字符是否可以组成有效括号对。

      那么有：`dp[i] = dp[i - 2] + 2`

      **情况2:**

      s[i - 1] == ')'

      这种情况下，如果前面有和s[i]组成有效括号对的字符，即形如( (....) )，这样的话，就要求s[i - 1]位置必然是有效的括号对，否则s[i]无法和前面对字符组成有效括号对。这时，我们只需要找到和s[i]配对对位置，并判断其是否是 `(` 即可。

      和其配对对位置为：`i - dp[i - 1] - 1`

      如果：`s[i - dp[i - 1] - 1] == '('`,有效括号长度新增长度2，i位置对最长有效括号长度为 i-1位置的最长括号长度加上当前位置新增的2，那么有：

      `dp[i] = dp[i - 1] + 2`

      值得注意的是，`i - dp[i - 1] - 1`和 i 组成了有效括号对，这将是一段独立的有效括号序列，如果之前的子序列是形如 (...)这种序列，那么当前位置的最长有效括号长度还需要加上这一段。所以：`dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2`


![image-20220416165854222](image-20220416165854222.png)

```python
if s[i] == '(' :
    dp[i] = 0
if s[i] == ')' :
    if s[i - 1] == '(' :
        dp[i] = dp[i - 2] + 2 #要保证i - 2 >= 0
    if s[i - 1] == ')' and s[i - dp[i - 1] - 1] == '(' :
        dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2 
        #要保证i - dp[i - 1] - 2 >= 0
```

​			下面贴上Java的源代码

```java
public int longestValidParentheses(String s) {
    if (s.length()==0){
        return 0;
    }
    int[] dp=new int[s.length()];
    dp[0]=0;
    int maxLen=0;
    for (int i = 1; i < s.length(); i++) {
        if (s.charAt(i)==')'){
            if (s.charAt(i-1)=='('){
                dp[i]=2;
                if (i-2>=0){
                    dp[i]=dp[i-2]+2;
                }
            }else{//s[i-1]=')'
                if (i-dp[i-1]-1>=0&&s.charAt(i-dp[i-1]-1)=='('){
                    dp[i]=dp[i-1]+2;
                    if (i-dp[i-1]-2>=0){
                        dp[i]=dp[i-1]+2+dp[i-dp[i-1]-2];
                    }
                }
            }
        }
        if (dp[i]>maxLen){
            maxLen=dp[i];
        }
    }
    return maxLen;
}
```

​		方法二：使用**栈**

​		栈里存的是括号对应的**下标**，具体做法是我们始终保持**栈底元素**为当前已经遍历过的元素中「最后一个没有被匹配的右括号的下标」，这样的做法主要是考虑了边界条件的处理，栈里其他元素维护左括号的下标：

​		对于遇到的每个 ‘(’ ，我们将它的下标放入栈中
​		对于遇到的每个 ‘)’ ，我们先弹出栈顶元素表示匹配了当前右括号：

​		如果栈为空，说明当前的右括号为没有被匹配的右括号，我们将其下标放入栈中来更新我们之前提到的「最后一个没有被匹配的右括号的下标」
​		如果栈不为空，当前右括号的下标减去栈顶元素即为「以该右括号为结尾的最长有效括号的长度」
​		我们从前往后遍历字符串并更新答案即可。需要注意的是，如果一开始栈为空，第一个字符为左括号的时候我们会将其放入栈中，这样就不满足提及的「最后一个没有被匹配的右括号的下标」，为了保持统一，我们在一开始的时候往栈中放入一个值为-1 的元素。

​		总结：两种索引会入栈

1. 等待被匹配的左括号索引。
2. 充当「参照物」的右括号索引。因为：当左括号匹配光时，栈需要留一个垫底的参照物，用于计算一段连续的有效长度。

```java
public int longestValidParentheses(String s){
    int maxLen=0;
    Deque<Integer> stack=new LinkedList<>();
    stack.push(-1);
    for (int i = 0; i < s.length(); i++) {
        if (s.charAt(i)=='('){
            stack.push(i);
        }else{//s[i]==')'
            stack.pop();//先出栈，再相减计算有多少个匹配的括号
            //举个例子 ()(()))
            if (!stack.isEmpty()){
                int curLen=i-stack.peek();
                maxLen=Math.max(maxLen,curLen);
            }else{
                stack.push(i);
            }
        }
    }
    return maxLen;
}
```

​		方法三：

​		在此方法中，我们利用两个计数器 left 和 right 。首先，我们从左到右遍历字符串，对于遇到的每个 ‘(’，我们增加 left 计数器，对于遇到的每个 ‘)’ ，我们增加 right 计数器。每当 left 计数器与 right 计数器相等时，我们计算当前有效字符串的长度，并且记录目前为止找到的最长子字符串。当 right 计数器比left 计数器大时，我们将left 和 right 计数器同时变回 0。

​		这样的做法**贪心**地考虑了以当前字符下标结尾的有效括号长度，每次当右括号数量多于左括号数量的时候之前的字符我们都扔掉不再考虑，重新从下一个字符开始计算，但这样会漏掉一种情况，就是遍历的时候左括号的数量始终大于右括号的数量，即 (() ，这种时候最长有效括号是求不出来的。

​		解决的方法也很简单，我们只需要从右往左遍历用类似的方法计算即可，只是这个时候判断条件反了过来：

- 当 left 计数器比right 计数器大时，我们将 left 和right 计数器同时变回 0。
- 当 left 计数器与right 计数器相等时，我们计算当前有效字符串的长度，并且记录目前为止找到的最长子字符串。

​		这样我们就能涵盖所有情况从而求解出答案。

```java
public int longestValidParentheses(String s) {
    int left = 0, right = 0, maxLen = 0;
    for (int i = 0; i < s.length(); i++) {//从左向右遍历
        if (s.charAt(i) == '(') {
            left++;
        } else {
            right++;
        }
        if (left == right) {
            maxLen = Math.max(left + right, maxLen);
        }
        if (right > left) {
            left = 0;
            right = 0;
        }
    }
    left=0;
    right=0;
    for (int i = s.length() - 1; i >= 0; i--) {//从右向左遍历
        if (s.charAt(i) == '(') {
            left++;
        } else {
            right++;
        }
        if (left == right) {
            maxLen = Math.max(left + right, maxLen);
        }
        if (left>right){
            left=0;
            right=0;
        }
    }
    return maxLen;
}
```

17. 搜索旋转排序数组(33)：

    - 方法一：

      比如`nums = [4,5,6,7,0,1,2], target = 0`，可以将该数组分为两部分看待，从4到7，从0到2，首先比较target与nums[0]的大小，如果target<nums[0]，则在后半部分从后往前搜索，如果target>nums[0]，则在前半部分从前往后搜索

    ```java
    public int search(int[] nums, int target) {
        if (nums.length == 1) {
            if (nums[0] == target) {
                return 0;
            } else {
                return -1;
            }
        }
        int index = 0;
        //[4,5,6,7,0,1,2], target = 0
        boolean isInFront = true;
        if (nums[0] > target) {
            index = nums.length - 1;
            isInFront = false;
        }
        if (isInFront) {
            while (index < nums.length - 1 && nums[index] < nums[index + 1]) {
                if (nums[index] == target) {
                    return index;
                }
                index++;
            }
        } else {
            while (index > 0 && nums[index] > nums[index - 1]) {
                if (nums[index] == target) {
                    return index;
                }
                index--;
            }
        }
        if (nums[index] == target) {
            return index;
        }
        return -1;
    }
    ```

    - 方法二：

      对于旋转数组 nums = [4,5,6,7,0,1,2]，首先根据 nums[0] 与 target 的关系判断 target 是在左段还是右段。

      例如 target = 5, 目标值在左半段，因此在 [4, 5, 6, 7, inf, inf, inf] 这个有序数组里找就行了；
      例如 target = 1, 目标值在右半段，因此在 [-inf, -inf, -inf, -inf, 0, 1, 2] 这个有序数组里找就行了。
      如此，我们又将「旋转数组中找目标值」 转化成了 「有序数组中找目标值」

      ```java
      public int search(int[] nums, int target) {
          int left = 0, right = nums.length - 1;
          while (left <= right) {
              int mid = left + (right - left) / 2;
              if (nums[mid] == target) {
                  return mid;
              }
              // 先根据 nums[0] 与 target 的关系判断目标值是在左半段还是右半段
              if (target >= nums[0]) {
                  // 目标值在左半段时，若 mid 在右半段，则将 mid 索引的值改成 inf
                  // 注意这里的>=，如果target在最左端，就需要>=，比如
                  // nums=[5,1,2,3] target=5
                  if (nums[mid] < nums[0]) {
                      nums[mid] = Integer.MAX_VALUE;
                  }
              } else {
                // 目标值在右半段时，若 mid 在左半段，则将 mid 索引的值改成 -inf
                  if (nums[mid] >= nums[0]) {
                      nums[mid] = Integer.MIN_VALUE;
                  }
              }
              if (nums[mid] < target) {
                  left = mid + 1;
              } else {
                  right = mid - 1;
              }
          }
          return -1;
      }
      ```

18. 在排序数组中查找元素的第一个和最后一个位置(34)：

    - 方法一：

      先用二分查找找到该元素的位置，然后从该位置不断向前遍历和向后遍历，以此来找到第一个和最后一个位置

      ```java
      public int[] searchRange(int[] nums, int target) {
          if (nums.length == 0) {
              return new int[]{-1, -1};
          }
          int left = 0, right = nums.length - 1;
          int mid = (left + right) / 2, index = -1;
          while (left <= right) {
              mid = (left + right) / 2;
              if (nums[mid] == target) {
                  index = mid;
                  break;
              }
              if (nums[mid] > target) {
                  right = mid - 1;
              } else {
                  left = mid + 1;
              }
          }
          if (index == -1) {
              return new int[]{-1, -1};
          }
          int indexA = index, indexB = index;
          while (indexA > 0 && nums[indexA] == nums[indexA - 1]) {
              indexA--;
          }
          while (indexB < nums.length - 1 && nums[indexB] == nums[indexB + 1]) {
              indexB++;
          }
          return new int[]{indexA, indexB};
      }
      ```

    - 二分查找主要用于在有序的列表中查找目标值，注意二分查找的前提条件必须是有序或者部分有序的列表，当列表中目标值存在多个时，我们可以利用二分查找目标值的左边界和右边界，即**左值二分查找**和**右值二分查找**。需要特别注意目标值在列表中找不到的几种情况。

      ```java
      /**
      * 左值二分模板
      */
      public int searchLeft(int[] nums, int target) {
          int left = 0;
          int right = nums.length - 1;
          while (left <= right) {
              // 防止溢出，如果left和right都很大直接相加会溢出，
              // 最好不要直接相加除以二
              int middle = (right - left) / 2 + left;
              if (nums[middle] >= target) {
                  // 继续往左边找
                  right = middle - 1;
              } else {
                  // 继续往右边找
                  left = middle + 1;
              }
          }
          // 考虑目标值不在数组中的两种情况,目标值小于最小值left等于0，
          // 目标值大于最大值left等于num.length
          if (left == nums.length || nums[left] != target) left = -1;
          return left;
      }
      
      /**
      * 右值二分模板
      */
      public int searchRight(int[] nums, int target) {
          int left = 0;
          int right = nums.length - 1;
          while (left <= right) {
              // 防止溢出
              int middle = (right - left) / 2 + left;
              if (nums[middle] <= target) {
                  // 继续往右边找
                  left = middle + 1;
              } else {
                  // 继续往左边找
                  right = middle - 1;
              }
          }
          // 考虑目标值不在数组中的两种情况，目标值小于最小值right会等于-1，
          // 目标值大于最大值right会等于nums.length-1
          if (right == -1 || nums[right] != target) right = -1;
          return right;
      }
      ```

19. 组数总和(39)：

  思路分析：根据示例 1：输入: candidates = [2, 3, 6, 7]，target = 7。

  候选数组里有 2，如果找到了组合总和为 7 - 2 = 5 的所有组合，再在之前加上 2 ，就是 7 的所有组合；
  同理考虑 3，如果找到了组合总和为 7 - 3 = 4 的所有组合，再在之前加上 3 ，就是 7 的所有组合，依次这样找下去。
  基于以上的想法，可以画出如下的树形图。建议大家自己在纸上画出这棵树，这一类问题都需要先画出树形图，然后编码实现。

  ![image-20220418221514841](image-20220418221514841.png)

  编码通过 **深度优先遍历** 实现，使用一个列表，在 深度优先遍历 变化的过程中，遍历所有可能的列表并判断当前列表是否符合题目的要求，成为「回溯算法」。

  说明上图：

  以 target = 7 为 根结点 ，创建一个分支的时 做减法 ；
  每一个箭头表示：从父亲结点的数值减去边上的数值，得到孩子结点的数值。边的值就是题目中给出的 candidate 数组的每个元素的值；
  减到 0 或者负数的时候停止，即：结点 0 和负数结点成为叶子结点；
  所有从根结点到结点 0 的路径（只能从上往下，没有回路）就是题目要找的一个结果。
  这棵树有 4 个叶子结点的值 0，对应的路径列表是 [[2, 2, 3], [2, 3, 2], [3, 2, 2], [7]]，而示例中给出的输出只有 [[7], [2, 2, 3]]。即：题目中要求每一个符合要求的解是 不计算顺序 的。下面我们分析为什么会产生重复。

  产生重复的原因是：在每一个结点，做减法，展开分支的时候，由于题目中说 **每一个元素可以重复使用**，我们考虑了 **所有的** 候选数，因此出现了重复的列表。具体的做法是：每一次搜索的时候设置 **下一轮搜索的起点** `index`，请看下图。

![image-20220418221816511](image-20220418221816511.png)

​		即：从每一层的第 2 个结点开始，都不能再搜索产生同一层结点已经使用过的 `candidate` 里的元素。

​		**剪枝提速**

​		根据上面画树形图的经验，如果 target 减去一个数得到负数，那么减去一个更大的数依然是负数，同样搜索不到结果。基于这个想法，我们可以对输入数组进行排序（从小到大），添加相关逻辑达到进一步剪枝的目的，下面贴上剪枝提速后的代码

```java
public void f(List<List<Integer>> res, List<Integer> integerList, int[] candidates, int target, int index) {
    //[2, 3, 6, 7]  4
    if (target==0){
        //添加的时候拷贝比每次传进来就拷贝效率高
        res.add(new ArrayList<>(integerList));
        return;
    }
    for (int i = index; i < candidates.length; i++) {
        if (target-candidates[i]<0){
            break;//在这里剪枝效率很高
        }
        integerList.add(candidates[i]);
        f(res,integerList,candidates,target-candidates[i],i);
        integerList.remove(integerList.size()-1);
    }
}
```

- 排列问题，讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为不同列表时），需要记录哪些数字已经使用过，此时用 used 数组；

- 组合问题，不讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为相同列表时），需要按照某种顺序搜索，此时使用 begin 变量（就是上面用的index）。

下面详细介绍一下**回溯算法与深度优先遍历**

- 回溯法 采用试错的思想，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将**取消上一步甚至是上几步的计算**，再通过其它的可能的分步解答再次尝试寻找问题的答案。回溯法通常用最简单的递归方法来实现，在反复重复上述的步骤后可能出现两种情况：

​		找到一个可能存在的正确的答案；
​		在尝试了所有可能的分步方法后宣告该问题没有答案。

- 深度优先搜索 算法（英语：Depth-First-Search，DFS）是一种用于遍历或搜索树或图的算法。这个算法会 **尽可能深** 的搜索树的分支。当结点 v 的所在边都己被探寻过，搜索将**回溯到发现结点 v 的那条边的起始结点**。这一过程一直进行到已发现从源结点可达的所有结点为止。如果还存在未被发现的结点，则选择其中一个作为源结点并重复以上过程，整个进程反复进行直到所有结点都被访问为止。

20. 全排列（46）：

    用这道题来与上面那道组数总和的回溯算法进行对比，再次深刻理解一下回溯算法和深度优先遍历（DFS）

    从全排列问题开始理解回溯算法
    我们尝试在纸上写 3 个数字、4 个数字、5 个数字的全排列，相信不难找到这样的方法。以数组 [1, 2, 3] 的全排列为例。

    先写以 1 开头的全排列，它们是：[1, 2, 3], [1, 3, 2]，即 1 + [2, 3] 的全排列（注意：递归结构体现在这里）；
    再写以 2 开头的全排列，它们是：[2, 1, 3], [2, 3, 1]，即 2 + [1, 3] 的全排列；
    最后写以 3 开头的全排列，它们是：[3, 1, 2], [3, 2, 1]，即 3 + [1, 2] 的全排列。
    总结搜索的方法：按顺序枚举每一位可能出现的情况，已经选择的数字在 当前 要选择的数字中不能出现。按照这种策略搜索就能够做到 不重不漏。这样的思路，可以用一个树形结构表示。

    ![image-20220422115357752](image-20220422115357752.png)

```java
public void dfs(List<List<Integer>> res, List<Integer> integerList, int[] nums, boolean[] isUsed, int countNum) {
    if (countNum == nums.length) {
        res.add(new ArrayList<>(integerList));//每次添加到结果列表中进行拷贝会节省时间
        return;
    }
    for (int i = 0; i < nums.length; i++) {
        if (isUsed[i]) {
            continue;
        }
        integerList.add(nums[i]);
        isUsed[i]=true;
        dfs(res, integerList, nums, isUsed, countNum + 1);
        isUsed[i] = false;//回溯之前清除这一次操作的影响
        integerList.remove(integerList.size()-1);
    }
}
```

​		**说明**：

​		每一个结点表示了求解全排列问题的不同的阶段，这些阶段通过变量的「不同的值」体现，这些变量的不同的值，称之为「状态」；
​		使用深度优先遍历有「回头」的过程，**在「回头」以后， 状态变量需要设置成为和先前一样** ，因此在回到上一层结点的过程中，需要**撤销上一次的选择**，这个操作称之为「状态重置」；
​		深度优先遍历，借助系统栈空间，保存所需要的状态变量，在编码中只需要注意遍历到相应的结点的时候，状态变量的值是正确的，具体的做法是：**往下走一层的时候，path 变量在尾部追加，而往回走的时候，需要撤销上一次的选择，也是在尾部操作，因此 path 变量是一个栈；**
​		深度优先遍历通过「回溯」操作，实现了全局使用一份状态变量的效果。
​		使用编程的方法得到全排列，就是在这样的一个树形结构中完成 遍历，从树的根结点到叶子结点形成的路径就是其中一个全排列。

​		**设计状态变量**

​		首先这棵树除了根结点和叶子结点以外，每一个结点做的事情其实是一样的，即：在已经选择了一些数的前提下，在剩下的还没有选择的数中，依次选择一个数，这显然是一个 递归 结构；
​		递归的终止条件是： 一个排列中的数字已经选够了 ，因此我们**需要一个变量来表示当前程序递归到第几层**，我们把这个变量叫做 depth，或者命名为 index ，表示当前要确定的是某个全排列中下标为 index 的那个数是多少；
​		布尔数组 used，初始化的时候都为 false 表示这些数还没有被选择，当我们选定一个数的时候，就将这个数组的相应位置设置为 true ，这样在考虑下一个位置的时候，就能够以 O(1) 的时间复杂度判断这个数是否被选择过，这是一种「以空间换时间」的思想。
​		这些变量称为「状态变量」，它们表示了在求解一个问题的时候所处的阶段。需要根据问题的场景设计合适的状态变量。

​		**每一次尝试都「复制」，则不需要回溯**

​		如果在每一个 非叶子结点 分支的尝试，都创建 新的变量 表示状态，那么在回到上一层结点的时候不需要「回溯」；在递归终止的时候也不需要做拷贝。
​		这样的做法虽然可以得到解，但也会创建很多中间变量，这些中间变量很多时候是我们不需要的，会有一定空间和时间上的消耗。

​		**剪枝**

​		回溯算法会应用「剪枝」技巧达到以加快搜索速度。有些时候，需要做一些预处理工作（例如排序）才能达到剪枝的目的。预处理工作虽然也消耗时间，但能够剪枝节约的时间更多；
​		提示：剪枝是一种技巧，通常需要根据不同问题场景采用不同的剪枝策略，需要在做题的过程中不断总结。由于回溯问题本身时间复杂度就很高，所以能用空间换时间就尽量使用空间。

​		**总结**

​		做题的时候，建议 先画树形图 ，画图能帮助我们想清楚递归结构，想清楚如何剪枝。拿题目中的示例，想一想人是怎么做的，一般这样下来，这棵递归树都不难画出。

​		在画图的过程中思考清楚：

​		分支如何产生；题目需要的解在哪里？是在叶子结点、还是在非叶子结点、还是在从跟结点到叶子结点的路径？哪些搜索会产生不需要的解的？例如：产生重复是什么原因，如果在浅层就知道这个分支不能产生需要的结果，应该提前剪枝，剪枝的条件是什么，代码怎么写？

21. 接雨水(42):

    方法一：

    按行求，先求出数组的最大的高度，然后遍历数组，从最高的一层开始，计算每一层的积水量，思路比较简单，下面贴上代码

    ```java
    public int trap(int[] height) {
        //一层一层的计算，这样直接模拟会超时，这是按行求的做法
        int maxHeight=0,res=0;
        for (int i = 0; i < height.length; i++) {
            if (height[i]>maxHeight){
                maxHeight=height[i];
            }
        }
        int tempMax=maxHeight;
        for (int i = 0; i < maxHeight; i++) {
            int leftIndex=0;
            for (int j = 0; j < height.length; j++) {
                if (height[j]==tempMax){
                    leftIndex=j;
                    height[j]--;
                    for (int k = leftIndex+1; k < height.length; k++) {
                        if (height[k]==tempMax){
                            res+=k-leftIndex-1;
                            leftIndex=k;
                            height[k]--;
                        }
                    }
                    tempMax--;
                    break;
                }
            }
        }
        return res;
    }
    ```

    方法二：

    按列求，求每一列的水，我们只需要关注当前列，以及左边最高的墙，右边最高的墙就够了。装水的多少，当然根据木桶效应，我们只需要看左边最高的墙和右边最高的墙中较矮的一个就够了。所以，根据较矮的那个墙和当前列的墙的高度可以分为两种情况。

    - 较矮的墙的高度大于当前列的墙的高度，此时积水量就是较矮的墙的高度减去当前列的墙的高度
    - 较矮的墙的高度小于或等于当前列的墙的高度，此时无法积水

    ```java
    public int trap(int[] height) {
        int indexHeight=0,res=0;
        //直接按列计算也会超时，但可以用动态规划进行优化
        for (int i = 1; i < height.length-1; i++) {
            indexHeight=height[i];
            int leftHeightMax=0,rightHeightMax=0;
            for (int j = 0; j < height.length; j++) {
                if (j<i){
                    if (height[j]>leftHeightMax){
                        leftHeightMax=height[j];
                    }
                }
                if (j>i){
                    if (height[j]>rightHeightMax){
                        rightHeightMax=height[j];
                    }
                }
            }
            int minHeight=Math.min(leftHeightMax,rightHeightMax);
            if (minHeight>indexHeight){
                res+=minHeight-indexHeight;
            }
        }
        return res;
    }
    ```

    方法三：

    使用动态规划对方法二按列求的方法进行优化

    我们注意到，解法二中。对于每一列，我们求它左边最高的墙和右边最高的墙，都是重新遍历一遍所有高度，这里我们可以优化一下。

    首先用两个数组，max_left [i] 代表第 i 列左边最高的墙的高度，max_right[i] 代表第 i 列右边最高的墙的高度。（一定要注意下，第 i 列左（右）边最高的墙，是不包括自身的，和 leetcode 上边的讲的有些不同）

    对于 max_left我们其实可以这样求。

    max_left [i] = Max(max_left [i-1],height[i-1])。它前边的墙的左边的最高高度和它前边的墙的高度选一个较大的，就是当前列左边最高的墙了。

    对于 max_right我们可以这样求。

    max_right[i] = Max(max_right[i+1],height[i+1]) 。它后边的墙的右边的最高高度和它后边的墙的高度选一个较大的，就是当前列右边最高的墙了。

    这样，我们再利用解法二的算法，就不用在 for 循环里每次重新遍历一次求 max_left 和 max_right 了。

    ```java
    public int trap(int[] height) {
        //dp做法
        int[] maxLeft = new int[height.length];//初始化都是0
        int[] maxRight = new int[height.length];
        int indexHeight=0,res=0;
        for (int i = 1; i < maxLeft.length; i++) {//从左往右求
            maxLeft[i] = Math.max(maxLeft[i - 1], height[i - 1]);
        }
        for (int i = maxLeft.length - 2; i >= 0; i--) {//从右往左求
            maxRight[i]=Math.max(maxRight[i+1],height[i+1]);
        }
        for (int i = 1; i < height.length - 1; i++) {
            indexHeight=height[i];
            int minHeight=Math.min(maxLeft[i],maxRight[i]);//直接查表得出两边较矮的高度
            if (minHeight>indexHeight){
                res+=minHeight-indexHeight;
            }
        }
        return res;
    }
    ```

    方法四：

    **双指针优化动态规划**

    我们先明确几个变量的意思：

    ```scss
    left_max：左边的最大值，它是从左往右遍历找到的
    right_max：右边的最大值，它是从右往左遍历找到的
    left：从左往右处理的当前下标
    right：从右往左处理的当前下标
    ```

    定理一：在某个位置`i`处，它能存的水，取决于它左右两边的最大值中较小的一个。

    定理二：当我们从左往右处理到left下标时，左边的最大值left_max对它而言是可信的，但right_max对它而言是不可信的。（见下图，由于中间状况未知，对于left下标而言，right_max未必就是它右边最大的值）

    定理三：当我们从右往左处理到right下标时，右边的最大值right_max对它而言是可信的，但left_max对它而言是不可信的。

    ```text
                                       right_max
     left_max                             __
       __                                |  |
      |  |__   __??????????????????????  |  |
    __|     |__|                       __|  |__
            left                      right
    ```

    对于位置`left`而言，它左边最大值一定是left_max，右边最大值**大于等于**right_max，这时候，如果`left_max<right_max`成立，那么它就知道自己能存多少水了。无论右边将来会不会出现更大的right_max，都不影响这个结果。 所以当`left_max<right_max`时，我们就希望去处理left下标，反之，我们希望去处理right下标。

    ```java
    public int trap(int[] height) {
        int res = 0, left = 1, right = height.length - 2;
        int leftMax = height[0], rightMax = height[height.length - 1];//初始化左边最大高度和右边最大高度
        while (left <= right) {//等于是保证最后一列也会被处理
            if (leftMax < rightMax) {
                if (leftMax > height[left]) {//先判断能不能积水再对左边最大高度进行更新
                    res += leftMax - height[left];
                }
                leftMax = Math.max(leftMax, height[left]);
                left++;
            } else {//leftMax>=rightMax
                if (rightMax>height[right]){
                    res+=rightMax-height[right];
                }
                rightMax = Math.max(rightMax,height[right]);
                right--;
            }
        }
        return res;
    }
    ```

    方法五：

    使用**栈**，说到栈，我们肯定会想到括号匹配了。我们仔细观察蓝色的部分，可以和括号匹配类比下。每次匹配出一对括号（找到对应的一堵墙），就计算这两堵墙中的水。我们用栈保存每堵墙。

​		当遍历墙的高度的时候，如果当前高度小于栈顶的墙高度，说明这里会有积水，我们将墙的高度的下标入栈。

​		如果当前高度大于栈顶的墙的高度，说明之前的积水到这里停下，我们可以计算下有多少积水了。计算完，就把当前的墙继续入栈，作为新的积水的墙。

​		总体的原则就是，当前高度小于等于栈顶高度，入栈，指针后移。当前高度大于栈顶高度，出栈，计算出当前墙和栈顶的墙之间水的多少，然后计算当前的高度和新栈的高度的关系，重复第 2 步。直到当前墙的高度不大于栈顶高度或者栈空，然后把当前墙入栈，指针后移。

​		而对于计算 current 指向墙和新的栈顶之间的水，根据图的关系，我们可以直接把这两个墙当做之前解法三的 max_left 和 max_right，然后之前弹出的栈顶当做每次遍历的 height [ i ]。水量就是 Min ( max _ left ，max _ right ) - height [ i ]，只不过这里需要乘上两个墙之间的距离。可以根据下面这个图自己推导（验证）一下计算方法即可

![image-20220422162412824](image-20220422162412824.png)

```java
public int trap(int[] height){
    int res=0;
    Deque<Integer> stack=new LinkedList<>();
    int current=0;
    while (current<height.length){
        //当前高度大于栈顶的高度
        while (!stack.isEmpty()&&height[current]>height[stack.peek()]){
            int h=height[stack.pop()];//每次遍历的 height[i]
            if (stack.isEmpty()){
                break;//如果栈为空直接将此时的current入栈即可
            }
            int distance=current-stack.peek()-1;//计算两堵墙之间的距离
            int min=Math.min(height[current],height[stack.peek()]);//取两堵墙的较矮的一堵墙的高度
            res+=distance*(min-h);
        }
        stack.push(current);
        current++;
    }
    return res;
}
```

22. 旋转图像(48):

    方法一：使用辅助数组

    对于矩阵中第 i 行的第 j个元素，在旋转后，它出现在倒数第 i 列的第 j个位置，即第j行第n-i-1列。这样以来，我们使用一个与 matrix 大小相同的辅助数组 newMatrix 临时存储旋转后的结果。我们遍历 matrix 中的每一个元素，根据上述规则将该元素存放到 newMatrix 中对应的位置。在遍历完成之后，再将 newMatrix 中的结果复制到原数组中即可。代码比较简单就不放上来了

    方法二：原地旋转

    **想出方法一中的等式很重要**

    ![image-20220422224637149](image-20220422224637149.png)

​		不断重复此过程，就可以发现这四项处于一个循环中，并且每一项旋转后的位置就是下一项所在的位置

![image-20220422224751882](image-20220422224751882.png)

​		当我们知道了如何原地旋转矩阵之后，还有一个重要的问题在于：我们应该枚举哪些位置 (row,col) 进行上述的原地交换操作呢？由于每一次原地交换四个位置，因此：

![image-20220422224901047](image-20220422224901047.png)

![image-20220422224918636](image-20220422224918636.png)

```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n / 2; i++) {
        for (int j = 0; j < (n+1) / 2; j++) {
            //如果n是奇数，(n-1)/2=n/2
            //如果n是偶数，(n+1)/2=n/2
            int temp = matrix[i][j];
            matrix[i][j] = matrix[n - j - 1][i];
            matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
            matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
            matrix[j][n - i - 1] = temp;
        }
    }
}
```

​		方法三：用翻转代替旋转

​		可以通过先进行水平翻转，再进行对角线翻转得到结果。

![image-20220422225301778](image-20220422225301778.png)

```java
public void rotate(int[][] matrix) {
    int n=matrix.length;
    //水平翻转
    for (int i = 0; i < n / 2; i++) {
        for (int j = 0; j < matrix[i].length; j++) {
            int temp=matrix[i][j];
            matrix[i][j]=matrix[n-i-1][j];
            matrix[n-i-1][j]=temp;
        }
    }
    //对角线翻转
    for (int i = 0; i < matrix.length; i++) {
        for (int j = i+1; j < matrix[i].length; j++) {
            int temp=matrix[i][j];
            matrix[i][j]=matrix[j][i];
            matrix[j][i]=temp;
        }
    }
}
```

23. 字母异位词分组(49)：

    两个字符串互为字母异位词，当且仅当**两个字符串包含的字母相同**。同一组字母异位词中的字符串具备相同特征，可以使用相同特征作为一组字母异位词的标志，使用哈希表存储每一组字母异位词，哈希表的键为一组字母异位词的标志，哈希表的值为一组字母异位词列表。

    遍历每个字符串，对于每个字符串，得到该字符串所在的一组字母异位词的标志，将当前字符串加入该组字母异位词的列表中。遍历全部字符串之后，哈希表中的每个键值对即为一组字母异位词。

    以下的两种方法分别使用排序和计数作为哈希表的键，也就是用排序和计数两种方式求出的对应的字符串的特征来作为哈希表的键。代码可以很好的复习一下Java的集合的使用。

    方法一：排序

    由于互为字母异位词的两个字符串包含的字母相同，因此对两个字符串分别进行排序之后得到的字符串一定是相同的，故可以将**排序之后的字符串**作为哈希表的键。

    ```java
    public List<List<String>> groupAnagrams(String[] strs){
        HashMap<String,List<String>> map=new HashMap<>();
        for (String str :
             strs) {
            char[] arr=str.toCharArray();
            Arrays.sort(arr);
            String key=new String(arr);
            //getOrDefault() 方法获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值。
            //hashmap.getOrDefault(Object key, V defaultValue)
            List<String> list=map.getOrDefault(key,new ArrayList<>());
            list.add(str);
            map.put(key,list);
        }
        return new ArrayList<>(map.values());
    }
    ```

    方法二：计数

    由于互为字母异位词的两个字符串包含的字母相同，因此两个字符串中的相同字母出现的次数一定是相同的，故可以将每个字母出现的次数使用字符串表示，作为哈希表的键。

    由于字符串只包含小写字母，因此对于每个字符串，可以使用长度为 26 的数组记录每个字母出现的次数。

    ```java
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String,List<String>> map=new HashMap<>();
        for (String str :
             strs) {
            int[] counts=new int[26];
            for (int i = 0; i < str.length(); i++) {
                counts[str.charAt(i)-'a']++;
            }
            // 将每个出现次数大于 0 的字母和出现次数按顺序拼接成字符串，作为哈希表的键
            StringBuffer stringBuffer=new StringBuffer();
            for (int i = 0; i < 26; i++) {
                if (counts[i]!=0){
                    stringBuffer.append((char)('a'+i));
                    stringBuffer.append(counts[i]);
                }
            }
            String key=stringBuffer.toString();
            List<String> list=map.getOrDefault(key,new ArrayList<>());
            list.add(str);
            map.put(key,list);//设置哈希表的键值对
        }
        return new ArrayList<>(map.values());
    }
    ```

24. 最大子数组和(53)：

    方法一：动态规划

    设计状态思路：**把不确定的因素确定下来**，进而**把子问题定义清楚，把子问题定义得简单**。动态规划的思想通过解决了一个一个简单的问题，进而把简单的问题的解组成了复杂的问题的解。

    编写动态规划解题的步骤，把「状态定义」「状态转移方程」「初始化」「输出」「是否可以空间优化」全都写出来。

    定义状态（定义子问题）

    `dp[i]`：表示以 `nums[i]` **结尾** 的 **连续** 子数组的最大和。

    **说明**：「结尾」和「连续」是关键字。

    状态转移方程（描述子问题之间的联系）

    根据状态的定义，由于 nums[i] 一定会被选取，并且以 nums[i] 结尾的连续子数组与以 nums[i - 1] 结尾的连续子数组只相差一个元素 nums[i] 。

    - 如果 `dp[i - 1] > 0`，那么可以把 `nums[i]` 直接接在 `dp[i - 1]` 表示的那个数组的后面，得到和更大的连续子数组；
    - 如果 dp[i - 1] <= 0，那么 nums[i] 加上前面的数 dp[i - 1] 以后值不会变大。于是 dp[i] 「另起炉灶」，此时单独的一个 nums[i] 的值，就是 dp[i]。

    以上两种情况的最大值就是 `dp[i]` 的值，写出如下状态转移方程：

    ![image-20220423152015663](image-20220423152015663.png)

    状态转移方程还可以这样写，反正求的是最大值，也不用分类讨论了，就这两种情况，取最大即可，因此还可以写出状态转移方程如下：

    ![image-20220423152050928](image-20220423152050928.png)

    友情提示：求解动态规划的问题经常要分类讨论，这是因为动态规划的问题本来就有「最优子结构」的特点，即大问题的最优解通常由小问题的最优解得到。因此我们在设计子问题的时候，就需要把求解出所有子问题的结果，进而选出原问题的最优解。

    思考初始值

    `dp[0]` 根据定义，只有 1 个数，一定以 `nums[0]` 结尾，因此 `dp[0] = nums[0]`。

    思考输出

    **注意**：这里状态的定义不是题目中的问题的定义，**不能直接将最后一个状态返回回去**，而要返回dp数组的最大值

25. 跳跃游戏(55)：

    方法一：

    根据题目所给条件`0 <= nums[i] <= 105`，如果数组中的数字全大于0，那么每次跳一步一定可以到达终点，那么就只需判断等于0的位置，看在等于0的位置前面有没有位置可以跳过0，这样又能化归到全部大于0的情况，~~偷鸡做法~~。

    > 开始想用回溯法来做，会超时，思路是从起点开始遍历所有可能走的情况看最终能不能走到终点，类似dfs

    ```java
    public boolean canJump(int[] nums) {
        //[2,0,0]，这种情况需要特别判断
        if (nums.length==1){
            return true;
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i]==0){
                int temp=i-1;
                boolean canJumpZero=false;
                while (temp>=0){
                    //如果从temp位置可以直接跳到结尾当然也能跳过0，这是防止结尾的数是0导致算法不正确
                    if (nums[temp]>=i-temp+1||nums[temp]>=nums.length-1-temp){
                        canJumpZero=true;
                        break;
                    }
                    temp--;
                }
                if (!canJumpZero){//不能跳过零就会卡在零的位置直接返回false即可
                    return false;
                }
            }
        }
        return true;
    }
    ```

    方法二：贪心

    设想一下，对于数组中的任意一个位置 y，我们如何判断它是否可以到达？根据题目的描述，只要存在一个位置 x，它本身可以到达，并且它跳跃的最大长度为 x+nums[x]，这个值大于等于 y，即x+nums[x]≥y，那么位置 y 也可以到达。

    换句话说，对于每一个可以到达的位置 x，它使得 x+1,x+2,⋯,x+nums[x] 这些连续的位置都可以到达。

    这样以来，我们依次遍历数组中的每一个位置，并实时维护 最远可以到达的位置。对于当前遍历到的位置 x，如果它在 最远可以到达的位置 的范围内，那么我们就可以从起点通过若干次跳跃到达该位置，因此我们可以用 x+nums[x] 更新 最远可以到达的位置。

    在遍历的过程中，如果 最远可以到达的位置 大于等于数组中的最后一个位置，那就说明最后一个位置可达，我们就可以直接返回 True 作为答案。反之，如果在遍历结束后，最后一个位置仍然不可达，我们就返回 False 作为答案。

    以题目中的示例一[2, 3, 1, 1, 4]为例：

    我们一开始在位置 0，可以跳跃的最大长度为 2，因此最远可以到达的位置被更新为 2；

    我们遍历到位置 1，由于1≤2，因此位置 1 可达。我们用 1 加上它可以跳跃的最大长度 3，将最远可以到达的位置更新为 1+3=4。由于 4 大于等于最后一个位置 4，因此我们直接返回 True。

    ```java
    public boolean canJump(int[] nums) {
        int cur=0;
        int canReach=cur+nums[cur];
        for (int i = 1; i < nums.length; i++) {
            if (canReach>= nums.length-1){
                return true;//稍微优化一下，如果能够到达终点就直接返回true了不要继续遍历
            }
            if (canReach>=i){
                canReach=Math.max(canReach,i+nums[i]);
            }else{
                return false;
            }
        }
        return true;
    }
    ```

26. 合并区间(56)：

    如果我们按照区间的左端点排序，那么在排完序的列表中，可以合并的区间一定是连续的。如下图所示，标记为蓝色、黄色和绿色的区间分别可以合并成一个大区间，它们在排完序的列表中是连续的：

    ![image-20220423194439426](image-20220423194439426.png)

    我们用数组 merged 存储最终的答案。

    首先，我们将列表中的区间按照左端点升序排序。然后我们将第一个区间加入 merged 数组中，并按顺序依次考虑之后的每个区间：

    - 如果当前区间的左端点在数组 merged 中最后一个区间的右端点之后，那么它们不会重合，我们可以直接将这个区间加入数组 merged 的末尾；

    - 否则，它们重合，我们需要用当前区间的右端点更新数组 merged 中最后一个区间的右端点，将其置为二者的较大值。举几个例子，`[1,3] 和 [3,6]`，`[1,3]和[2,6]`，但当前区间的左端点一定小于数组末尾的左端点，因为前面已经排过序了

    ```java
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (o1, o2) -> o1[0]-o2[0]);//按照数组的第一个元素升序排序
        List<int[]> merged=new ArrayList<>();
        for (int i = 0; i < intervals.length; i++) {
            int left=intervals[i][0];
            int right=intervals[i][1];
            //当前区间的左端点在数组 merged 中最后一个区间的右端点之后
            if (merged.size()==0||merged.get(merged.size()-1)[1]<left){
                merged.add(intervals[i]);
            }else{//重置数组末尾的右端点
                merged.get(merged.size()-1)[1]=Math.max(merged.get(merged.size()-1)[1],right);
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
    ```

27. 不同路径(62)：

    方法一：递归，~~虽然不能AC~~还是贴上代码

    ```java
    public void findPaths(int m, int n, int x, int y, int[] paths) {
        if (x == m && y == n) {
            paths[0]++;
            return;
        }
        if (x > m || y > n) {
            //越界就直接返回
            return;
        }
        findPaths(m, n, x + 1, y, paths);//向下走
        findPaths(m, n, x, y + 1, paths);//向右走
    }
    
    public int uniquePaths(int m, int n) {
        int[] paths = new int[1];//传数组进去是为了能在函数里面修改值
        findPaths(m, n, 1, 1, paths);
        return paths[0];
    }
    ```

    方法二：动态规划

    用f（i，j）表示从左上角走到第i行第j列位置的路径总数，1=<i<=m，1=<j<=n

    由于我们每一步只能从向下或者向右移动一步，因此要想走到 (i, j)，如果向下走一步，那么会从 (i-1, j) 走过来；如果向右走一步，那么会从 (i, j-1) 走过来。

    因此我们可以写出动态规划转移方程：`f(i, j) = f(i-1, j) + f(i, j-1)`
    需要注意**初始条件**，dp [1] [1] = 1，表示从（1，1）走到（1，1）有一种走法，这是为了保证m和n等于一时代码也是正确的，其次是第一行和第一列都是1，因为只能向右走和向下走

    ```java
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        dp[1][1] = 1;
        for (int i = 2; i < m + 1; i++) {
            dp[i][1] = 1;
        }
        for (int i = 2; i < n + 1; i++) {
            dp[1][i] = 1;
        }
        for (int i = 2; i < m + 1; i++) {
            for (int j = 2; j < n + 1; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m][n];
    }
    ```

    总结：

    **动态规划的解题套路**

    什么样的问题可以考虑使用动态规划解决呢？

    如果一个问题，可以把所有可能的答案穷举出来，并且穷举出来后，发现存在重叠子问题，就可以考虑使用动态规划。比如一些求最值的场景，如**最长递增子序列、最小编辑距离、背包问题、凑零钱问题**等等，都是动态规划的经典应用场景。

    动态规划的核心思想就是**拆分子问题，记住过往，减少重复计算。** 并且动态规划一般都是自底向上的，因此到这里总结了一下我做动态规划的思路：

    - 穷举分析
    - 确定边界
    - 找出规律，确定最优子结构
    - 写出状态转移方程

    **动态规划是自底向上解决问题，递归是自顶向下解决问题**

28. 最小路径和(64)：

    与62题的动态规划大同小异，由于路径的方向只能是向下或向右，因此网格的第一行的每个元素只能从左上角元素开始向右移动到达，网格的第一列的每个元素只能从左上角元素开始向下移动到达，此时的路径是唯一的，因此每个元素对应的最小路径和即为对应的路径上的数字总和。

    对于不在第一行和第一列的元素，可以从其上方相邻元素向下移动一步到达，或者从其左方相邻元素向右移动一步到达，元素对应的最小路径和等于其上方相邻元素与其左方相邻元素两者对应的最小路径和中的最小值加上当前元素的值。由于每个元素对应的最小路径和与其相邻元素对应的最小路径和有关，因此可以使用动态规划求解。

    创建二维数组 dp，与原始网格的大小相同，dp [i] [j] 表示从左上角出发到 (i,j) 位置的最小路径和。显然，dp[0] [0]=grid[0] [0]。对于 dp 中的其余元素，通过以下状态转移方程计算元素值。

    当 i>0 且 j=0 时，`dp [i] [j]=dp [i-1] [0]+grid[i] [0]`

    当 i=0 且 j>0 时，`dp [i] [j]=dp[0] [j-1]+grid[0] [j]`

    当 i>0 且 j>0 时，`dp[i] [j]=Math.min(dp[i-1] [j],dp[i] [j-1])+grid[i] [j]`

    最后得到 dp[m-1] [n-1] 的值即为从网格左上角到网格右下角的最小路径和。

    ```java
    public int minPathSum(int[][] grid) {
        int[][] dp=new int[grid.length][grid[0].length];
        dp[0][0]=grid[0][0];
        int m=grid.length,n=grid[0].length;
        for (int i = 1; i < m; i++) {
            dp[i][0]=dp[i-1][0]+grid[i][0];
        }
        for (int i = 1; i < n; i++) {
            dp[0][i]=dp[0][i-1]+grid[0][i];
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j]=Math.min(dp[i-1][j],dp[i][j-1])+grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
    ```

29. 爬楼梯(70)：

    方法一：动态规划

    经典的动态规划和递归的问题，比较简单直接贴代码

    ```java
    public int climbStairs(int n) {
        if (n==1){
            return 1;
        }
        int[] dp = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i < n + 1; i++) {
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
    ```

    方法二：矩阵快速幂

    ![image-20220423221916169](image-20220423221916169.png)

    快速幂算法求$M^n$可以使时间复杂度降为O(log n),下面简单介绍一下快速幂算法

    **递归快速幂**

    ![image-20220423230325266](image-20220423230325266.png)

    计算a的n次方，如果n是偶数（不为0），那么就**先计算a的n/2次方，然后平方**；如果n是奇数，那么就**先计算a的n-1次方，再乘上a**；递归出口是**a的0次方为1**。

    递归快速幂的思路非常自然，代码也很简单（直接把递归方程翻译成代码即可）：

    ```java
    //递归快速幂
    public int quickPow(int a, int n)
    {
        if (n == 0)
            return 1;
        else if (n % 2 == 1)
            return qpow(a, n - 1) * a;
        else
        {
            int temp = qpow(a, n / 2);
            return temp * temp;
        }
    }
    ```

    注意，这个temp变量是必要的，因为如果不把quickPow算出的结果记录下来，直接写成quickPow(a, n /2)*quickPow(a, n /2)，那会计算两次a^(n/2)，整个算法就退化为了 O(n)。

    在实际问题中，题目常常会要求对一个大素数取模，这是因为计算结果可能会非常巨大，但是在这里考察高精度又没有必要。这时我们的快速幂也应当进行取模，此时应当注意，原则是**步步取模**，如果MOD较大，还应当**开long long**。

    递归虽然**简洁**，但会产生**额外的空间开销**。我们可以把递归改写为循环，来避免对栈空间的大量占用，也就是**非递归快速幂**。

    **非递归快速幂**

    ```java
    //非递归快速幂
    public int quickPow(int a, int n){
        int ans = 1;
        while(n>0){
            if((n&1)==1) {       //如果n的当前末位为1
                ans *= a;
            }//ans乘上当前的a
            a *= a;        //a自乘
            n >>= 1;       //n往右移一位
        }
        return ans;
    }
    ```

    上面这段代码可以作为非递归快速幂的模板代码，尽量记住

    矩阵的快速幂只需要把上面代码中的乘法改成矩阵乘法，把ans初始化为单位矩阵即可，下面贴上代码

    ```java
    public int[][] matrixMultiply(int[][] a, int[][] b) {
        int[][] c = new int[2][2];
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
            }
        }
        return c;
    }
    
    public int[][] quickPow(int[][] a, int n) {
        int[][] res = {{1, 0}, {0, 1}};//初始化为单位矩阵
        while (n > 0) {
            if ((n & 1) == 1) {
                res = matrixMultiply(res, a);
            }
            a = matrixMultiply(a, a);//a=a*a
            n = n >> 1;
        }
        return res;
    }
    
    public int climbStairs(int n) {
        int[][] matrix = {{1, 1}, {1, 0}};
        int[][] res = quickPow(matrix, n);
        return res[1][0]+res[1][1];
    }
    ```

30. 编辑距离(72)：

    动态规划问题，dp[i] [j]表示word1中长度为i的字符串（下标从 0 到 i-1 ）转换成word2中长度为j的字符串（下标从 0 到 j-1 ）所需要的最小步数。

    状态转移方程：

    当 `word1[i] == word2[j]，dp[i] [j] = dp[i-1] [j-1]`；

    当 `word1[i] != word2[j]，dp[i] [j] = min(dp[i-1] [j-1], dp[i-1] [j], dp[i] [j-1]) + 1`

    其中，`dp[i-1] [j-1]`表示**替换**word1[i-1]为word2[j-1]操作，`dp[i-1] [j]` 表示**删除**word1[i-1]操作，`dp[i] [j-1]` 表示在word1后**插入**word[j-1]操作。三种操作中取步骤数最小的然后加上本次操作。 

    注意，针对第一行，第一列要单独考虑，我们引入 `''` 下图所示：

    ![image-20220424201017152](image-20220424201017152.png)

    第一行，是 `word1` 为空变成 `word2` 最少步数，就是插入操作

    第一列，是 `word2` 为空，需要的最少步数，就是删除操作

    ```java
    public int minDistance(String word1, String word2) {
        //动态规划，dp数组里的i j分别表示word1和word2的长度
        int[][] dp = new int[word1.length() + 1][word2.length() + 1];
        dp[0][0] = 0;
        for (int i = 1; i < word1.length() + 1; i++) {
            dp[i][0] = dp[i - 1][0] + 1;
        }
        for (int i = 1; i < word2.length() + 1; i++) {
            dp[0][i] = dp[0][i - 1] + 1;
        }
        for (int i = 1; i < word1.length() + 1; i++) {
            for (int j = 1; j < word2.length() + 1; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j], Math.min(dp[i][j - 1], dp[i - 1][j - 1])) + 1;
                }
            }
        }
        return dp[word1.length()][word2.length()];
    }
    ```

31. 颜色分类(75)：

    方法一：~~偷鸡做法~~

    因为一共只有三种ya颜色分别是0，1，2，所以遍历一遍原数组找出每种颜色有多少，在按照题目要求的颜色的顺序填回到原数组即可

    ```java
    public void sortColors(int[] nums) {
        int[] colors=new int[3];
        for (int i = 0; i < nums.length; i++) {
            colors[nums[i]]++;
        }
        int indexColors=0,indexRes=0;
        while (indexColors<3){
            while (colors[indexColors]>0){
                nums[indexRes]=indexColors;
                colors[indexColors]--;
                indexRes++;
            }
            indexColors++;
        }
    }
    ```

    方法二：

    直接~~调用排序AP~~I来做就好，自己手写排序算法即可

32. 最小覆盖子串(76)：**滑动窗口**

    本问题要求我们返回字符串 s 中包含字符串 t 的全部字符的最小窗口。我们称包含 t 的全部字母的窗口为「可行」窗口。

    我们可以用滑动窗口的思想解决这个问题。在滑动窗口类型的问题中都会有两个指针，一个用于「延伸」现有窗口的 r 指针，和一个用于「收缩」窗口的 l 指针。在任意时刻，只有一个指针运动，而另一个保持静止。**我们在 s 上滑动窗口，通过移动 r 指针不断扩张窗口。当窗口包含 t 全部所需的字符后，如果能收缩，我们就收缩窗口直到得到最小窗口。如果收缩后不满足条件，则让r指针不断扩张窗口，寻找新的满足条件的滑动窗口。**一直重复此过程直到 j 超出了字符串 S 范围。

    ![image-20220425220202755](image-20220425220202755.png)

    这道题目的解题代码中就用了两个HashMap来实现并维护s中出现的需要的字符数量表（简称已有字符表），实现了一次遍历统计出t中每个字符的数量（简称所需字符表），可以好好领悟一下HashMap的使用

    ```java
    public class MinWindow {
        HashMap<Character, Integer> ori = new HashMap<Character, Integer>();
        HashMap<Character, Integer> cnt = new HashMap<Character, Integer>();
        public String minWindow(String s, String t) {
            if (s==null|| s.equals("") ||t==null||t.equals("")||s.length()<t.length()){
                return "";
            }
            int tLen=t.length();
            //利用HashMap的特点统计t中每个字符出现的次数，getOrDefault经典操作
            for (int i = 0; i < tLen; i++) {
                char c=t.charAt(i);
                ori.put(c, ori.getOrDefault(c,0)+1);
            }
            //l窗口的左边界，r窗口的右边界,r初始化为-1是为了匹配下面while循环一开始就r++，len是窗口的最小长度
            int l=0,r=-1;
            int len=Integer.MAX_VALUE,ansL=-1,ansR=-1;
            int sLen=s.length();
            while (r<sLen){
                r++;
                //向右扩展一格窗口，如果此时右边界的字符是所需要的字符，则更新已有字符表
                if (r<sLen&& ori.containsKey(s.charAt(r))){
                    cnt.put(s.charAt(r),cnt.getOrDefault(s.charAt(r),0)+1);
                }
                //check满足条件后才开始不断收缩左边界直到不满足包含t中所有字符
                //l<=r,窗口会不断收缩l++但l不能超过r可以等于r
                while (check()&&l<=r){
                    if (r-l+1<len){//更新窗口的最小长度
                        len=r-l+1;
                        ansL=l;
                        ansR=r;
                    }
                    //收缩的过程中如果左边界的字符是t所需要的，l++后该字符就没有了，需要更新已有字符表
                    if (ori.containsKey(s.charAt(l))){
                        cnt.put(s.charAt(l),cnt.get(s.charAt(l))-1);
                    }
                    l++;
                }
            }
            return (ansL==-1&&ansR==-1)?"":s.substring(ansL,ansR+1);
        }
        public boolean check(){
            for (Map.Entry<Character, Integer> entry :
                 ori.entrySet()) {
                Character key = entry.getKey();
                Integer value= entry.getValue();
                //s的子串的某些字符的数量大于等于t中每一个字符的数量都是可以的，但如果某一个小于就不符合条件了
                if (cnt.getOrDefault(key,0)<value){
                    return false;
                }
            }
            return true;
        }
    }
    ```

33. 子集(78)：

    方法一：迭代法实现子集枚举

    用01序列表示特征函数，1表示该位置的数在子集里，0表示该位置的数不在子集里，可以发现 01 序列对应的二进制数正好从 0 到 2^n - 1，这样我们可以吗，枚举完所有的情况

    ```java
    public void update(int[] index) {
        for (int i = 0; i < index.length; i++) {//枚举01序列的函数
            if (index[i] == 0) {
                index[i] = 1;
                break;
            } else {
                index[i] = 0;
            }
        }
    }
    
    public List<List<Integer>> subsets(int[] nums) {
        int[] index = new int[nums.length];
        //00 10 01 11
        //000 100 010 110 001 101 011 111
        int cnt = (int) Math.pow(2, nums.length);//计算出需要枚举的次数
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 0; i < cnt; i++) {
            List<Integer> temp = new ArrayList<>();
            for (int j = 0; j < index.length; j++) {
                if (index[j] == 1) {
                    temp.add(nums[j]);
                }
            }
            res.add(temp);
            update(index);
        }
        return res;
    }
    ```

    方法二：递归法实现子集枚举

    对于集合中的每一个数，只有两种情况，选取该数字加入子集中，不选取该数字，和上面的01序列相似

    ```java
    public void dfs(List<List<Integer>> res, List<Integer> temp,int cur,int[] nums){
        if (cur== nums.length){
            res.add(new ArrayList<>(temp));
            return;
        }
        temp.add(nums[cur]);//取当前位置的数字
        dfs(res, temp, cur+1, nums);
        temp.remove(temp.size()-1);//不取当前位置的数字
        dfs(res, temp, cur+1, nums);
    }
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(res,new ArrayList<>(),0,nums);
        return res;
    }
    ```

34. 单词搜索(79)：

    与递归走迷宫类似，从某一个起点出发，递归的向上走，向下走，向左走，向右走，看四个方向的哪个方向可以满足单词的字符，如果都不能满足直接返回false即可，还要注意不能走回头，走过的路不能重复再走一次，需要维护一个`boolean[] isVisited`，访问过就标记为已访问。如果此路不通则要回退为没用访问过

    ```java
    public class Exist {
        public boolean findWord(char[][] board, String word, int x, int y, int wordIndex, boolean[][] isVisited) {
            if (wordIndex == word.length()) {
                return true;//搜索到单词的末尾返回true
            }
            if (x + 1 < board.length && board[x + 1][y] == word.charAt(wordIndex) && !isVisited[x + 1][y]) {
                isVisited[x + 1][y] = true;//先标记为已访问再递归进行搜索
                if (findWord(board, word, x + 1, y, wordIndex + 1, isVisited)) {
                    return true;
                } else {
                    isVisited[x + 1][y] = false;//如果此路不通需要回退为没用访问过
                }
            }
            if (x - 1 >= 0 && board[x - 1][y] == word.charAt(wordIndex) && !isVisited[x - 1][y]) {
                isVisited[x - 1][y] = true;
                if (findWord(board, word, x - 1, y, wordIndex + 1, isVisited)) {
                    return true;
                } else {
                    isVisited[x - 1][y] = false;
                }
            }
            if (y + 1 < board[0].length && board[x][y + 1] == word.charAt(wordIndex) && !isVisited[x][y + 1]) {
                isVisited[x][y + 1] = true;
                if (findWord(board, word, x, y + 1, wordIndex + 1, isVisited)) {
                    return true;
                } else {
                    isVisited[x][y + 1] = false;
                }
            }
            if (y - 1 >= 0 && board[x][y - 1] == word.charAt(wordIndex) && !isVisited[x][y - 1]) {
                isVisited[x][y - 1] = true;
                if (findWord(board, word, x, y - 1, wordIndex + 1, isVisited)) {
                    return true;
                } else {
                    isVisited[x][y - 1] = false;
                }
            }
            return false;
        }
    
        public boolean exist(char[][] board, String word) {
            if (board.length * board[0].length < word.length()) {
                return false;//判断特殊情况直接返回false
            }
            ArrayList<Point> points = new ArrayList<>();
            for (int i = 0; i < board.length; i++) {
                for (int j = 0; j < board[i].length; j++) {
                    if (board[i][j] == word.charAt(0)) {
                        points.add(new Point(i, j));//找到满足第一个字符的所有位置，依次从这些位置开始搜索
                    }
                }
            }
            for (Point start :
                 points) {
                boolean[][] isVisited = new boolean[board.length][board[0].length];
                isVisited[start.x][start.y] = true;//将起始位置标记为已访问，这一步很重要!
                //防止后面递归搜索路径的时候又搜索到开始的位置
                if (findWord(board, word, start.x, start.y, 1, isVisited)) {
                    return true;//有一条路存在直接返回true
                }
            }
            return false;
        }
    }
    
    class Point {
        public int x;
        public int y;
    
        public Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    
    }
    ```

35. 柱状图中最大的矩形(84)：这道题可以好好的理解一下**单调栈**

    方法一：暴力模拟（超时）

    依次遍历柱形的高度，对于每一个高度分别向两边扩散，求出以当前高度为矩形的最大宽度多少。

    为此，我们需要：

    左边看一下，看最多能向左延伸多长，找到大于等于当前柱形高度的最左边元素的下标；

    右边看一下，看最多能向右延伸多长；找到大于等于当前柱形高度的最右边元素的下标。

    对于每一个位置，我们都这样操作，得到一个矩形面积，求出它们的最大值。

    ```java
    public int largestRectangleArea(int[] heights) {
        int len = heights.length;
        if (len == 0) {
            return 0;//特殊情况直接返回
        }
        int res = 0;
        for (int i = 0; i < len; i++) {
            int left = i;
            while (left - 1 >= 0 && heights[left - 1] >= heights[i]) {//先判断前一位是否满足条件再left--
                left--;
            }
            int right = i;
            while (right + 1 < len && heights[right + 1] >= heights[i]) {
                right++;
            }
            int temp = heights[i] * (right - left + 1);
            if (temp > res) {
                res = temp;
            }
        }
        return res;
    }
    ```

    方法二：**单调栈**

    先说明一下单调栈是什么

    - 单调递增栈：从**栈底到栈顶**，栈中的值单调递增
    - 单调递减栈：从**栈底到栈顶**，栈中的值单调递减

    单调栈则主要用于解决**NGE问题**（Next Greater Element），也就是，对序列中每个元素，找到下一个比它大的元素。（当然，“下一个”可以换成“上一个”，“比它大”也可以换成“比他小”，原理不变。）

    我们维护一个栈，表示“**待确定NGE的元素**”，然后遍历序列。当我们碰上一个新元素，我们知道，越靠近栈顶的元素离新元素位置越近。所以不断比较新元素与栈顶，如果新元素比栈顶大，则可断定新元素就是栈顶的NGE，于是弹出栈顶并继续比较。直到新元素不比栈顶大，再将新元素压入栈。显然，这样形成的栈是单调递减的。

    我们归纳一下枚举「高」的方法：

    首先我们枚举某一根柱子 i 作为高 h=heights[i]；

    随后我们需要进行向左右两边扩展，使得**扩展到的柱子的高度均不小于 h**。换句话说，我们需要**找到左右两侧最近的高度小于 h 的柱子**，这样这两根柱子之间（不包括其本身）的所有柱子高度均不小于 h，并且就是 i 能够扩展到的最远范围。

    那么我们先来看看如何求出**一根柱子的左侧且最近的小于其高度的柱子**，可以用单调递增栈来实现，栈中存放的是每根柱子的下标

    栈中存放了 j 值。从栈底到栈顶，j 的值严格单调递增，同时对应的高度值也严格单调递增；

    当我们枚举到第 i 根柱子时，我们从栈顶不断地移除 height[j]≥height[i] 的 j 值。在移除完毕后，栈顶的 j 值就一定满足 height[j]<height[i]，此时 j 就是 i 左侧且最近的小于其高度的柱子，我们再将 i 放入栈顶。

    这里会有一种特殊情况。如果我们移除了栈中所有的 j 值，那就说明 i 左侧所有柱子的高度都大于等于 height[i]，那么我们可以认为 i 左侧且最近的小于其高度的柱子在位置 j=-1，它是一根「虚拟」的、高度无限低的柱子。这样的定义不会对我们的答案产生任何的影响，我们也称这根「虚拟」的柱子为「哨兵」。相对应的最右侧也会有哨兵。

    下面的代码是通过从左向右和从右向左两次遍历分别求出一根柱子的左侧且最近的小于其高度的柱子，一根柱子的右侧且最近的小于其高度的柱子

    ```java
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Deque<Integer> stack = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                stack.pop();
            }
            if (stack.isEmpty()) {
                left[i] = -1;
            } else {
                left[i] = stack.peek();
            }
            stack.push(i);
        }
        stack.clear();
        for (int i = n - 1; i >= 0; i--) {
            while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                stack.pop();
            }
            if (stack.isEmpty()) {
                right[i] = n;
            } else {
                right[i] = stack.peek();
            }
            stack.push(i);
        }
        int res = 0;
        for (int i = 0; i < n; i++) {
            res = Math.max(res, (right[i] - left[i] - 1) * heights[i]);
        }
        return res;
    }
    ```

    除了两次遍历之外，我们还可以对该方法进行优化，只用一次遍历就找出left和right数组.

    在方法一中，我们在对位置 i 进行入栈操作时，确定了它的左边界。从直觉上来说，与之对应的我们在对位置 i 进行出栈操作时可以确定它的右边界！仔细想一想，这确实是对的。当位置 i 被弹出栈时，说明此时遍历到的位置的高度小于等于 height[i]，并且在 i0 与 i 之间没有其他高度小于等于 height[i] 的柱子。这是因为，如果在 i 和i0之间还有其它位置的高度小于等于 height[i] 的，那么在遍历到那个位置的时候，i 应该已经被弹出栈了。所以位置 i0就是位置 i的右边界。

    等等，我们需要的是「一根柱子的左侧（或右侧）且最近的小于其高度的柱子」，但这里我们求的是小于等于，那么会造成什么影响呢？答案是：我们确实无法求出正确的右边界，但对最终的答案没有任何影响。这是因为在答案对应的矩形中，如果有若干个柱子的高度都等于矩形的高度，那么**最右侧的那根柱子是可以求出正确的右边界**的，而我们没有对求出左边界的算法进行任何改动，因此最终的答案还是可以从最右侧的与矩形高度相同的柱子求得的。下面举一个例子来说明这一点

    ![image-20220502225551702](image-20220502225551702.png)

    该算法有两个规则：

    - 当有柱子假设为柱子i入栈时，栈顶的柱子就是柱子i的左边界（因为是单调递增栈）
    - 当有柱子（记为柱子i）出栈时，此时遍历到的柱子（记为index）的高度小于等于柱子i，柱子i的右边界为柱子index

    首先要明确当有柱子出栈和入栈时，可以确定柱子的左边界或右边，还要时刻记得栈中的数据从栈底到栈顶是严格递增的。如上图所示，当我们遍历到柱子4的时候，栈中只有柱子3（因为柱子4最近的小于其高度的柱子是柱子3，如果不明白可以从头开始推），我们将柱子4入栈，那么可以确定柱子4的左边界为柱子3，然后遍历到柱子5，此时柱子5的高度大于等于柱子4，柱子4将出栈，按照规则此时可以确定柱子4的右边界是柱子5（虽然柱子5不是右边最近的小于其高度的柱子，耐心接着往下看），然后柱子5比栈中的柱子3高，柱子5入栈，此时可以确定柱子5的左边界是柱子3，然后接着遍历到了柱子6，柱子6的高度小于此时栈顶柱子（为柱子5）的高度，柱子5出栈，可以确定柱子5的右边界为柱子6，所以柱子5的边界为柱子3和柱子6，虽然柱子4的右边界不正确。至此就可以理解如果有若干个柱子的高度都等于矩形的高度，那么**最右侧的那根柱子是可以求出正确的右边界**的.

    在遍历结束后，栈中仍然有一些位置，这些位置对应的右边界就是位置为 n 的「哨兵」。我们可以将它们依次出栈并更新右边界，也可以在初始化右边界数组时就将所有的元素的值置为 n。

    ```java
    public int largestRectangleArea(int[] heights){
        int n=heights.length;
        int[] left=new int[n];
        int[] right=new int[n];
        Arrays.fill(right,n);//初始化右边界哨兵为n
        Deque<Integer> stack = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                right[stack.peek()]=i;
                stack.pop();
            }
            if (stack.isEmpty()){
                left[i]=-1;
            }else{
                left[i]=stack.peek();
            }
            stack.push(i);
        }
        int res=0;
        for (int i = 0; i < n; i++) {
            res=Math.max(res,(right[i]-left[i]-1)*heights[i]);
        }
        return res;
    }
    ```

36. 最大矩形(85)：

    如果能发现这道题是上一题的一个变种就比较好做，题目要求的是在二维矩阵中找出最大矩形，可以**一行一行的看**，当作是**在一维数组中找到最大的矩形**，如下图所示

    ![image-20220507143621905](image-20220507143621905.png)

    需要注意的是，比如说以第四行为坐标轴的时候，第四行第二列的元素为0，那么这一行的高度就为0，而不要考虑上面有多高，不会出现悬浮的列，这样才能和84题统一。

    ```java
    public int largestRectangleArea(int[] heights){
        int n=heights.length;
        int[] left=new int[n];
        int[] right=new int[n];
        Arrays.fill(right,n);//记得初始化右哨兵为n
        Deque<Integer> stack=new LinkedList<>();//依旧是单调栈解法找出第一个比他小的元素
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                right[stack.pop()]=i;
            }
            if (stack.isEmpty()){
                left[i]=-1;
            }else{
                left[i]=stack.peek();
            }
            stack.push(i);
        }
        int res=0;
        for (int i = 0; i < n; i++) {
            res=Math.max(res,heights[i]*(right[i]-left[i]-1));
        }
        return res;
    }
    public int maximalRectangle(char[][] matrix) {
        if (matrix.length==0){
            return 0;
        }
        int[] heights=new int[matrix[0].length];
        int maxArea=0;
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (matrix[i][j]=='1'){
                    heights[j]+=1;
                }else{
                    heights[j]=0;
                }
            }
            maxArea=Math.max(maxArea,largestRectangleArea(heights));
        }
        return maxArea;
    }
    ```

37. 二叉树的中序遍历(94)：

    简单题，一定要记得二叉树的三种递归遍历方式

38. 不同的二叉搜索树(96)：

    这道题是一个[卡塔兰树](https://baike.baidu.com/item/catalan/7605685?fr=aladdin)问题，~~虽然我不知道卡塔兰树~~

    题目要求是计算不同二叉搜索树的个数。为此，我们可以定义两个函数：

    G(n): 长度为 n 的序列能构成的不同二叉搜索树的个数。

    F(i, n): 以 i 为根、序列长度为 n 的不同二叉搜索树个数 (1≤i≤n)。

    可见，G(n) 是我们求解需要的函数。稍后我们将看到，G(n) 可以从 F(i, n) 得到，而 F(i, n)又会递归地依赖于 G(n)。不同的二叉搜索树的总数 G(n)，是对遍历所有 i (1≤i≤n) 的 F(i, n) 之和。对于边界情况，当序列长度为 1（只有根）或为 0（空树）时，只有一种情况，即：G(0)=1,G(1)=1

    给定序列 1 2 3 ⋯ n，我们选择数字 i 作为根，则根为 i 的所有二叉搜索树的集合是左子树集合和右子树集合的笛卡尔积，对于笛卡尔积中的每个元素，加上根节点之后形成完整的二叉搜索树，如下图所示：

    ![image-20220507211036783](image-20220507211036783.png)

    注意到 G(n)和序列的内容无关，只和序列的长度有关。 因此，我们可以得到以下公式：

    `F(i, n) = G(i-1)*G(n-i)`
    最终G(n)的递归表达式为：

    ![image-20220507211414749](image-20220507211414749.png)

    至此，我们从小到大计算 G函数即可，因为 G(n)的值依赖于 G(0)⋯G(n−1)。

    ```java
    public int numTrees(int n) {
        if (n==0||n==1){
            return 1;
        }
        int[] dp=new int[n+1];
        dp[0]=1;
        dp[1]=1;
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i]+=dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
    ```

39. 验证二叉搜索树(98)：

    方法一：递归

    由题目给出的信息我们可以知道：如果该二叉树的左子树不为空，则左子树上所有节点的值均小于它的根节点的值； 若它的右子树不空，则右子树上所有节点的值均大于它的根节点的值；它的左右子树也为二叉搜索树。

    这启示我们设计一个递归函数 helper(root, lower, upper) 来递归判断，函数表示考虑以 root 为根的子树，判断子树中所有节点的值是否都在 (l,r) 的范围内（注意是开区间）。如果 root 节点的值 val 不在 (l,r) 的范围内说明不满足条件直接返回，否则我们要继续递归调用检查它的左右子树是否满足，如果都满足才说明这是一棵二叉搜索树。

    那么根据二叉搜索树的性质，在递归调用左子树时，我们需要把上界 upper 改为 root.val，即调用 helper(root.left, lower, root.val)，因为左子树里所有节点的值均小于它的根节点的值。同理递归调用右子树时，我们需要把下界 lower 改为 root.val，即调用 helper(root.right, root.val, upper)。

    函数递归调用的入口为 helper(root, -inf, +inf)， inf 表示一个无穷大的值。

    ```java
    public boolean helper(TreeNode node,long low,long high){
        if (node==null){
            return true;
        }
        if (node.val<=low||node.val>=high){
            return false;
        }
        return helper(node.left,low,node.val)&&helper(node.right,node.val,high);
    }
    public boolean isValidBST(TreeNode root) {
        return helper(root,Long.MIN_VALUE,Long.MAX_VALUE);//取为long防止数据过大达到int的最大值
    }
    ```

    还有另一种递归的方式，中序遍历时，判断当前节点是否大于中序遍历的前一个节点，如果大于，说明满足 BST，继续遍历；否则直接返回 false。

    ```java
    long pre = Long.MIN_VALUE;//初始化前驱结点的值为最小值保证根结点处的正确性
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // 访问左子树
        if (!isValidBST(root.left)) {
            return false;
        }
        // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
        if (root.val <= pre) {
            return false;
        }
        pre = root.val;//向右递归时，将此时的结点作为pre结点
        // 访问右子树
        return isValidBST(root.right);
    }
    ```

    方法二：

    二叉搜索树中序遍历得到严格升序的序列，对给定的二叉树中序遍历，结果记录于res之中，检验res是否为严格的升序，若是则为true，反之false。因为中序遍历得到的序列（记为res）一定严格递增，所以如果有一处位置i不满足单调递增，一定有res[i]>=res[i+1]，换句话说就是不满足递增的位置一定是相邻的两个位置，假设位置不相邻，比如序列 i , j , k，res[i]>=res[k]，但是res[j]>=res[i]，所以res[j]>=res[k]，所以一定可以找到一个不满足递增的相邻位置。

    这一点上有时候直觉（直接的感觉）挺有用的，可以先按照直觉写出代码跑一下，如果不正确根据错误用例来改就行

    ```java
    public void inorderTraversal(TreeNode root, ArrayList<Integer> res) {
        if (root.left != null) {
            inorderTraversal(root.left, res);
        }
        res.add(root.val);
        if (root.right != null) {
            inorderTraversal(root.right, res);
        }
    }
    
    public boolean isValidBST(TreeNode root) {
        ArrayList<Integer> res = new ArrayList<>();
        inorderTraversal(root, res);
        Integer[] seq = new Integer[res.size()];
        res.toArray(seq);
        //遍历一次如果有相邻的两个数不满足单调递增直接返回false即可
        for (int i = 0; i < seq.length - 1; i++) {
            if (seq[i] >= seq[i + 1]) {
                return false;
            }
        }
        return true;
    }
    ```

40. 二叉树的层序遍历(102)：广度优先搜索

    首先根元素入队
    当队列不为空的时候

    - 求当前队列的长度 si 
    - 依次从队列中取 si个元素进行拓展，然后进入下一次迭代

    它和普通广度优先搜索的区别在于，普通广度优先搜索每次只取一个元素拓展，而这里**每次取 si个元素**。在上述过程中的第 i 次迭代就得到了二叉树的第 i 层的 si个元素。

    ```java
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) {
            return new ArrayList<>();
        }
        Deque<TreeNode> deque = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        deque.add(root);
        while (deque.size() > 0) {
            int nodeCount = deque.size();
            List<Integer> temp = new ArrayList<>();
            for (int i = 0; i < nodeCount; i++) {//每次取si个元素进行扩展
                TreeNode cur = deque.removeFirst();
                temp.add(cur.val);
                if (cur.left != null) {//左子节点为空或右子节点为空就不要加入队列
                    deque.add(cur.left);
                }
                if (cur.right != null) {
                    deque.add(cur.right);
                }
            }
            res.add(temp);
        }
        return res;
    }
    ```

41. 二叉树的最大深度(104)：

    方法一：深度优先搜索

    如果我们知道了左子树和右子树的最大深度 l 和 r，那么该二叉树的最大深度即为`max(l,r)+1`

    而左子树和右子树的最大深度又可以以同样的方式进行计算。因此我们可以用「深度优先搜索」的方法来计算二叉树的最大深度。具体而言，在计算当前二叉树的最大深度时，可以先递归计算出其左子树和右子树的最大深度，然后在 O(1)时间内计算出当前二叉树的最大深度。递归在访问到空节点时退出。**这是求二叉树深度的经典算法最好背下来**

    ```java
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return Math.max(left, right) + 1;
    }
    ```

    方法二：广度优先搜索

    我们也可以用「广度优先搜索」的方法来解决这道题目，但我们需要对其进行一些修改，此时我们广度优先搜索的队列里存放的是「当前层的所有节点」。每次拓展下一层的时候，不同于广度优先搜索的每次只从队列里拿出一个节点，我们需要将队列里的所有节点都拿出来进行拓展，这样能保证每次拓展完的时候队列里存放的是当前层的所有节点，即我们是一层一层地进行拓展，最后我们用一个变量 ans 来维护拓展的次数，该二叉树的最大深度即为 ans。

    ```java
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Deque<TreeNode> deque = new LinkedList<>();
        deque.add(root);
        int res = 0;
        while (!deque.isEmpty()) {
            int cnt = deque.size();
            res++;
            for (int i = 0; i < cnt; i++) {
                TreeNode cur = deque.removeFirst();
                if (cur.left != null) {
                    deque.add(cur.left);
                }
                if (cur.right != null) {
                    deque.add(cur.right);
                }
            }
        }
        return res;
    }
    ```

42. 从前序与中序遍历序列构造二叉树(105)：

    方法一：递归

    对于任意一颗树而言，前序遍历的形式总是 [ 根节点, [左子树的前序遍历结果], [右子树的前序遍历结果] ]
    即根节点总是前序遍历中的第一个节点。

    而中序遍历的形式总是[ [左子树的中序遍历结果], 根节点, [右子树的中序遍历结果] ]
    只要我们在中序遍历中定位到根节点，那么我们就可以分别知道左子树和右子树中的节点数目。由于同一颗子树的前序遍历和中序遍历的长度显然是相同的，因此我们就可以对应到前序遍历的结果中，对上述形式中的所有左右括号进行定位。

    这样以来，我们就知道了左子树的前序遍历和中序遍历结果，以及右子树的前序遍历和中序遍历结果，我们就可以递归地对构造出左子树和右子树，再将这两颗子树接到根节点的左右位置。

    **细节**

    在中序遍历中对根节点进行定位时，一种简单的方法是直接扫描整个中序遍历的结果并找出根节点，但这样做的时间复杂度较高。我们可以考虑使用哈希表来帮助我们快速地定位根节点。对于哈希映射中的每个键值对，键表示一个元素（节点的值），值表示其在中序遍历中的出现位置。在构造二叉树的过程之前，我们可以对中序遍历的列表进行一遍扫描，就可以构造出这个哈希映射。在此后构造二叉树的过程中，我们就只需要 O(1)的时间对根节点进行定位了。

    ```java
    private HashMap<Integer, Integer> indexMap;
    
    public TreeNode myBuildTree(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
        if (preorder_left > preorder_right) {//说明此时得到的是叶子结点，直接返回null就行
            return null;
        }
        // 前序遍历中的第一个节点就是根节点
        // 在中序遍历中定位根节点
        int inorder_root = indexMap.get(preorder[preorder_left]);
        // 先把根节点建立出来
        TreeNode root = new TreeNode(preorder[preorder_left]);
        // 得到左子树中的节点数目
        int size_left_subtree = inorder_root - inorder_left;
        // 递归地构造左子树，并连接到根节点
        // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
        root.left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
        // 递归地构造右子树，并连接到根节点
        // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
        root.right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
        return root;
    }
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        // 构造哈希映射，帮助我们快速定位根节点
        indexMap = new HashMap<Integer, Integer>();
        for (int i = 0; i < n; i++) {
            indexMap.put(inorder[i], i);
        }
        return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
    }
    ```

    方法二：迭代

    了解一下，这个做法不一定能想得出来[从前序与中序遍历序列构造二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/cong-qian-xu-yu-zhong-xu-bian-li-xu-lie-gou-zao-9/)

43. 二叉树展开为链表(114)：

    方法一：前序遍历

    将二叉树展开为单链表之后，单链表中的节点顺序即为二叉树的前序遍历访问各节点的顺序。因此，可以对二叉树进行前序遍历，获得各节点被访问到的顺序。由于将二叉树展开为链表之后会破坏二叉树的结构，因此在前序遍历结束之后更新每个节点的左右子节点的信息，将二叉树展开为单链表。可以通过递归或迭代的方式实现前序遍历

    ```java
    public void preorder(TreeNode node, ArrayList<Integer> res) {
        res.add(node.val);
        if (node.left != null) {
            preorder(node.left, res);
        }
        if (node.right != null) {
            preorder(node.right, res);
        }
    }
    
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        ArrayList<Integer> res = new ArrayList<>();
        preorder(root, res);
        TreeNode cur = root;
        for (int i = 1; i < res.size(); i++) {
            cur.left = null;
            cur.right = new TreeNode(res.get(i));
            cur = cur.right;
        }
    }
    ```

    方法二：前序遍历和展开同时进行

    使用方法一的前序遍历，由于将节点展开之后会破坏二叉树的结构而丢失子节点的信息，因此前序遍历和展开为单链表分成了两步。能不能在不丢失子节点的信息的情况下，将前序遍历和展开为单链表同时进行？

    之所以会在破坏二叉树的结构之后丢失子节点的信息，是因为在对左子树进行遍历时，没有存储右子节点的信息，在遍历完左子树之后才获得右子节点的信息。只要对前序遍历进行修改，**在遍历左子树之前就获得左右子节点的信息**，并存入栈内，子节点的信息就不会丢失，就可以将前序遍历和展开为单链表同时进行。

    该做法不适用于递归实现的前序遍历，只适用于迭代实现的前序遍历。修改后的前序遍历的具体做法是，每次从栈内弹出一个节点作为当前访问的节点，获得该节点的子节点，如果子节点不为空，则**依次将右子节点和左子节点压入栈内（注意入栈顺序）**。

    展开为单链表的做法是，维护上一个访问的节点 prev，每次访问一个节点时，令当前访问的节点为 curr，将 prev 的左子节点设为 null 以及将 prev 的右子节点设为 curr，然后将 curr 赋值给 prev，进入下一个节点的访问，直到遍历结束。需要注意的是，初始时 prev 为 null，只有在 prev 不为 null 时才能对 prev 的左右子节点进行更新。

    ```java
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        TreeNode pre = null;
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            if (pre != null) {
                pre.left = null;
                pre.right = cur;
            }
            TreeNode left = cur.left, right = cur.right;
            //先压栈的是右节点，然后再压入左节点，出栈是就是先左再右的顺序
            if (right != null) {
                stack.push(right);
            }
            if (left != null) {
                stack.push(left);
            }
            //第一次pre等于null的时候将pre写为cur，不能写成pre=pre.right，第一次pre为null
            pre = cur;
        }
    }
    ```

    方法三：寻找前驱结点

    注意到前序遍历访问各节点的顺序是根节点、左子树、右子树。如果一个节点的左子节点为空，则该节点不需要进行展开操作。如果一个节点的左子节点不为空，则该节点的左子树中的最后一个节点被访问之后，该节点的右子节点被访问。该节点的左子树中最后一个被访问的节点是左子树中的最右边的节点，也是该节点的前驱节点。因此，问题转化成寻找当前节点的前驱节点。找前驱结点在**线索化二叉树**的时候做过，找到叶子结点 的前驱结点和后继结点。

    具体做法是，对于当前节点，如果其左子节点不为空，则在其左子树中找到最右边的节点，作为前驱节点，将当前节点的右子节点赋给前驱节点的右子节点（就是把后继结点挂上去），然后将当前节点的左子节点赋给当前节点的右子节点，并将当前节点的左子节点设为空，（向右展开）。对当前节点处理结束后，继续处理链表中的下一个节点（下一个右子节点），直到所有节点都处理结束。这个地方需要画图好好理解一下。

    ```java
    public void flatten(TreeNode root) {
        TreeNode cur=root;
        while (cur!=null){
            if (cur.left!=null){
                //记录下当前结点的左子结点
                TreeNode next=cur.left;
                TreeNode pre=next;
               //对于当前节点cur，如果其左子节点不为空，则在其左子树中找到最右边的节点，作为前驱节点
                while (pre.right!=null){
                    pre=pre.right;
                }
                //将当前节点的右子节点赋给前驱节点的右子节点（就是把后继结点挂上去）
                pre.right=cur.right;
                //将当前节点的左子节点赋给当前节点的右子节点，并将当前节点的左子节点设为空
                cur.left=null;
                cur.right=next;
            }
            cur=cur.right;
        }
    }
    ```

44. 买卖股票的最佳时机(121)：

    一道简单题，翻译一下就是找到数组中后一个数与前一个数的最大差值（差值必须是真的，如果只能是负的就返回0），可以通过一次遍历来实现这个结果

    ```java
    public int maxProfit(int[] prices) {
        int res=0,buy=prices[0];//买入的价格初始化为第一天的价格，结果初始化为0
        for (int i = 1; i < prices.length; i++) {
            //如果某一天的价格大于买入的价格就尝试卖出，并且和之前卖出的收入进行比较，取收入高的那个
            if (prices[i]>buy){
                res=Math.max(res,prices[i]-buy);
            //如果某天的价格比买入的价格低，那么就应该从这一天买入
            }else if (prices[i]<buy){
                buy=prices[i];
            }
        }
        return res;
    }
    ```

45. 二叉树中的最大路径和(124)：

    这题目的难点在于理解题意和转化题意。

    **思路**

    1. 「可以从任意节点出发, 到达任意节点」 的路径, 
       一定是先上升（ 0 ～ n 个）节点, 到达顶点, 后下降（ 0 ～ n 个）节点。当我们站在顶点的位置来看，就是向左子树走一条路径，向右子树走一条路径，在左子树中不能既向左走又向右走，这样就无法从左子树走回到右子树了（下面还会仔细说明这个地方）。我们可以通过枚举顶点的方式来枚举路径。

    2. 我们枚举顶点时, 可以把路径分拆成3部分： 左侧路径、右侧路径和顶点。
       如下面的路径, 顶点为 20, 左侧路径为 6 -> 15, 右侧为 6 -> 7。

        ``` 
          -10
          /  \
         9   20
            /  \
           15   7
          /    / \
         6    4   6
        ```

       以当前节点为顶点的路径中, 最大和为 两侧路径的最大和 + 节点的值。需要注意的是, 两侧路径也可能不选, 此时取 0

    **实现细节**

    首先，考虑实现一个简化的函数 maxGain(node)，该函数计算二叉树中的一个节点的最大贡献值，具体而言，就是在以该节点为根节点的子树中寻找以该节点为起点的一条路径，使得该路径上的节点值之和最大。

    具体而言，该函数的计算如下。

    - 空节点的最大贡献值等于 0。

    - 非空节点的最大贡献值等于节点值与其子节点中的最大贡献值之和（对于叶节点而言，最大贡献值等于节点值）。

    例如，考虑如下二叉树。

     ```  
        -10
       /   \
      9    20
          /  \
        15    7
     ```

    叶节点 9、15、7 的最大贡献值分别为 9、15、7。

    得到叶节点的最大贡献值之后，再计算非叶节点的最大贡献值。节点 20 的最大贡献值等于 20+max(15,7)=35，节点 -10 的最大贡献值等于 −10+max(9,35)=25。

    上述计算过程是递归的过程，因此，对根节点调用函数 maxGain，即可得到每个节点的最大贡献值。

    再次说明一下这里的最大贡献值，某一个结点的最大贡献值只能取左子树的最大贡献值和右子树的最大贡献值中较大的一个并且和结点的值相加，然后会将该结果返回到递归的上一层，这样对于上一层来看，拿到的就是左子树的最大贡献值，并且可以从左子树走到这一层的顶点，然后再往右子树走

    根据函数 maxGain 得到每个节点的最大贡献值之后，如何得到二叉树的最大路径和？对于二叉树中的一个节点，**该节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值**，如果子节点的最大贡献值为正，则计入该节点的最大路径和，否则不计入该节点的最大路径和。维护一个全局变量 maxSum 存储最大路径和，在递归过程中更新 maxSum 的值，最后得到的 maxSum 的值即为二叉树中的最大路径和。

    ```java
    private int res=Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        if (root==null){
            return 0;//保证传入空结点时的正确性
        }
        maxGain(root);//求出每个结点的最大贡献值，这是个递归的过程
        return res;
    }
    public int maxGain(TreeNode node){
        if (node==null){
            return 0;
        }
        int left=Math.max(maxGain(node.left),0);//左结点的最大贡献值如果小于0就不要贡献了，越加越小肯定是不对的
        int right=Math.max(maxGain(node.right),0);
        res=Math.max(res,node.val+left+right);
        return node.val+Math.max(left,right);//这里体现了这个递归的精髓
        //将该结点与左子树贡献和右子树贡献最大的一个向上返回（只返回左右子树的一边），对于父亲结点（大问题）来看，就是在父结点中找到了加起来的值最大的一条通路
    }
    ```

    **复盘总结递归**
    递归一个树，会对每个子树做同样的事（你写的处理逻辑），所以你需要思考要对每个子树做什么事，即思考子问题是什么，大问题怎么拆解成子问题。
    通过求出每个子树对外提供的最大路径和（返回出来给父节点），从递归树底部向上，不断求出了每个子树内部的最大路径和，后者是求解的目标，它的求解需要前者，搞清楚二者的关系。
    每个子树的内部最大路径和，都挑战一下最大纪录，递归结束时，最大纪录就有了。
    思考递归问题，别纠结细节实现，内部细节是子递归帮你去做的，应结合求解的目标，自顶而下、屏蔽细节地思考，思考递归子问题的定义。随着递归出栈，子问题自下而上地解决，最后解决了整个问题。
    要做的只是**写好递归的处理逻辑，怎么处理当前子树？需要返回什么吗？怎么设置递归的出口？**
    没有思路的时候，试着画画递归树找思路。就算做对了，画图也能加深对递归算法的理解。

    画图的时候可以反过来自底向上的画，先画出最简单的情况，然后看得出简单情况的结果后，如果该情况是一个大问题的子问题，那么该子问题可以怎么运用到大问题的解决当中去

46. 最长连续序列(128)：

    这是一道很好的使用**哈希表**的题目（~~虽然我想到了用哈希表但还是没做出来~~），我们考虑枚举数组中的每个数 x，考虑以其为起点，不断尝试匹配 x+1,x+2,⋯ 是否存在，假设最长匹配到了 x+y，那么以 x 为起点的最长连续序列 x,x+1,x+2,⋯,x+y，其长度为 y+1，我们不断枚举并更新答案即可。

    对于匹配的过程，暴力的方法是 O(n) 遍历数组去看是否存在这个数，但其实更高效的方法是用一个哈希表存储数组中的数，这样查看一个数是否存在即能优化至 O(1) 的时间复杂度。

    仅仅是这样我们的算法时间复杂度最坏情况下还是会达到 O(n^2)（即外层需要枚举 O(n) 个数，内层需要暴力匹配 O(n) 次），无法满足题目的要求。但仔细分析这个过程，我们会发现其中执行了很多不必要的枚举，如果已知有一个 x,x+1,x+2,⋯,x+y 的连续序列，而我们却重新从 x+1，x+2 或者是 x+y 处开始尝试匹配，那么得到的结果肯定不会优于枚举 x为起点的答案，因此我们在外层循环的时候碰到这种情况跳过即可。

    那么怎么判断是否跳过呢？由于我们**要枚举的数 x 一定是在数组中不存在前驱数 x−1 的**，不然按照上面的分析我们会从 x-1 开始尝试匹配，因此我们每次在哈希表中检查是否存在 x-1 即能判断是否需要跳过了。

    ```java
    public int longestConsecutive(int[] nums) {
        HashSet<Integer> numsSet = new HashSet<>();//创建HashSet去重并且实现了HashMap
        for (int num :
             nums) {
            numsSet.add(num);
        }
        int longest=0;
        for (int num :
             numsSet) {
            if (!numsSet.contains(num-1)){//不存在前继才开始向后枚举
                int curLong=1;
                int temp=num+1;
                while (numsSet.contains(temp)){
                    curLong+=1;
                    temp+=1;
                }
                longest=Math.max(longest,curLong);
            }
        }
        return longest;
    }
    ```

47. 只出现一次的数字(136)：

    方法一：利用HashSet，遍历数组，当该数字出现在HashSet中就将HashSet中的该数字删掉，如果不存在就将该数字加入到HashSet中。

    ```java
    public int singleNumber(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int num : nums) {
            if (set.contains(num)) {
                set.remove(num);
            } else {
                set.add(num);
            }
        }
        int res=0;
        for (int num :
             set) {
            res=num;
            break;
        }
        return res;
    }
    ```

    方法二：异或运算

    经典的异或运算的题目，根据异或运算的交换律和结合律，以及下面两条性质`A^A=0, A^0=A`，对数组中的数字一次进行异或运算，最后得到的值就是结果

    ```java
    public int singleNumber(int[] nums){
        int res=nums[0];
        for (int i = 1; i < nums.length; i++) {
            res=res^nums[i];
        }
        return res;
    }
    ```

48. 单词拆分(139)：

    方法一：**动态规划**

    我们定义 dp[i] 表示字符串 s 前 i 个字符组成的字符串 s[0..i−1] 是否能被空格拆分成若干个字典中出现的单词。从前往后计算考虑转移方程，每次转移的时候我们需要枚举包含位置 i−1 的最后一个单词，看它是否出现在字典中以及除去这部分的字符串是否合法即可。公式化来说，我们需要枚举 s[0..i−1] 中的分割点 j ，看 s[0..j−1] 组成的字符串s~1~（默认 j = 0 时 s~1~ 为空串）和  s[j..i−1] 组成的字符串 s~2~ 是否都合法，如果两个字符串均合法，那么按照定义 s~1~ 和 s~2~ 拼接成的字符串也同样合法。由于计算到 dp[i] 时我们已经计算出了 dp[0..i−1] 的值，因此字符串 s~1~ 是否合法可以直接由 dp[j] 得知，剩下的我们只需要看 s~2~ 是否合法即可，因此我们可以得出如下转移方程：
    `dp[i]=dp[j] && check(s[j..i−1])`，其中 check(s[j..i−1]) 表示子串 s[j..i−1] 是否出现在字典中。

    对于检查一个字符串是否出现在给定的字符串列表里一般可以考虑哈希表来快速判断，同时也可以做一些简单的剪枝，枚举分割点的时候倒着枚举，如果分割点 j 到 i 的长度已经大于字典列表里最长的单词的长度，那么就结束枚举，但是需要注意的是下面的代码给出的是不带剪枝的写法。

    对于边界条件，我们定义 dp[0]=true 表示空串且合法。

    ```java
    public boolean wordBreak(String s, List<String> wordDict) {
        HashSet<String> stringHashSet = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && stringHashSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
    ```

49. 环形链表(141)：

    方法一：哈希表

    最容易想到的方法是遍历所有节点，每次遍历到一个节点时，判断该节点此前是否被访问过。

    具体地，我们可以使用哈希表来存储所有已经访问过的节点。每次我们到达一个节点，如果该节点已经存在于哈希表中，则说明该链表是环形链表，否则就将该节点加入哈希表中。重复这一过程，直到我们遍历完整个链表即可。

    ```java
    public boolean hasCycle(ListNode head) {
        HashSet<ListNode> set = new HashSet<>();
        while (head != null) {
            if (set.contains(head)) {
                return true;
            }
            set.add(head);
            head = head.next;
        }
        return false;
    }
    ```

    方法二：快慢指针

    本方法需要对「Floyd 判圈算法」（又称龟兔赛跑算法）有所了解。

    假想「乌龟」和「兔子」在链表上移动，「兔子」跑得快，「乌龟」跑得慢。当「乌龟」和「兔子」从链表上的同一个节点开始移动时，如果该链表中没有环，那么「兔子」将一直处于「乌龟」的前方；如果该链表中有环，那么「兔子」会先于「乌龟」进入环，并且一直在环内移动。等到「乌龟」进入环时，由于「兔子」的速度快，它一定会在某个时刻与乌龟相遇，即套了「乌龟」若干圈。

    我们可以根据上述思路来解决本题。具体地，我们定义两个指针，一快一满。慢指针每次只移动一步，而快指针每次移动两步。初始时，慢指针和慢指针都在位置 head。这样一来，如果在移动的过程中，快指针反过来追上慢指针，就说明该链表为环形链表。否则快指针将到达链表尾部，该链表不为环形链表。

    ```java
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        do {//注意这里只能用do-while循环
            //先进行判断，防止移动快指针的时候出现空指针异常
            if (fast == null || fast.next == null) {
                return false;//fast走到结尾则一定没有环
            }
            slow = slow.next;
            fast = fast.next.next;
        } while (slow != fast);
        return true;//while循环退出说明追上了
    }
    ```

50. 环形链表ⅱ

    方法一：哈希表

    一个非常直观的思路是：我们遍历链表中的每个节点，并将它记录下来；一旦遇到了此前遍历过的节点，就可以判定链表中存在环。借助哈希表可以很方便地实现。代码很简单和上一题差不多，就不放上来了

    方法二：快慢指针

    可以当作一个追及问题来列方程，我们使用两个指针，fast 与 slow。它们起始都位于链表的头部。随后，slow 指针每次向后移动一个位置，而 fast 指针向后移动两个位置。如果链表中存在环，则 fast 指针最终将再次与 slow 指针在环中相遇。

    > 解释一下为何慢指针第一圈没走完一定会和快指针相遇： 首先，第一步，快指针先进入环 第二步：当慢指针刚到达环的入口时，快指针此时在环中的某个位置(也可能此时相遇) 第三步：设此时快指针和慢指针距离为x，若在第二步相遇，则x = 0； 第四步：设环的周长为n，那么看成快指针追赶慢指针，需要追赶n-x； 第五步：快指针每次都追赶慢指针1个单位，设慢指针速度1/s，快指针2/s，那么追赶需要(n-x)s 第六步：在n-x秒内，慢指针走了n-x单位，因为x>=0（x=0会在第二步相遇），则慢指针走的路程小于等于n，即走不完一圈就和快指针相遇

    如下图所示，设链表中环外部分的长度为 a。slow 指针进入环后，又走了 b 的距离与fast 相遇。此时，fast 指针已经走完了环的 n 圈，因此它走过的总距离为 a+n(b+c)+b = a+(n+1)b+nc。

    ![image-20220514215556947](image-20220514215556947.png)

    根据题意，任意时刻，fast 指针走过的距离都为 slow 指针的 2 倍。因此，我们有
    a+(n+1)b+nc=2(a+b)⟹a=c+(n−1)(b+c)

    有了 a=c+(n-1)(b+c) 的等量关系，我们会发现：从相遇点到入环点的距离加上 n−1 圈的环长，恰好等于从链表头部到入环点的距离。

    因此，当发现 slow 与 fast 相遇时，我们再额外使用一个指针 ptr。起始，它指向链表头部；随后，它和 slow 每次向后移动一个位置。最终，它们会在入环点相遇。

    ```java
    public ListNode detectCycle(ListNode head){
        if (head==null){
            return null;
        }
        ListNode slow=head,fast=head;
        while (fast!=null){
            slow=slow.next;
            if (fast.next!=null){//特判fast的next是否为空，防止空指针异常
                fast=fast.next.next;
            }else{
                return null;//说明没有环
            }
            if (fast==slow){
                ListNode ptr=head;
                while (ptr!=slow){
                    ptr=ptr.next;
                    slow=slow.next;
                }
                return ptr;
            }
        }
        return null;//此时fast=null
    }
    ```

51. LRU缓存(146)：

    **哈希表+双向链表**

    很好的一道面向对象的题目，同时也考察了Java集合和一些常用的数据结构

    LRU 缓存机制可以通过哈希表辅以双向链表实现，我们用一个哈希表和一个双向链表维护所有在缓存中的键值对。

    双向链表**按照被使用的顺序**存储了这些键值对，**靠近头部的键值对是最近使用的，而靠近尾部的键值对是最久未使用的。**

    哈希表即为普通的哈希映射（HashMap），**通过缓存数据的键映射到其在双向链表中的位置**。

    这样以来，我们首先使用哈希表进行定位，找出缓存项在双向链表中的位置，随后将其移动到双向链表的头部，即可在 O(1) 的时间内完成 get 或者 put 操作。具体的方法如下：

    对于 get 操作，首先判断 key 是否存在：

    - 如果 key 不存在，则返回 -1；

    - 如果 key 存在，则 key 对应的节点是最近被使用的节点。通过哈希表定位到该节点在双向链表中的位置，并将其移动到双向链表的头部，最后返回该节点的值。

    对于 put 操作，首先判断 key 是否存在：

    - 如果 key 不存在，使用 key 和 value 创建一个新的节点，在双向链表的头部添加该节点，并将 key 和该节点添加进哈希表中。然后判断双向链表的节点数是否超出容量，如果超出容量，则删除双向链表的尾部节点，并删除哈希表中对应的项；

    - 如果 key 存在，则与 get 操作类似，先通过哈希表定位，再将对应的节点的值更新为 value，并将该节点移到双向链表的头部。

    上述各项操作中，访问哈希表的时间复杂度为 O(1)，在双向链表的头部添加节点、在双向链表的尾部删除节点的复杂度也为 O(1)。而将一个节点移到双向链表的头部，可以分成「删除该节点」和「在双向链表的头部添加节点」两步操作，都可以在 O(1) 时间内完成。

    小贴士

    在双向链表的实现中，使用一个伪头部（dummy head）和伪尾部（dummy tail）标记界限，这样在添加节点和删除节点的时候就不需要检查相邻的节点是否存在。==**在链表有关的题目中都可以使用哑结点这个技巧**==

    ```java
    public class LRUCache {
        class LinkedNode {
            int key;
            int value;
            LinkedNode prev;
            LinkedNode next;
    
            public LinkedNode() {
    
            }
    
            public LinkedNode(int key, int value) {
                this.key = key;
                this.value = value;
            }
        }
    
        private HashMap<Integer, LinkedNode> cache = new HashMap<>();
        private int capacity;
        private int size;
        private LinkedNode head, tail;//head和tail是哑结点，删除和插入的时候要注意
    
        public LRUCache(int capacity) {
            this.capacity = capacity;
            this.size = 0;
            head = new LinkedNode();
            tail = new LinkedNode();
            head.next = tail;
            tail.prev = head;
        }
    
        public int get(int key) {
            LinkedNode node = cache.get(key);
            if (node == null) {
                return -1;
            }
            moveToHead(node);
            return node.value;
        }
    
        public void put(int key, int value) {
            LinkedNode node = cache.get(key);
            if (node == null) {
                LinkedNode newNode = new LinkedNode(key, value);
                cache.put(key, newNode);
                addToHead(newNode);
                size++;
                if (size > capacity) {
                    LinkedNode tail = removeTail();
                    cache.remove(tail.key);
                    size--;
                }
            } else {
                node.value = value;
                moveToHead(node);
            }
        }
    
        public void addToHead(LinkedNode node) {
            node.prev = head;
            node.next = head.next;
            head.next.prev = node;
            head.next = node;
        }
    
        public void removeNode(LinkedNode node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
        }
    
        public void moveToHead(LinkedNode node) {
            removeNode(node);
            addToHead(node);
        }
    
        public LinkedNode removeTail() {
            LinkedNode res = tail.prev;
            removeNode(res);
            return res;
        }
    }
    /**
     * Your LRUCache object will be instantiated and called as such:
     * LRUCache obj = new LRUCache(capacity);
     * int param_1 = obj.get(key);
     * obj.put(key,value);
     */
    ```

52. 排序链表(148)：

    方法一：**自顶向下归并排序**

    对链表自顶向下归并排序的过程如下。

    找到链表的中点，以中点为分界，将链表拆分成两个子链表。寻找链表的中点可以使用快慢指针的做法，快指针每次移动 2 步，慢指针每次移动 1 步，当快指针到达链表末尾时，慢指针指向的链表节点即为链表的中点。

    对两个子链表分别排序。

    将两个排序后的子链表合并，得到完整的排序后的链表。可以使用合并两个有序链表的做法，将两个有序的子链表进行合并。

    上述过程可以通过递归实现。递归的终止条件是链表的节点个数小于或等于 1，即当链表为空或者链表只包含 1 个节点时，不需要对链表进行拆分和排序。下面有一个草图帮助理解该算法

    ![image-20220521160851670](image-20220521160851670.png)

    ```java
    public ListNode sortList(ListNode head) {
        return sortList(head,null);//排序是不包含第二个参数tail一起的，前一个参数包含后一个不包含
    }
    public ListNode sortList(ListNode head,ListNode tail){
        if (head==null){
            return head;//如果传入的是null也在这里统一处理了
        }
        if (head.next==tail){
            head.next=null;//把head的next标记为null，做到真正将子链表分开，方便后面找到中点和merge
            return head;
        }
        ListNode slow=head,fast=head;
        while (fast!=tail){
            slow=slow.next;
            fast=fast.next;
            if (fast!=tail){
                fast=fast.next;
            }
        }
        ListNode mid=slow;
        ListNode list1=sortList(head,mid);//排序是不包含第二个参数tail一起的，前一个参数包含后一个不包含
        ListNode list2=sortList(mid,tail);
        return merge(list1,list2);
    }
    
    public ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummy=new ListNode(0);
        ListNode temp=dummy,temp1=head1,temp2=head2;
        while (temp1!=null&&temp2!=null){
            if (temp1.val<=temp2.val){
                temp.next=temp1;
                temp1=temp1.next;
            }else{
                temp.next=temp2;
                temp2=temp2.next;
            }
            temp=temp.next;
        }
        if (temp1!=null){
            temp.next=temp1;
        }else if (temp2!=null){
            temp.next=temp2;
        }
        return dummy.next;
    }
    ```

    方法二：自底向上归并排序

    使用自底向上的方法实现归并排序，则可以达到 O(1) 的空间复杂度。

    首先求得链表的长度length，然后将链表拆分成子链表进行合并。具体做法如下。

    用 subLength 表示每次需要排序的子链表的长度，初始时 subLength=1。

    每次将链表拆分成若干个长度为subLength 的子链表（最后一个子链表的长度可以小于 subLength），按照每两个子链表一组进行合并，合并后即可得到若干个长度为subLength×2 的有序子链表（最后一个子链表的长度可以小于 subLength×2）。合并两个子链表仍然使用 合并两个有序链表的做法。

    将 subLength 的值加倍，重复第 2 步，对更长的有序子链表进行合并操作，直到有序子链表的长度大于或等于 length，整个链表排序完毕。

    ```java
    // 自底向上归并排序
    public ListNode sortList(ListNode head) {
        if(head == null){
            return head;
        }
    
        // 1. 首先从头向后遍历,统计链表长度
        int length = 0; // 用于统计链表长度
        ListNode node = head;
        while(node != null){
            length++;
            node = node.next;
        }
    
        // 2. 初始化 引入dummynode
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
    
        // 3. 每次将链表拆分成若干个长度为subLen的子链表 , 并按照每两个子链表一组进行合并
        for(int subLen = 1;subLen < length;subLen <<= 1){ // subLen每次左移一位（即sublen = sublen*2） PS:位运算对CPU来说效率更高
            ListNode prev = dummyHead;
            ListNode curr = dummyHead.next;     // curr用于记录拆分链表的位置
    
            while(curr != null){               // 如果链表没有被拆完
                // 3.1 拆分subLen长度的链表1
                ListNode head_1 = curr;        // 第一个链表的头 即 curr初始的位置
                for(int i = 1; i < subLen && curr != null && curr.next != null; i++){     // 拆分出长度为subLen的链表1
                    curr = curr.next;
                }
    
                // 3.2 拆分subLen长度的链表2
                ListNode head_2 = curr.next;  // 第二个链表的头  即 链表1尾部的下一个位置
                curr.next = null;             // 断开第一个链表和第二个链表的链接
                curr = head_2;                // 第二个链表头 重新赋值给curr
                for(int i = 1;i < subLen && curr != null && curr.next != null;i++){      // 再拆分出长度为subLen的链表2
                    curr = curr.next;
                }
    
                // 3.3 再次断开 第二个链表最后的next的链接
                ListNode next = null;        
                if(curr != null){
                    next = curr.next;   // next用于记录 拆分完两个链表的结束位置
                    curr.next = null;   // 断开连接
                }
    
                // 3.4 合并两个subLen长度的有序链表
                ListNode merged = mergeTwoLists(head_1,head_2);
                prev.next = merged;        // prev.next 指向排好序链表的头
                while(prev.next != null){  // while循环 将prev移动到 subLen*2 的位置后去
                    prev = prev.next;
                }
                curr = next;              // next用于记录 拆分完两个链表的结束位置
            }
        }
        // 返回新排好序的链表
        return dummyHead.next;
    }
    
    
    // 此处是Leetcode21 --> 合并两个有序链表
    public ListNode mergeTwoLists(ListNode l1,ListNode l2){
        ListNode dummy = new ListNode(0);
        ListNode curr  = dummy;
    
        while(l1 != null && l2!= null){ // 退出循环的条件是走完了其中一个链表
            // 判断l1 和 l2大小
            if (l1.val < l2.val){
                // l1 小 ， curr指向l1
                curr.next = l1;
                l1 = l1.next;       // l1 向后走一位
            }else{
                // l2 小 ， curr指向l2
                curr.next = l2;
                l2 = l2.next;       // l2向后走一位
            }
            curr = curr.next;       // curr后移一位
        }
    
        // 退出while循环之后,比较哪个链表剩下长度更长,直接拼接在排序链表末尾
        if(l1 == null) curr.next = l2;
        if(l2 == null) curr.next = l1;
    
        // 最后返回合并后有序的链表
        return dummy.next; 
    }
    ```

53. 乘积最大子数组(152)：

    方法一：暴力枚举

    直接枚举每个位置开始的所有的乘积，注意只有一个数字的乘积也要参与比较

    ```java
    public int maxProduct(int[] nums) {
        //0 2
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int temp = nums[i];
            if (temp > max) {//只有一个数也要与最大值比较一下
                max = temp;
            }
            for (int j = i + 1; j < nums.length; j++) {
                temp = temp * nums[j];
                if (temp > max) {
                    max = temp;
                }
            }
        }
        return max;
    }
    ```

    方法二：动态规划

    我们可以根据当前位置的正负性进行分类讨论。

    如果当前位置是一个负数的话，那么我们希望以它前一个位置结尾的某个段的积也是个负数，这样就可以负负得正，并且我们希望这个积尽可能「负得更多」，即尽可能小。

    如果当前位置是一个正数的话，我们更希望以它前一个位置结尾的某个段的积也是个正数，并且希望它尽可能地大。

    于是这里我们可以再维护一个它表示以第 i 个元素结尾的最小子数组的乘积，那么我们可以得到这样的动态规划转移方程：

    `max[i]=max(max[i-1] * nums[i],nums[i],min[i-1] * nums[i])`
    `min[i]=min(min[i-1] * nums[i],nums[i],max[i-1] * nums[i])`

    ```java
    public int maxProduct(int[] nums) {
        //    2 -3   2   -3
        //max 2 -3   2   36
        //min 2 -6  -12  -6
        //max[i]=max(max[i-1]*nums[i],nums[i],min[i-1]*nums[i])
        //min[i]=min(min[i-1]*nums[i],nums[i],max[i-1]*nums[i])
        //max[i]表示以元素nums[i]结尾的序列的乘积的最大值
        int[] max = new int[nums.length];
        int[] min = new int[nums.length];
        max[0] = nums[0];
        min[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            max[i] = Math.max(Math.max(max[i - 1] * nums[i], nums[i]), min[i - 1] * nums[i]);
            min[i] = Math.min(Math.min(min[i - 1] * nums[i], nums[i]), max[i - 1] * nums[i]);
        }
        //结果需要遍历以数组中每一个元素作为结尾的所有情况找出最大的
        int res = max[0];
        for (int i = 1; i < max.length; i++) {
            if (max[i] > res) {
                res = max[i];
            }
        }
        return res;
    }
    ```

54. 最小栈（155）：

    用**辅助栈**来实现（第一次做这题真没想到这种做法）

    要做出这道题目，首先要理解栈结构**后进先出**的性质。

    对于栈来说，如果一个元素 a 在入栈时，栈里有其它的元素 b, c, d，那么无论这个栈在之后经历了什么操作，只要 a 在栈中，b, c, d 就一定在栈中，因为在 a 被弹出之前，b, c, d 不会被弹出。

    因此，在操作过程中的任意一个时刻，只要栈顶的元素是 a，那么我们就可以确定栈里面现在的元素（从栈顶到栈底）一定是 a, b, c, d。

    那么，我们可以在每个元素 a 入栈时把当前栈的最小值 m 存储起来。在这之后无论何时，如果栈顶元素是 a，我们就可以直接返回存储的最小值 m。

    我们可以使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值。

    - 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；

    - 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；

    - 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中。

    ```java
    public class MinStack {
        private Deque<Integer> stack;
        private Deque<Integer> minStack;
    
        public MinStack() {
            stack=new LinkedList<>();
            minStack=new LinkedList<>();
            minStack.push(Integer.MAX_VALUE);//先放入最大值做为标记
        }
    
        public void push(int val) {
            int min=minStack.peek();
            minStack.push(Math.min(val, min));//minStack中放入当前元素与栈顶元素较小的那个
            stack.push(val);
        }
    
        public void pop() {
            stack.pop();//同时弹出
            minStack.pop();
        }
    
        public int top() {
            return stack.peek();
        }
    
        public int getMin() {
            return minStack.peek();
        }
    }
    ```

55. 相交链表（160）：

    方法一：HashSet

    判断两个链表是否相交，可以使用哈希集合存储某一条链表的所有节点。

    首先遍历链表 headA，并将链表 headA 中的每个节点加入哈希集合中。然后遍历链表 headB，对于遍历到的每个节点，判断该节点是否在哈希集合中：

    如果当前节点不在哈希集合中，则继续遍历下一个节点；

    如果当前节点在哈希集合中，则后面的节点都在哈希集合中，即从当前节点开始的所有节点都在两个链表的相交部分，因此在链表 headB 中遍历到的第一个在哈希集合中的节点就是两个链表相交的节点，返回该节点。

    如果链表 headB 中的所有节点都不在哈希集合中，则两个链表不相交，返回 null。

    ```java
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashSet<ListNode> hashSet = new HashSet<>();
        while (headA != null) {
            hashSet.add(headA);
            headA = headA.next;
        }
        while (headB != null) {
            if (hashSet.contains(headB)) {
                return headB;
            }
            headB = headB.next;
        }
        return null;
    }
    ```

    方法二：双指针

    根据题目意思，如果两个链表相交，那么相交点之后的长度是相同的。

    我们需要做的事情是，让**两个链表从同距离末尾同等距离的位置开始遍历**。为此，我们必须**消除两个链表的长度差**。有一个重要的前提是，如果让指针a遍历完A链表后再遍历B链表，让指针b遍历完B链表后再遍历A链表，这样两个指针最后一定会走过相同的距离，如果两个链表相交两个指针就一定会落在同一个结点。

    下面是算法的具体步骤：

    - 当链表 headA 和 headB 都不为空时，创建两个指针 pA 和 pB，初始时分别指向两个链表的头节点 headA 和 headB
    - 每步操作需要同时更新指针 pA 和 pB。
    - 如果指针 pA 不为空，则将指针 pA 移到下一个节点；如果指针 pB 不为空，则将指针 pB 移到下一个节点。
    - 如果指针 pA 为空，则将指针 pA 移到链表 headB 的头节点；如果指针 pB 为空，则将指针 pB 移到链表 headA 的头节点。
    - 当指针 pA 和 pB 指向同一个节点或者都为空时，返回它们指向的节点或者 null。

    下面提供双指针方法的正确性证明。考虑两种情况，第一种情况是两个链表相交，第二种情况是两个链表不相交。

    情况一：两个链表相交

    链表headA 和 headB 的长度分别是 m 和 n。假设链表 headA 的不相交部分有 a 个节点，链表 headB 的不相交部分有 b 个节点，两个链表相交的部分有 c 个节点，则有 a+c=m，b+c=n。

    如果 a=b，则两个指针会同时到达两个链表相交的节点，此时返回相交的节点；

    如果 a != b，则指针 pA 会遍历完链表 headA，指针 pB 会遍历完链表 headB，两个指针不会同时到达链表的尾节点，然后指针 pA 移到链表 headB 的头节点，指针 pB 移到链表 headA 的头节点，然后两个指针继续移动，在指针 pA 移动了 a+c+b 次、指针 pB 移动了 b+c+a 次之后，两个指针会同时到达两个链表相交的节点，该节点也是两个指针第一次同时指向的节点，此时返回相交的节点。

    ![image-20220521220004531](image-20220521220004531.png)

    情况二：两个链表不相交

    链表 headA 和 headB 的长度分别是 m 和 n。考虑当 m=n 和 m!=n  时，两个指针分别会如何移动：

    如果 m=n，则两个指针会同时到达两个链表的尾节点，然后同时变成空值null，此时返回 null；

    如果 m != n ，则由于两个链表没有公共节点，两个指针也不会同时到达两个链表的尾节点，因此两个指针都会遍历完两个链表，在指针 pA 移动了 m+n 次、指针 \pB 移动了 n+m 次之后，两个指针会同时指向链表末尾变成空值 null，此时返回 null。

    ```java
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode pa=headA,pb=headB;
        while (pa!=pb){
            pa=pa==null?headB:pa.next;
            pb=pb==null?headA:pb.next;
        }
        return pb;
    }
    ```

56. 多数元素（169）：

    方法一：哈希表

    我们使用哈希映射（HashMap）来存储每个元素以及出现的次数。对于哈希映射中的每个键值对，键表示一个元素，值表示该元素出现的次数。

    我们用一个循环遍历数组 nums 并将数组中的每个元素加入哈希映射中。在这之后，我们遍历哈希映射中的所有键值对，找到出现次数大于n/2的键即可。

    ```java
    public int majorityElement(int[] nums) {
        HashMap<Integer, Integer> hashMap = new HashMap<>();
        for (int num : nums) {
            int cnt = hashMap.getOrDefault(num, 0);
            hashMap.put(num, cnt + 1);
        }
        int res = 0;
        for (Map.Entry<Integer, Integer> entry :
             hashMap.entrySet()) {
            if (entry.getValue() > nums.length / 2) {
                res = entry.getKey();
                break;
            }
        }
        return res;
    }
    ```

    方法二：排序

    如果将数组 nums 中的所有元素按照单调递增或单调递减的顺序排序，那么下标为 n/2 的元素（下标从 0 开始）一定是众数。

    算法

    对于这种算法，我们先将 nums 数组排序，然后返回上文所说的下标对应的元素。下面的图中解释了为什么这种策略是有效的。在下图中，第一个例子是 n 为奇数的情况，第二个例子是 n 为偶数的情况。

    ![image-20220521223252489](image-20220521223252489.png)

    对于每种情况，数组下面的线表示如果众数是数组中的最小值时覆盖的下标，数组上面的线表示如果众数是数组中的最大值时覆盖的下标。对于其他的情况，这条线会在这两种极端情况的中间。对于这两种极端情况，它们会在下标为 n/2 的地方有重叠。因此，无论众数是多少，返回 n/2 下标对应的值都是正确的。

    ```java
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }
    ```

    方法三：[Boyer-Moore 投票算法](https://leetcode.cn/problems/majority-element/solution/duo-shu-yuan-su-by-leetcode-solution/)

    注意看题解的评论，有指出官方题解错误的地方

57. 打家劫舍（198）：

    经典**动态规划问题**，可以好好领悟一下。

    动态规划的的四个解题步骤是：

    - 定义子问题
    - 写出子问题的递推关系
    - 确定 DP 数组的计算顺序
    - 空间优化（可选）

    步骤一：定义子问题
    稍微接触过一点动态规划的朋友都知道动态规划有一个“子问题”的定义。什么是子问题？子问题是和原问题相似，但规模较小的问题。例如这道小偷问题，原问题是“从全部房子中能偷到的最大金额”，将问题的规模缩小，子问题就是“从 k 个房子中能偷到的最大金额”，用 f(k) 表示。

    可以看到，子问题是参数化的，我们定义的子问题中有参数 k。假设一共有 n 个房子的话，就一共有 n 个子问题。动态规划实际上就是通过求这一堆子问题的解，来求出原问题的解。这要求子问题需要具备两个性质：

    原问题要能由子问题表示。例如这道小偷问题中，k=n 时实际上就是原问题。否则，解了半天子问题还是解不出原问题，那子问题岂不是白解了。
    一个子问题的解要能通过其他子问题的解求出。例如这道小偷问题中，f(k) 可以由 f(k-1) 和 f(k-2) 求出，具体原理后面会解释。这个性质就是教科书中所说的“最优子结构”。如果定义不出这样的子问题，那么这道题实际上没法用动态规划解。
    小偷问题由于比较简单，定义子问题实际上是很直观的。一些比较难的动态规划题目可能需要一些定义子问题的技巧。

    步骤二：写出子问题的递推关系

    这一步是求解动态规划问题最关键的一步。然而，这一步也是最无法在代码中体现出来的一步。在做题的时候，最好把这一步的思路用注释的形式写下来。做动态规划题目不要求快，而要确保无误。否则，写代码五分钟，找 bug 半小时，岂不美哉？

    我们来分析一下这道小偷问题的递推关系：

    首先考虑最简单的情况。如果只有一间房屋，则偷窃该房屋，可以偷窃到最高总金额。如果只有两间房屋，则由于两间房屋相邻，不能同时偷窃，只能偷窃其中的一间房屋，因此选择其中金额较高的房屋进行偷窃，可以偷窃到最高总金额。

    如果房屋数量大于两间，应该如何计算能够偷窃到的最高总金额呢？对于第k (k>2) 间房屋，有两个选项：

    - 偷窃第 k 间房屋，那么就不能偷窃第 k-1 间房屋，偷窃总金额为前 k−2 间房屋的最高总金额与第 k 间房屋的金额之和。

    - 不偷窃第 k 间房屋，偷窃总金额为前 k-1 间房屋的最高总金额。

    在两个选项中选择偷窃总金额较大的选项，该选项对应的偷窃总金额即为前 k 间房屋能偷窃到的最高总金额。

    这时我们就可以写出状态转移方程了：`dp[i]=max(dp[i-2]+nums[i],dp[i-1])`，注意边界条件只有一间房屋和两间房屋的情况

    ```java
    public int rob(int[] nums) {
        if (nums.length==1){//特殊情况判断
            return nums[0];
        }
        int[] dp=new int[nums.length];
        dp[0]=nums[0];
        dp[1]=Math.max(nums[0],nums[1]);
        for (int i = 2; i < nums.length; i++) {
            dp[i]=Math.max(dp[i-2]+nums[i],dp[i-1]);
        }
        return dp[nums.length-1];
    }
    ```

58. 岛屿数量(200)：

    方法一：广度优先搜索

    为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 1，则将其加入队列，开始进行广度优先搜索。在广度优先搜索的过程中，**每个搜索到的 1 都会被重新标记为 0**，这样就相当于把岛屿所联通的区域清除，表示已经搜索过了（标记为2是更好的做法，更能区分出这是已经遍历过的位置），不会干扰后面接着扫描二维网络。直到队列为空，搜索结束。

    最终岛屿的数量就是我们进行广度优先搜索的次数。

    ```java
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        int res = 0;
        //t=x*column+y,注意一下这里的写法，将二维的信息保存在一维中，
        //因为后面的y一定小于column，所以t%column可以正确求出y,t/column求出x
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    res++;
                    ArrayList<Integer> island = new ArrayList<>();
                    island.add(i * n + j);
                    while (island.size() > 0) {
                        Integer cur = island.remove(0);
                        int x = cur / n, y = cur % n;
                        grid[x][y] = '0';
                        if (x + 1 < m && grid[x + 1][y] == '1') {
                            grid[x + 1][y] = '0';
                            island.add((x + 1) * n + y);
                        }
                        if (x - 1 >= 0 && grid[x - 1][y] == '1') {
                            grid[x - 1][y] = '0';
                            island.add((x - 1) * n + y);
                        }
                        if (y + 1 < n && grid[x][y + 1] == '1') {
                            grid[x][y + 1] = '0';
                            island.add(x * n + y + 1);
                        }
                        if (y - 1 >= 0 && grid[x][y - 1] == '1') {
                            grid[x][y - 1] = '0';
                            island.add(x * n + y - 1);
                        }
                    }
                }
            }
        }
        return res;
    }
    ```

    方法二：深度优先搜索

    我们可以将二维网格看成一个无向图，竖直或水平相邻的 1 之间有边相连。

    为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 1，则以其为起始节点开始进行深度优先搜索。在深度优先搜索的过程中，每个搜索到的 1 都会被重新标记为 0，表示已经搜索过了。

    最终岛屿的数量就是我们进行深度优先搜索的次数。

    ```java
    public void dfs(int x, int y, char[][] grid) {
        if (x >= grid.length || x < 0 || y >= grid[0].length || y < 0 || grid[x][y] == '0') {
            return;
        }
        grid[x][y] = '0';
        dfs(x + 1, y, grid);
        dfs(x - 1, y, grid);
        dfs(x, y + 1, grid);
        dfs(x, y - 1, grid);
    }
    
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }
        int res = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    res++;
                    dfs(i, j, grid);
                }
            }
        }
        return res;
    }
    ```

59. 翻转链表(206)：

    简单题，可以复习一下链表，做链表题目的时候先画图，重点就是把图画出来按着图去做

    ```java
    public ListNode reverseList(ListNode head) {
        if (head==null){
            return null;
        }
        ListNode cur=head;
        head=head.next;
        cur.next=null;
        while (head!=null){
            ListNode temp=head.next;
            head.next=cur;
            cur=head;
            head=temp;
        }
        return cur;
    }
    ```

60. 课程表(207)：

    本题是一道经典的「拓扑排序」问题。

    给定一个包含 n 个节点的有向图 G，我们给出它的节点编号的一种排列，如果满足：对于图 G 中的任意一条有向边 (u, v)，u 在排列中都出现在 v 的前面。那么称该排列是图 G 的「拓扑排序」。根据上述的定义，我们可以得出两个结论：

    如果图 G 中存在环（即图 G 不是「有向无环图」），那么图 G 不存在拓扑排序。

    如果图 G 是有向无环图，那么它的拓扑排序可能不止一种。举一个最极端的例子，如果图 G 值包含 n 个节点却没有任何边，那么任意一种编号的排列都可以作为拓扑排序。

    有了上述的简单分析，我们就可以将本题建模成一个求拓扑排序的问题了：

    我们将每一门课看成一个节点；

    如果想要学习课程 A 之前必须完成课程 B，那么我们从 B 到 A 连接一条有向边。这样以来，在拓扑排序中，B 一定出现在 A 的前面。

    求出该图是否存在拓扑排序，就可以判断是否有一种符合要求的课程学习顺序

    方法一：深度优先搜索

    **思路**

    我们可以将深度优先搜索的流程与**拓扑排序**(离散学过)的求解联系起来，用一个栈来存储所有已经搜索完成的节点。

    对于一个节点 u，如果它的所有相邻节点都已经搜索完成，那么在搜索回溯到 u 的时候，u 本身也会变成一个已经搜索完成的节点。这里的「相邻节点」指的是从 u 出发通过一条有向边可以到达的所有节点。

    假设我们当前搜索到了节点 u，如果它的所有相邻节点都已经搜索完成，那么这些节点都已经在栈中了，此时我们就可以把 u 入栈。可以发现，如果我们从栈顶往栈底的顺序看，由于 u 处于栈顶的位置，那么 u 出现在所有 u 的相邻节点的前面。因此对于 u 这个节点而言，它是满足拓扑排序的要求的。

    这样以来，我们对图进行一遍深度优先搜索。当每个节点进行回溯的时候，我们把该节点放入栈中。最终从栈顶到栈底的序列就是一种拓扑排序。

    **算法**

    对于图中的任意一个节点，它在搜索的过程中有三种状态，即：

    「未搜索」：我们还没有搜索到这个节点；

    「搜索中」：我们搜索过这个节点，但还没有回溯到该节点，即该节点还没有入栈，还有相邻的节点没有搜索完成）；

    「已完成」：我们搜索过并且回溯过这个节点，即该节点已经入栈，并且所有该节点的相邻节点都出现在栈的更底部的位置，满足拓扑排序的要求。

    通过上述的三种状态，我们就可以给出使用深度优先搜索得到拓扑排序的算法流程，在每一轮的搜索搜索开始时，我们任取一个「未搜索」的节点开始进行深度优先搜索。

    我们将当前搜索的节点 u 标记为「搜索中」，遍历该节点的每一个相邻节点 v：

    如果 v 为「未搜索」，那么我们开始搜索 v，待搜索完成回溯到 u；

    如果 v 为「搜索中」，那么我们就找到了图中的一个环，也就是递归搜索的时候遇到了之前搜索过的点，所以存在环，因此是不存在拓扑排序的；

    如果 v 为「已完成」，那么说明 v 已经在栈中了，而 u 还不在栈中，因此 u 无论何时入栈都不会影响到 (u, v)之前的拓扑关系，以及不用进行任何操作。

    当 u 的所有相邻节点都为「已完成」时，我们将 u 放入栈中，并将其标记为「已完成」。

    在整个深度优先搜索的过程结束后，如果我们没有找到图中的环，那么栈中存储这所有的 n 个节点，从栈顶到栈底的顺序即为一种拓扑排序。

    ```java
    // 存储有向图，存的是每一个顶点可以到的所有其他顶点，这样来表示边
    List<List<Integer>> edges;
    // 标记每个节点的状态：0=未搜索，1=搜索中，2=已完成
    int[] visited;
    // 判断有向图中是否有环
    boolean valid = true;
    
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        edges = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            edges.add(new ArrayList<>());
        }
        visited = new int[numCourses];
        for (int[] prereq :
             prerequisites) {
            //要学0 先学1，所以存在从1到0的边，get(1).add(0)
            edges.get(prereq[1]).add(prereq[0]);
        }
        // 每次挑选一个「未搜索」的节点，开始进行深度优先搜索
        for (int i = 0; i < numCourses && valid; i++) {
            if (visited[i]==0){
                dfs(i);
            }
        }
        return valid;
    }
    
    private void dfs(int u) {
        //先将u标记为搜索中
        visited[u]=1;
        for (int v :
             edges.get(u)) {
            // 如果「未搜索」那么搜索相邻节点v
            if (visited[v]==0) {
                dfs(v);
                //剪枝，搜v结点已经有环了，就不用在搜索和u相邻的其他顶点了
                if (!valid){
                    return;
                }
            }else if (visited[v]==1){
                valid=false;
                return;
            }
        }
        //u的所有相邻顶点搜索完后，再将u标记为已完成
        visited[u]=2;
    }
    ```

    方法二：广度优先搜索

    **思路**

    方法一的深度优先搜索是一种「逆向思维」：最先被放入栈中的节点是在拓扑排序中最后面的节点。我们也可以使用正向思维，顺序地生成拓扑排序，这种方法也更加直观。

    我们考虑拓扑排序中最前面的节点，**该节点一定不会有任何入边，也就是它没有任何的先修课程要求**。当我们将一个节点加入答案中后，我们就可以移除它的所有出边，代表着**它的相邻节点少了一门先修课程的要求**。如果某个相邻节点变成了「没有任何入边的节点」，那么就代表着这门课可以开始学习了。按照这样的流程，我们不断地将没有入边的节点加入答案，直到答案中包含所有的节点（得到了一种拓扑排序）或者不存在没有入边的节点（图中包含环）。

    上面的想法类似于广度优先搜索，因此我们可以将广度优先搜索的流程与拓扑排序的求解联系起来。

    **算法**

    我们使用一个队列来进行广度优先搜索。开始时，所有入度为 0 的节点都被放入队列中，他们没有先修课程的要求，可以直接学，它们就是可以作为拓扑排序最前面的节点，并且它们之间的相对顺序是无关紧要的。

    在广度优先搜索的每一步中，我们取出队首的节点 u：

    我们将 u 放入答案中；

    我们移除 u 的所有出边，也就是将 u 的所有相邻节点的入度减少 1。如果某个相邻节点 v 的入度变为 0，那么我们就将 v 放入队列中。

    在广度优先搜索的过程结束后。如果答案中包含了这 n 个节点，那么我们就找到了一种拓扑排序，否则说明图中存在环，也就不存在拓扑排序了。

    ```java
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> edges = new ArrayList<>();
        int[] indeg = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            edges.add(new ArrayList<>());
        }
        for (int[] info :
             prerequisites) {
            //从1出发指向0的边
            edges.get(info[1]).add(info[0]);
            indeg[info[0]]++;
        }
        Deque<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            // 将所有入度为 0 的节点放入队列中，意味着队列中的课可以先学不需要先修课程
            if (indeg[i] == 0) {
                queue.add(i);
            }
        }
        int res = 0;
        while (!queue.isEmpty()) {
            int u = queue.removeFirst();
            res++;
            for (int v :
                 edges.get(u)) {
                //将u的相邻结点的入度减少一
                indeg[v]--;
                // 如果相邻节点 v 的入度为 0，就可以选 v 对应的课程了
                if (indeg[v] == 0) {
                    queue.add(v);
                }
            }
        }
        return res == numCourses;
    }
    ```

    **总结**：深度优先搜索的时候做标记一定要先看到底有几种标记，想好每一种标记在什么时候打上，这和实际问题有关，但模板都差不多，这个题对图的这种存储方式很值得学习，用`List<List<Integer>> edges = new ArrayList<>();`存储边，edges.get(0)意味着拿到从顶点0出发可以到达的所有点的一个List

61. 数组中的第k个最大元素(215)：

    我直接排序后（升序）从后往前找出第k个就好了，想了解快速选择算法去看看[官方题解](https://leetcode.cn/problems/kth-largest-element-in-an-array/solution/shu-zu-zhong-de-di-kge-zui-da-yuan-su-by-leetcode-/)，这题也可以用堆排序来做，建立大顶堆，做 k - 1 次删除堆顶的操作后堆顶元素就是我们要找的答案

62. 会议室2：

    ![image-20220620111447008](image-20220620111447008.png)

    **思路**：

    先对所有的会议安排按照开始时间升序排列。

    安排第一个会议，此时一个会议室都没有，直接开放一间会议室使用；

    安排第 i 个会议，查看当前有没有会议室是已开放且空闲的，没有则接着开放新的会议室；

    查看是否有会议室已开放且空闲，是看当前正在使用会议室的会议中，**最早结束的那场会议的结束时间**，如果现在还没结束，说明其他会议更不可能结束，直接开放新的会议室。

    若在已开放的会议室中，最早结束的那场会议已经结束，说明现在存在空闲会议室，直接加入即可。

    **算法**

    1、将最先开始的会议的结束时间加入小顶堆，最先开始的会议肯定要先进行

    2、接着对 [1, size-1] 个会议依次进行操作：对比当前会议的开始时间和小顶堆的堆顶元素值，若小于，说明当前所有会议室正在进行的会议中，最早结束的都还没结束，只能新建会议室了，也就是将其加入小顶堆；

    3、若当前会议的开始时间大于等于小顶堆的堆顶元素值，说明会议室正在进行的会议中，最早结束的会议已经结束，可以把它从小顶堆删除，自己进入小顶堆（重复利用会议室）。

    4、等最后一个会议时间进入小顶堆，此时的小顶堆元素个数即至少需要的会议室数量。

    ```java
    public int minMeetingRooms(int[][] intervals) {
        if (intervals == null || intervals.length == 0) {
            return 0;
        }
        PriorityQueue<Integer> heap = new PriorityQueue<>((o1, o2) -> o1 - o2);//小顶堆
        Arrays.sort(intervals, (o1, o2) -> o1[0] - o2[0]);//按照会议开始时间升序排列
        //将最早开始的会议的结束时间加入小顶堆，最早开始的会议肯定要先安排会议室
        heap.add(intervals[0][1]);
        // 小顶堆中堆顶始终是最早结束的会议的时间
        for (int i = 1; i < intervals.length; i++) {
            // 如果当前会议的开始时间大于等于前面已经开始的会议中最早结束的时间
            // 说明有会议室空闲出来了，可以直接重复利用
            // 把堆顶会议删除，当前的会议结束时间加入堆，意味着当前会议在进行
            if (intervals[i][0] >= heap.peek()) {
                heap.poll();
            }
            //把当前会议的结束时间加入小顶堆
            heap.add(intervals[i][1]);
        }
        // 当所有会议遍历完毕，还在最小堆里面的，说明会议还没结束，此时的数量就是会议室的最少数量
        return heap.size();
    }
    ```

63. 最大正方形：

    方法一：暴力模拟

    遍历矩阵中的每个元素，每次遇到 1，则将该元素作为正方形的左上角；

    确定正方形的左上角后，根据**左上角所在的行和列计算可能的最大正方形的边长**（正方形的范围不能超出矩阵的行数和列数），在该边长范围内寻找只包含 1 的最大正方形；

    每次在**下方新增一行**以及在**右方新增一列**，判断新增的行和列是否满足所有元素都是 1。

    ```java
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        int maxSide = 0, m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (matrix[i][j] == '1') {
                    maxSide = Math.max(maxSide, 1);
                    // 计算可能的最大正方形边长
                    int curMax = Math.min(m - i, n - j);
                    for (int k = 1; k < curMax; k++) {
                        boolean flag = true;
                        //新增的一行一列的右下角那个特判
                        if (matrix[i + k][j + k] == '0') {
                            break;
                        }
                        for (int l = 0; l < k; l++) {
                            //判断新增的一行一列是不是都是1
                            if (matrix[i + k][j + l] == '0' 
                                || matrix[i + l][j + k] == '0') {
                                flag = false;
                                break;
                            }
                        }
                        if (flag) {
                            maxSide = Math.max(maxSide, k + 1);
                        } else {
                            break;
                        }
                    }
                }
            }
        }
        return maxSide * maxSide;
    }
    ```

    方法二：动态规划

    可以使用动态规划降低时间复杂度。我们用 dp(i,j) 表示以 (i, j) 为右下角，且只包含 1 的正方形的边长最大值。如果我们能计算出所有 dp(i,j) 的值，那么其中的最大值即为矩阵中只包含 1 的正方形的边长最大值，其平方即为最大正方形的面积。

    那么如何计算 dp 中的每个元素值呢？对于每个位置 (i,j)，检查在矩阵中该位置的值：

    如果该位置的值是 0，则 dp(i,j)=0，因为当前位置不可能在由 1 组成的正方形中；

    如果该位置的值是 1，则 dp(i,j) 的值由其上方、左方和左上方的三个相邻位置的 dp 值决定。具体而言，当前位置的元素值等于三个相邻位置的元素中的最小值加 1，状态转移方程如下：

    `dp(i, j)=min(dp(i−1, j), dp(i−1, j−1), dp(i, j−1))+1`

    此外，还需要考虑边界条件。如果 i 和 j 中至少有一个为 0，则以位置 (i,j) 为右下角的最大正方形的边长只能是 1，因此 dp(i,j)=1。

    该状态转移方程的推导可以看看[官方题解](https://leetcode.cn/problems/count-square-submatrices-with-all-ones/solution/tong-ji-quan-wei-1-de-zheng-fang-xing-zi-ju-zhen-2/)，看懂后该类正方形的问题应该都能明白

    ```java
    public int maximalSquare(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        int maxSide = 0;
        int rows = matrix.length, column = matrix[0].length;
        int[][] dp = new int[rows][column];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < column; j++) {
                if (matrix[i][j] == '1') {
                    if (i == 0 || j == 0) {
                        dp[i][j] = 1;
                    } else {
                        dp[i][j] = Math.min(dp[i - 1][j], Math.min(dp[i - 1][j - 1], dp[i][j - 1])) + 1;
                    }
                    maxSide = Math.max(maxSide, dp[i][j]);
                }
            }
        }
        return maxSide*maxSide;
    }
    ```

64. 除自身以外数组的乘积

    方法一：左右乘积列表

    思路

    我们不必将所有数字的乘积除以给定索引处的数字得到相应的答案，而是利用索引左侧所有数字的乘积和右侧所有数字的乘积（即前缀与后缀）相乘得到答案。

    对于给定索引 i，我们将使用它左边所有数字的乘积乘以右边所有数字的乘积。

    `left[i]=left[i-1]*nums[i-1]`, `right[i]=right[i+1]*nums[i+1]`

    ```java
    public int[] productExceptSelf(int[] nums) {
        int[] left = new int[nums.length];
        int[] right = new int[nums.length];
        //left[i]=left[i-1]*nums[i-1]
        left[0]=1;
        for (int i = 1; i < nums.length; i++) {
            left[i]=left[i-1]*nums[i-1];
        }
        right[nums.length-1]=1;
        for (int i = nums.length-2; i >=0; i--) {
            right[i]=right[i+1]*nums[i+1];
        }
        int[] res=new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            res[i]=left[i]*right[i];
        }
        return res;
    }
    ```

    上述方法还可以优化空间，我们先把输出数组当作left数组，计算出left数组的值，然后**从右向左遍历**，动态的构造出right数组，只需要一个变量保存right当前的值就好

    ```java
    public int[] productExceptSelf(int[] nums) {
        int len=nums.length;
        int[] ans = new int[len];
        ans[0]=1;
        for (int i = 1; i < len; i++) {
            ans[i]=ans[i-1]*nums[i-1];
        }
        int right=1;
        for (int i = len-1; i >=0 ; i--) {
            // 对于索引 i，左边的乘积为 answer[i]，右边的乘积为 R
            ans[i]=ans[i]*right;
            // R 需要包含右边所有的乘积，所以计算下一个结果时需要将当前值乘到 R 上
            right=right*nums[i];
        }
        return ans;
    }
    ```

65. 搜索二维矩阵2(240)：

    方法一：每行进行二分查找

    ```java
    public boolean searchMatrix(int[][] matrix, int target) {
        for (int[] row :
             matrix) {
            int res = search(row, target);
            if (res >= 0) {
                return true;
            }
        }
        return false;
    }
    
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid - 1;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
    ```

    方法二：z字形查找

    我们可以从矩阵 matrix 的右上角 (0,n−1) 进行搜索。在每一步的搜索过程中，如果我们位于位置 (x,y)，那么我们希望在以 matrix 的左下角为左下角、以 (x,y) 为右上角的矩阵中进行搜索，即行的范围为 [x,m−1]，列的范围为 [0,y]：

    小tips：matrix[x,y]始终保持在上述的搜索区域是一行最大，一列最小的元素

    如果 matrix[x,y]=target，说明搜索完成；

    如果 matrix[x,y]>target，由于每一列的元素都是升序排列的，那么在当前的搜索矩阵中，所有位于**第 y 列**的元素都是严格大于 target 的，因此我们可以将它们全部忽略，即将 y 减少 1；

    如果 matrix[x,y]<target，由于每一行的元素都是升序排列的，那么在当前的搜索矩阵中，所有位于**第 x 行**的元素都是严格小于 target 的，因此我们可以将它们全部忽略，即将 x 增加 1。

    在搜索的过程中，如果我们超出了矩阵的边界，那么说明矩阵中不存在 target。

    ```java
    public boolean searchMatrix(int[][] matrix, int target){
        int m=matrix.length,n=matrix[0].length;
        int x=0,y=n-1;//矩阵的右上角，行最大的元素，列最小的元素
        while (x<m&&y>=0){
            if (target>matrix[x][y]){
                x++;
            }else if (target<matrix[x][y]){
                y--;
            }else{
                return true;
            }
        }
        return false;
    }
    ```

66. 完全平方数(279)：

    方法一：动态规划

    我们可以依据题目的要求写出状态表达式：f[i]表示最少需要多少个数的平方来表示整数 i。这些数必然落在区间 [1,sqrt(n)]。我们可以枚举这些数，假设当前枚举到 j，那么我们还需要取若干数的平方，构成 i-j^2 此时我们发现该子问题和原问题类似，只是规模变小了。这符合了动态规划的要求，于是我们可以写出状态转移方程 `dp[i]=min(dp[i],dp[i-j*j]+1),j从1枚举到sqrt(n)`

    边界条件，将dp[0]初始化为0，为了满足j*j恰好为n的情况

    题目有个坑，不一定每次都取最大的平方数，结果就是用了最少的数字，比如41，如果取36，那么还需要5=4+1，总共3个数字，但可以直接取41=25+16总共2个数字

    ```java
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 0;
        // 5
        // 0 1 2 3 1 2
        for (int i = 1; i <= n; i++) {
            //将当前数字先更新为最大的结果，
            //即 dp[i]=i，比如 i=4，最坏结果为 4=1+1+1+1 即为 4 个数字
            dp[i] = i;
            for (int j = 1; j * j <= i; j++) {
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
    ```

67. 实现Trie（前缀树）：208题

    ```c++
    class Trie {
        private:
        bool isEnd;
        Trie* next[26];
        public:
        Trie() {
            isEnd = false;
            memset(next, 0, sizeof(next));
        }
    /**
     * 这个操作和构建链表很像。首先从根结点的子结点开始与 word 第一个字符进行匹配，
     * 一直匹配到前缀链上没有对应的字符，这时开始不断开辟新的结点，直到插入完 word 的最后一个字符，
     * 同时还要将最后一个结点isEnd = true;，表示它是一个单词的末尾。
     * @param word 
     */
        void insert(string word) {
            Trie* node = this;
            for (char c : word) {
                if (node->next[c-'a'] == NULL) {
                    node->next[c-'a'] = new Trie();
                }
                node = node->next[c-'a'];
            }
            node->isEnd = true;
        }
    /**
     *从根结点的子结点开始，一直向下匹配即可，如果出现结点值为空就返回 false，
     * 如果匹配到了最后一个字符，那我们只需判断 node->isEnd即可。
     * @param word
     * @return
     */
        bool search(string word) {
            Trie* node = this;
            for (char c : word) {
                node = node->next[c - 'a'];
                if (node == NULL) {
                    return false;
                }
            }
            return node->isEnd;
        }
    /**
     * 和 search 操作类似，只是不需要判断最后一个字符结点的isEnd，
     * 因为既然能匹配到最后一个字符，那后面一定有单词是以它为前缀的。
     * @param prefix
     * @return
     */
        bool startsWith(string prefix) {
            Trie* node = this;
            for (char c : prefix) {
                node = node->next[c-'a'];
                if (node == NULL) {
                    return false;
                }
            }
            return true;
        }
    };
    ```

    **总结**
    通过以上介绍和代码实现我们可以总结出 Trie 的几点性质：

    - Trie 的形状和单词的插入或删除顺序无关，也就是说对于任意给定的一组单词，Trie 的形状都是唯一的。

    - 查找或插入一个长度为 L 的单词，访问 next 数组的次数最多为 L，和 Trie 中包含多少个单词无关。

    - Trie 的每个结点中都保留着一个字母表，这是很耗费空间的。如果 Trie 的高度为 n，字母表的大小为 m，最坏的情况是 Trie 中还不存在前缀相同的单词，那空间复杂度就为 O(m^n)

    最后，关于 Trie 的应用场景，希望你能记住 8 个字：**一次建树，多次查询**。(慢慢领悟叭~~)

    注意：一道题目**先思考20-30分钟**如果实在做不出来再看看题解和思路，然后自己试试能不能自己写出代码，题目后面是在LeetCode网站上的题号，前面那个是自动生成的有序列表。

    - 2022/4/24 基本上一天四道题

    1. 两数之和(1)：太简单没什么说的。

    2. 两数相加(2)：应该根据题目现有的条件来做，从题目本身出发，不要一开始就考虑转换为常用的按顺序的十进制加法，**类比二进制加法计算（二进制全加法器）**，直接用两个链表的结点从前往后相加即可。

    3. 无重复字符的最长子串(3)：

       用**滑动窗口**思想，两个指针left和right，如果left和right之间没有重复就让right++，因为之前left和right之间无重复，所有如果right++后出现重复一定是现在的right和之前的left到right之间有一个值重复了，以```string.charAt(right)```为标准从left开始遍历，分三种情况（前部、中部、后部）

       （1）a b c a

       （2）a b c b

       （3）a b c c

       从j=left开始遍历（j++），如果遇到这两种重复的情况，让left=j+1即左指针指向重复位置的下一个位置。

    4. 寻找两个正序数组的中位数(4)：

       归并排序，将**两个有序数组合并为一个**，for循环遍历合并后的数组，然后分别设置两个指针指向需要排序的两个数组，取两个数组中小的数放入新数组，并且让该数组指针后移，**边界情况**若一个数组指针已经指向末尾，则另外判断次情况，将另一个数组的数全部放入即可。

       算法时间复杂度为O（m+n），只用遍历一次新数组即可。

       <!--more-->

    5. 最长回文子串(5)：

       （1）扩展中心：回文子串分两种，奇数长度：abcba可以从中间的a开始向两边扩展，偶数长度：abcabbabb，从[1]和[2]中间开始向两边扩展，下面是扩展的函数

       ```java
       public int expand(String s,int start,int end){
       	while(start>=0&&end<s.length()&&s.charAt(start)==s.charAt(end)){
                   start--;
                   end++;
               }
          	return end-start-1;//注意这里的减一，比如abcba，到两边a的时候还会start--和end++,所以要减一
       }
       ```

       遍历字符串，从每个点开始扩展，有两种扩展方式，分别为`expand(s,i,i);//abcba`（从c开始扩展）和`len2=expand(s,i,i+1);//abba`（从[1]b和[2]b开始扩展），然后找出最长的那个扩展方式，`start=i-(len-1)/2;end=i+len/2;`,注意这里的start和end。

       （2）动态规划：p（i，j）=p(i+1,j-1)&&s[i]==s[j]所以如果我们想知道 P（i , j）的情况，只需要知道 P（i + 1，j - 1）的情况就可以了，然后向两边扩展即可，这样时间复杂度就少了 O(n)。因此我们可以用动态规划的方法，空间换时间，把已经求出的 P（i，j）存储起来，求 长度为 1 和长度为 2 的 P(i,j) 时不能用上边的公式，例如求p（1，2），带入会出现p（2，1），长度为1和2的回文串需要单独判断。

       ```java
       for (int len = 1; len <= length; len++) //遍历所有的长度
               for (int start = 0; start < length; start++) {
                   int end = start + len - 1;
                   if (end >= length) //下标已经越界，结束本次循环
                       break;
                   P[start][end] = (len == 1 || len == 2 || P[start + 1][end - 1]) && s.charAt(start) == s.charAt(end); //长度为 1 和 2 的单独判断下
                   if (P[start][end] && len > maxLen) {
                       maxLen=len;
                       maxPal = s.substring(start, end + 1);
                   }
               }
       ```

       时间复杂度：两层循环 O(n²）

       空间复杂度：用二维数组 P*P* 保存每个子串的情况 O(n²)

    6. Z字形变换(6)：是一个找规律的题目，可以找出规律来用二维数组模拟

       ![image-20220328225949259](image-20220328225949259.png)

    7. 正则表达式匹配(10)：

       给你一个字符串 `s` 和一个字符规律 `p`，请你来实现一个支持 `'.'` 和 `'*'` 的正则表达式匹配。

       - `'.'` 匹配任意单个字符
       - `'*'` 匹配零个或多个前面的那一个元素

       使用动态规划，对匹配的方案进行枚举。我们用 f [i] [j] 表示 s 的前 i 个字符与 p 中的前 j 个字符是否能够匹配.

       如果 p 的第 j 个字符是一个小写字母，那么我们必须在 s中匹配一个相同的小写字母，即

    ![image-20220407223143316](1-166246313752888.png)

    ![image-20220407223216513](2-166246313752887.png)

    如果我们通过这种方法进行转移，那么我们就需要枚举这个组合到底匹配了 s 中的几个字符，会增导致时间复杂度增加，并且代码编写起来十分麻烦。我们不妨换个角度考虑这个问题：字母 + 星号的组合在匹配的过程中，本质上只会有两种情况：

    + 匹配 s 末尾的一个字符，将该字符扔掉，而该组合还可以继续进行匹配；

    + 不匹配字符，将该组合扔掉，不再进行匹配。

    如果按照这个角度进行思考，我们可以写出很精巧的状态转移方程：

    ![image-20220407223308404](3-166246313752889.png)

    

    ![image-20220407223334274](4-166246313752890.png)

    其中 matches(x,y) 判断两个字符是否匹配的辅助函数。只有当 y 是 . 或者 x 和 y 本身相同时，这两个字符才会匹配。

    8. 盛最多水的容器(11)：

      **双指针**解决，第一次做这题可能不会想到双指针。下面解释双指针算法的过程，并证明其正确性。给定一个数组height[1, 8, 6, 2, 5, 4, 8, 3, 7]，初始时分别有一左一右两个指针位于边界，容纳的水量=两个指针指向的数字中较小值∗指针之间的距离，假设左指针为left右指针为right，不妨设此时height[left]<height[right]，如果移动右指针，那么两个指针的距离一定变小，并且min{height[left],height[right']}<=之前的高度，因为如果此时新的高度比原来的高度大，取height[left]和height[right]的最小值还是之前的height[left]，高度没变但距离变小，所以容器的容积变小，如果新的高度比原来的高度小，则导致高度变小且距离变小，容积也会变小，即无论我们怎么移动右指针，得到的容器的容量都小于移动前容器的容量，也就是说，这个左指针对应的数不会作为容器的边界了，那么我们就可以丢弃这个位置，将左指针向右移动一个位置，此时新的左指针于原先的右指针之间的左右位置，才可能会作为容器的边界。综上，可以得出每次移动指针的时候移动高度较小的那个指针是正确答案，边界条件是left<right。

    9. 整数转罗马数字(12)：

       按照题目所给规则可能出现的罗马数字如下

       ![image-20220410224312579](image-20220410224312579.png)

       ![image-20220410225427004](image-20220410225427004.png)

       根据上图求140的罗马数字，我们用来确定罗马数字的规则是：对于罗马数字从左到右的每一位，选择尽可能大的符号值。对于 140，最大可以选择的符号值为C=100。接下来，对于剩余的数字 40，最大可以选择的符号值为 XL=40。因此，140 的对应的罗马数字为 C+XL=CXL。

       采取模拟的思路，根据罗马数字的唯一表示法，为了表示一个给定的整数 num，我们寻找不超过 num 的最大符号值，将 num 减去该符号值，然后继续寻找不超过 num 的最大符号值，将该符号拼接在上一个找到的符号之后，循环直至 num 为 0。最后得到的字符串即为 num 的罗马数字表示。

       编程时，可以建立一个数值-符号对的列表valueSymbols，按数值从大到小排列。遍历 valueSymbols 中的每个数值-符号对，若当前数值 value 不超过 num，则从 num 中不断减去 value，直至 num 小于 value，然后遍历下一个数值-符号对。若遍历中 num 为 0 则跳出循环。valueSymbols对应表和代码如下

       ```java
       int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
       String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
       public String intToRoman(int num) {
           StringBuffer roman = new StringBuffer();
           for (int i = 0; i < values.length; ++i) {
               int value = values[i];
               String symbol = symbols[i];
               while (num >= value) {
                   num -= value;
                   roman.append(symbol);
               }
               if (num == 0) {
                   break;
               }
           }
           return roman.toString();
       }
       ```

    10. 三数之和(15)：

       刚开始做这题直接三重循环遍历，会超出时间限制，需要改进算法，看来LeetCode上的题目有些**不能直接模拟**，要想想降低复杂度的做法。「不重复」的本质是什么？我们保持三重循环的大框架不变，只需要保证：

       第二重循环枚举到的元素不小于当前第一重循环枚举到的元素；

       第三重循环枚举到的元素不小于当前第二重循环枚举到的元素。

       也就是说，我们枚举的三元组 (a, b, c) 满足 a≤b≤c，保证了只有(a,b,c) 这个顺序会被枚举到，而 (b, a, c)、(c, b, a) 等等这些不会，这样就减少了重复。要实现这一点，我们可以将数组中的元素从小到大进行排序，随后使用普通的三重循环就可以满足上面的要求。

       同时，**对于每一重循环而言，相邻两次枚举的元素不能相同**，否则也会造成重复。举个例子，如果排完序的数组为

       [0, 1, 2, 2, 2, 3]
        ^  ^  ^
       我们使用三重循环枚举到的第一个三元组为 (0, 1, 2)，如果第三重循环继续枚举下一个元素，那么仍然是三元组 (0, 1, 2)，产生了重复。因此我们需要将第三重循环「跳到」下一个不相同的元素，即数组中的最后一个元素 3，枚举三元组 (0, 1, 3)。

       ![image-20220411225207697](image-20220411225207697.png)

       这个方法就是我们常说的「双指针」，当我们需要枚举数组中的两个元素时，如果我们发现随着第一个元素的递增，第二个元素是递减的，那么就可以使用双指针的方法，将枚举的时间复杂度从 O(N^2) 减少至 O(N)。为什么是 O(N) 呢？这是因为在枚举的过程每一步中，「左指针」会向右移动一个位置（也就是题目中的 b），而「右指针」会向左移动若干个位置，这个与数组的元素有关，但我们知道它一共会移动的位置数为 O(N)，均摊下来，每次也向左移动一个位置，因此时间复杂度为 O(N)。

       双指针看似容易理解，但不同题目下的**边界条件**都不尽相同，边界条件应该是一个难点，需要仔细考虑。

       ```java
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int k = nums.length - 1;
            int target = -nums[i];
            if (i == 0 || nums[i] != nums[i - 1]) {//对于每一重循环相邻两个元素不能相同
                for (int j = i + 1; j < nums.length; j++) {
                    if (j == i + 1 || nums[j] != nums[j - 1]) {
                        while (j < k && nums[j] + nums[k] > target) {
                            k--;
                        }
                        // 如果指针重合，一种情况是上一次j<k时（j和k相邻），nums[j] + nums[k] > target
                        // 另一种情况时j不断增加但nums[j]+nums[k]<target，随着 j 后续的增加
                        // 就不会有满足 nums[j] + nums[k] = target 并且 j<k 的值了，可以退出循环
                        if (j == k) {
                            break;
                        }
                        if (nums[j] + nums[k] == target) {
                            List<Integer> integerList = new ArrayList<>();
                            integerList.add(nums[i]);
                            integerList.add(nums[j]);
                            integerList.add(nums[k]);
                            list.add(integerList);
                            // 这里不要写k--，要让k--交由上面的while循环来判断执行
                            // 如果写了k--，在j和k相邻的情况下，下一次循环j会大于k（j比k大1），
                            // while循环和if失效，所以k--要交由while循环来执行
                        }
                    }
                }
            }
        }
        return list;
       ```

    11. 电话号码的字母组合(17)：

       这道题其实~~直接模拟~~也能做，因为最多只有四位电话号码，直接根据不同的位数电话号码采取一层for，二层for，三层for，四层for遍历也可AC。

       首先使用哈希表存储每个数字对应的所有可能的字母，然后进行回溯操作。

       回溯过程中维护一个字符串，表示已有的字母排列（如果未遍历完电话号码的所有数字，则已有的字母排列是不完整的）。该字符串初始为空。每次取电话号码的一位数字，从哈希表中获得该数字对应的所有可能的字母，并将其中的一个字母插入到已有的字母排列后面，然后继续处理电话号码的后一位数字，直到处理完电话号码中的所有数字，即得到一个完整的字母排列。然后进行回退操作，遍历其余的字母排列。

       回溯算法用于寻找所有的可行解，如果发现一个解不可行，则会舍弃不可行的解。在这道题中，由于每个数字对应的每个字母都可能进入字母组合，因此不存在不可行的解，直接穷举所有的解即可。下面贴上带有回溯的递归函数的代码


    ```java
    // 这个回溯的递归函数借鉴了赫夫曼编码的向左为0向右为1的那个递归函数
    public void f(List<String> res, HashMap<Character, String> stringHashMap, String digits, StringBuilder stringBuilder, int index) {
        //用index遍历电话号码，for循环遍历每一个电话号码对应的所有字符
        //  2   3
        // abc def
        if (digits.length() != index) {
            String digitString = stringHashMap.get(digits.charAt(index));
            for (int i = 0; i < digitString.length(); i++) {
                stringBuilder.append(digitString.charAt(i));
                f(res, stringHashMap, digits, stringBuilder, index + 1);
                // 当该递归函数返回进入下一行时，说明已经遍历完电话号码的所有数字，
                // 下一步应该取最后一个电话号码的下一个字符（也就是i++），需要删掉stringBuilder2的末尾一位，
                // 让最后一个电话号码的下一个字符填入进来
                stringBuilder.deleteCharAt(stringBuilder.length() - 1);
            }
        } else {
            res.add(stringBuilder.toString());
        }
    }
    ```
    
    12. 删除链表的倒数第N个结点(19)：
    
        我的想法是从头节点开始遍历，用于遍历的结点记为indexNode，如果indexNode向后遍历n次为null的话，证明此时indexNode结点即为要删除的结点，看下面这个图可以轻松证明
    
    ![image-20220414201955313](image-20220414201955313.png)
    
    ​	还需要设置一个preNode作为indexNode的前一个结点，这样才能做到删除操作，除此以外需要注意一个特殊情况即删除的结点是头结点，直接返回head.next即可。
    
    ​	一个小tips：处理链表是可以在头结点前面加上一个哑节点dummy，dummy不保存任何信息只是指向原链表的头结点，这样可以有效解决删除链表头相关的特殊情况，可以把有关链表头的特殊情况统一到普通情况的代码中。（不是下面贴的那段代码）
    
    ```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head.next==null){
            return null;//该链表只有一个结点
        }
        ListNode indexNode=head;
        ListNode preNode=null;
        while (indexNode!=null){
            ListNode temp=indexNode;
            for (int i = 0; i < n; i++) {
                temp=temp.next;
            }
            if (temp==null){
                if (preNode==null){
                    //删除的是头节点的情况
                    return head.next;
                }
                preNode.next=preNode.next.next;
                break;
            }
            //这里的保存preNode是一个保存链表前一个结点的经典操作
            preNode=indexNode;
            indexNode=indexNode.next;
        }
        return head;
    }
    ```
    
    13. 有效的括号(20)：
    
    ​		典型的**栈的应用**，我们遍历给定的字符串 s。当我们遇到一个左括号时，我们会期望在后续的遍历中，有一个相同类型的右括号将其闭合。由于后遇到的左括号要先闭合（**后进先出**），因此我们可以将这个左括号放入栈顶。当我们遇到一个右括号时，我们需要将一个相同类型的左括号闭合。此时，我们可以取出栈顶的左括号并判断它们是否是相同类型的括号。**如果不是相同的类型，或者栈中并没有左括号**，那么字符串 s 无效，返回 False。在遍历结束后，如果栈中没有左括号，说明我们将字符串 s 中的所有左括号闭合，返回 True，否则返回 False。
    
    ​		注意到有效字符串的长度一定为偶数，因此如果字符串的长度为奇数，我们可以直接返回 False，省去后续的遍历判断过程。
    
    14. 合并k个升序列表(23):
    
        前置知识：合并两个有序链表
    
        - 首先我们需要一个变量 head 来保存合并之后链表的头部，你可以把 head 设置为一个虚拟的头（也就是 head 的 val 属性不保存任何值），这是为了方便代码的书写，在整个链表合并完之后，返回它的下一位置即可。**设置哑结点方便链表操作的代码书写**
        - 我们需要一个指针 tail 来记录下一个插入位置的前一个位置，以及两个指针 aPtr 和 bPtr 来记录 a 和 b 未合并部分的第一位。注意这里的描述，tail 不是下一个插入的位置，aPtr 和 bPtr 所指向的元素处于「待合并」的状态，也就是说它们还没有合并入最终的链表。 当然你也可以给他们赋予其他的定义，但是定义不同实现就会不同。
        - 当 aPtr 和 bPtr 都不为空的时候，取 val 熟悉较小的合并；如果 aPtr 为空，则把整个 bPtr 以及后面的元素全部合并；bPtr 为空时同理。
    
        ```java
        public ListNode mergeTwoLists(ListNode a, ListNode b) {
            if (a == null || b == null) {
                return a != null ? a : b;
            }
            ListNode head = new ListNode(0);//这里的head就是哑结点
            ListNode tail = head, aPtr = a, bPtr = b;
            while (aPtr != null && bPtr != null) {
                if (aPtr.val < bPtr.val) {
                    tail.next = aPtr;
                    aPtr = aPtr.next;
                } else {
                    tail.next = bPtr;
                    bPtr = bPtr.next;
                }
                tail = tail.next;
            }
            tail.next = (aPtr != null ? aPtr : bPtr);
            return head.next;
        }
        ```
    
        方法一：顺序合并
    
        我们可以想到一种最朴素的方法：用一个变量 ans 来维护以及合并的链表，第 i 次循环把第 i个链表和 ans 合并，答案保存到 ans 中。
    
        ```java
        public ListNode mergeKLists(ListNode[] lists){
            ListNode ans=null;
            for (int i = 0; i < lists.length; i++) {
                ans=mergeTwoLists(ans,lists[i]);
            }
            return ans;
        }
        ```
    
        方法二：分治合并
    
        考虑优化方法一，用分治的方法进行合并。
    
    ![image-20220416111324477](image-20220416111324477.png)
    
    ```java
    public ListNode merge(ListNode[] lists,int l,int r){
        if (l==r){
            return lists[l];
        }
        if (l>r){
            return null;//本题中其实并不会出现l>r
        }
        int mid=(l+r)/2;
        return mergeTwoLists(merge(lists,l,mid),merge(lists,mid+1,r));
    }
    public ListNode mergeKLists(ListNode[] lists){
        if (lists==null||lists.length==0){//注意这两个特殊情况的判断
            return null;
        }
        return merge(lists,0,lists.length-1);
    }
    ```
    
    - 长度为0的数组 int[] arr = new int[0]，也称为空数组，虽然arr长度为0，但是依然是一个**对象**
    
    - null数组，int[] arr = null；arr是一个数组类型的**空引用**。
    
    ​	方法三：使用优先队列合并
    
    这个方法和前两种方法的思路有所不同，我们需要维护当前每个链表没有被合并的元素的最前面一个，k 个链表就最多有 k 个满足这样条件的元素，每次在这些元素里面选取 val 属性最小的元素合并到答案中。在选取最小元素的时候，我们可以用**优先队列**来优化这个过程。
    
    ```java
    class Status implements Comparable<Status> {
        int val;
        ListNode ptr;
    
        public Status(int val, ListNode ptr) {
            this.val = val;
            this.ptr = ptr;
        }
    
        @Override
        public int compareTo(Status o) {
            return this.val - o.val;
        }
    }
    
    PriorityQueue<Status> queue = new PriorityQueue<>();
    
    public ListNode mergeKLists(ListNode[] lists) {
        for (ListNode node :
             lists) {
            if (node != null) {
                queue.offer(new Status(node.val, node));
            }
        }
        ListNode head = new ListNode(0);
        ListNode tail = head;
        while (!queue.isEmpty()) {
            Status f = queue.poll();
            tail.next = f.ptr;
            tail = tail.next;
            if (f.ptr.next != null) {
                queue.offer(new Status(f.ptr.next.val, f.ptr.next));
            }
        }
        return head.next;
    }
    ```
    
    15. 下一个排列(31)：
    
        整数数组的 **下一个排列** 是指其整数的下一个**字典序更大的排列**，注意到下一个排列总是比当前排列要大，除非该排列已经是最大的排列。我们希望找到一种方法，能够找到一个大于当前序列的新序列，且变大的幅度尽可能小。具体地：
    
        我们需要将一个左边的「较小数」与一个右边的「较大数」交换，以能够让当前排列变大，从而得到下一个排列。
    
        同时我们要让这个「较小数」尽量靠右，而「较大数」尽可能小。当交换完成后，「较大数」右边的数需要按照升序重新排列。这样可以在保证新排列大于原来排列的情况下，使变大的幅度尽可能小。
    
        以排列 [4,5,2,6,3,1] 为例：
    
        我们能找到的符合条件的一对「较小数」与「较大数」的组合为 2 与 3，满足「较小数」尽量靠右，而「较大数」尽可能小。
    
        当我们完成交换后排列变为 [4,5,3,6,2,1]，此时我们可以重排「较小数」右边的序列，序列变为 [4,5,3,1,2,6]。
    
        具体地，我们这样描述该算法，对于长度为 n 的排列 a：
    
        - 首先从后向前查找第一个顺序对 (i,i+1)，满足 a[i] < a[i+1]。这样「较小数」即为 a[i]。此时 [i+1,n) 必然是下降序列。
    
        - 如果找到了顺序对，那么在区间 [i+1,n) 中从后向前查找第一个元素 j 满足 a[i] < a[j]。这样「较大数」即为 a[j]。
    
        - 交换 a[i] 与 a[j]，此时可以证明区间 [i+1,n) 必为降序。我们可以直接使用双指针反转区间 [i+1,n) 使其变为升序，而无需对该区间进行排序。
    
    ```java
    public void nextPermutation(int[] nums) {
        int index=nums.length-1;
        //从后向前找到第一个不满足从后向前为升序的位置，该位置为index-1（如果index！=0）
        //从后向前为升序意味着该子序列是最大的序列，无法通过交换该子序列的内部获得下一个排列
        //所以需要一直找到一个不满足该条件的位置
        while (index>0&&nums[index]<=nums[index-1]){
            index--;
        }
        int left=0,right=nums.length-1;;
        if (index != 0) {//如果index!=0意味着该数列不是递减数列
            int minTemp = index - 1;
            index = nums.length - 1;
            //在区间 [i+1,n) 中从后向前查找第一个元素 j 满足 a[i] < a[j]。这样「较大数」即为 a[j]。
            while (nums[index] <= nums[minTemp]) {
                index--;
            }
            //交换 a[i] 与 a[j]，此时可以证明区间 [i+1,n) 必为降序，交换后该序列一定比原序列大，但要保证**变大的幅度尽可能小**，所以需要把后面的子序列从小到大排列，这样就是下一个排列
            swap(nums, minTemp, index);
            left = minTemp + 1;
        }
        //使用双指针反转区间 [i+1,n) 使其变为升序，而无需对该区间进行排序。
        while (left<right){
            swap(nums,left,right);
            left++;
            right--;
        }
    }
    public void swap(int[] nums,int a,int b){
        int temp=nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
    }
    ```
    
    16. 最长有效括号(32)：
    
        + 方法一：动态规划
    
          首先要找到转移方程，怎么想转移方程？首先想的时候从已经求出了 `dp[i-1][j-1]` 入手，再加上已知 `s[i]`、`p[j]`，要想的问题就是怎么去求 `dp[i][j]`。
    
          结合题目，有最长这个字眼，可以考虑尝试使用 动态规划 进行分析。这是一个 最值型 动态规划的题目。
    
          动态规划题目分析的 4 个步骤：
    
          - 确定状态
    
            ​		研究**最优策略的最后一步**
    
            ​		化为子问题
    
          - 转移方程
    
            ​		根据子问题定义得到
    
          - 初始条件和边界情况
    
          - 计算顺序
    
          首先，我们定义一个 int[] dp 数组，其中第 i 个元素表示**以下标为 i 的字符结尾**的最长有效子字符串的长度。注意：如果第i个元素为 `(` 那么s[i] 无法和其之前的元素组成有效的括号对，所以，dp[i] = 0，以 `(` 结尾的子串对应的dp值都为零。下面来推导状态转移方程
    
          s[i]== '('
    
          这时，s[i] 无法和其之前的元素组成有效的括号对，所以，dp[i] = 0
    
          s[i] == ')'
    
          这时，需要看其前面对元素来判断是否有有效括号对。
    
          **情况1:**
    
          s[i - 1] == '('
    
          即 s[i] 和 s[i - 1] 组成一对有效括号，有效括号长度新增长度2，i位置对最长有效括号长度为 其之前2个位置的最长括号长度加上当前位置新增的2，我们无需知道i-2位置对字符是否可以组成有效括号对。
    
          那么有：`dp[i] = dp[i - 2] + 2`
    
          **情况2:**
    
          s[i - 1] == ')'
    
          这种情况下，如果前面有和s[i]组成有效括号对的字符，即形如( (....) )，这样的话，就要求s[i - 1]位置必然是有效的括号对，否则s[i]无法和前面对字符组成有效括号对。这时，我们只需要找到和s[i]配对对位置，并判断其是否是 `(` 即可。
    
          和其配对对位置为：`i - dp[i - 1] - 1`
    
          如果：`s[i - dp[i - 1] - 1] == '('`,有效括号长度新增长度2，i位置对最长有效括号长度为 i-1位置的最长括号长度加上当前位置新增的2，那么有：
    
          `dp[i] = dp[i - 1] + 2`
    
          值得注意的是，`i - dp[i - 1] - 1`和 i 组成了有效括号对，这将是一段独立的有效括号序列，如果之前的子序列是形如 (...)这种序列，那么当前位置的最长有效括号长度还需要加上这一段。所以：`dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2`


    ![image-20220416165854222](image-20220416165854222-166246313752896.png)
    
    ```python
    if s[i] == '(' :
        dp[i] = 0
    if s[i] == ')' :
        if s[i - 1] == '(' :
            dp[i] = dp[i - 2] + 2 #要保证i - 2 >= 0
        if s[i - 1] == ')' and s[i - dp[i - 1] - 1] == '(' :
            dp[i] = dp[i - 1] + dp[i - dp[i - 1] - 2] + 2 
            #要保证i - dp[i - 1] - 2 >= 0
    ```
    
    ​			下面贴上Java的源代码
    
    ```java
    public int longestValidParentheses(String s) {
        if (s.length()==0){
            return 0;
        }
        int[] dp=new int[s.length()];
        dp[0]=0;
        int maxLen=0;
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i)==')'){
                if (s.charAt(i-1)=='('){
                    dp[i]=2;
                    if (i-2>=0){
                        dp[i]=dp[i-2]+2;
                    }
                }else{//s[i-1]=')'
                    if (i-dp[i-1]-1>=0&&s.charAt(i-dp[i-1]-1)=='('){
                        dp[i]=dp[i-1]+2;
                        if (i-dp[i-1]-2>=0){
                            dp[i]=dp[i-1]+2+dp[i-dp[i-1]-2];
                        }
                    }
                }
            }
            if (dp[i]>maxLen){
                maxLen=dp[i];
            }
        }
        return maxLen;
    }
    ```
    
    ​		方法二：使用**栈**
    
    ​		栈里存的是括号对应的**下标**，具体做法是我们始终保持**栈底元素**为当前已经遍历过的元素中「最后一个没有被匹配的右括号的下标」，这样的做法主要是考虑了边界条件的处理，栈里其他元素维护左括号的下标：
    
    ​		对于遇到的每个 ‘(’ ，我们将它的下标放入栈中
    ​		对于遇到的每个 ‘)’ ，我们先弹出栈顶元素表示匹配了当前右括号：
    
    ​		如果栈为空，说明当前的右括号为没有被匹配的右括号，我们将其下标放入栈中来更新我们之前提到的「最后一个没有被匹配的右括号的下标」
    ​		如果栈不为空，当前右括号的下标减去栈顶元素即为「以该右括号为结尾的最长有效括号的长度」
    ​		我们从前往后遍历字符串并更新答案即可。需要注意的是，如果一开始栈为空，第一个字符为左括号的时候我们会将其放入栈中，这样就不满足提及的「最后一个没有被匹配的右括号的下标」，为了保持统一，我们在一开始的时候往栈中放入一个值为-1 的元素。
    
    ​		总结：两种索引会入栈
    
    1. 等待被匹配的左括号索引。
    2. 充当「参照物」的右括号索引。因为：当左括号匹配光时，栈需要留一个垫底的参照物，用于计算一段连续的有效长度。
    
    ```java
    public int longestValidParentheses(String s){
        int maxLen=0;
        Deque<Integer> stack=new LinkedList<>();
        stack.push(-1);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i)=='('){
                stack.push(i);
            }else{//s[i]==')'
                stack.pop();//先出栈，再相减计算有多少个匹配的括号
                //举个例子 ()(()))
                if (!stack.isEmpty()){
                    int curLen=i-stack.peek();
                    maxLen=Math.max(maxLen,curLen);
                }else{
                    stack.push(i);
                }
            }
        }
        return maxLen;
    }
    ```
    
    ​		方法三：
    
    ​		在此方法中，我们利用两个计数器 left 和 right 。首先，我们从左到右遍历字符串，对于遇到的每个 ‘(’，我们增加 left 计数器，对于遇到的每个 ‘)’ ，我们增加 right 计数器。每当 left 计数器与 right 计数器相等时，我们计算当前有效字符串的长度，并且记录目前为止找到的最长子字符串。当 right 计数器比left 计数器大时，我们将left 和 right 计数器同时变回 0。
    
    ​		这样的做法**贪心**地考虑了以当前字符下标结尾的有效括号长度，每次当右括号数量多于左括号数量的时候之前的字符我们都扔掉不再考虑，重新从下一个字符开始计算，但这样会漏掉一种情况，就是遍历的时候左括号的数量始终大于右括号的数量，即 (() ，这种时候最长有效括号是求不出来的。
    
    ​		解决的方法也很简单，我们只需要从右往左遍历用类似的方法计算即可，只是这个时候判断条件反了过来：
    
    - 当 left 计数器比right 计数器大时，我们将 left 和right 计数器同时变回 0。
    - 当 left 计数器与right 计数器相等时，我们计算当前有效字符串的长度，并且记录目前为止找到的最长子字符串。
    
    ​		这样我们就能涵盖所有情况从而求解出答案。
    
    ```java
    public int longestValidParentheses(String s) {
        int left = 0, right = 0, maxLen = 0;
        for (int i = 0; i < s.length(); i++) {//从左向右遍历
            if (s.charAt(i) == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxLen = Math.max(left + right, maxLen);
            }
            if (right > left) {
                left = 0;
                right = 0;
            }
        }
        left=0;
        right=0;
        for (int i = s.length() - 1; i >= 0; i--) {//从右向左遍历
            if (s.charAt(i) == '(') {
                left++;
            } else {
                right++;
            }
            if (left == right) {
                maxLen = Math.max(left + right, maxLen);
            }
            if (left>right){
                left=0;
                right=0;
            }
        }
        return maxLen;
    }
    ```
    
    17. 搜索旋转排序数组(33)：
    
        - 方法一：
    
          比如`nums = [4,5,6,7,0,1,2], target = 0`，可以将该数组分为两部分看待，从4到7，从0到2，首先比较target与nums[0]的大小，如果target<nums[0]，则在后半部分从后往前搜索，如果target>nums[0]，则在前半部分从前往后搜索
    
        ```java
        public int search(int[] nums, int target) {
            if (nums.length == 1) {
                if (nums[0] == target) {
                    return 0;
                } else {
                    return -1;
                }
            }
            int index = 0;
            //[4,5,6,7,0,1,2], target = 0
            boolean isInFront = true;
            if (nums[0] > target) {
                index = nums.length - 1;
                isInFront = false;
            }
            if (isInFront) {
                while (index < nums.length - 1 && nums[index] < nums[index + 1]) {
                    if (nums[index] == target) {
                        return index;
                    }
                    index++;
                }
            } else {
                while (index > 0 && nums[index] > nums[index - 1]) {
                    if (nums[index] == target) {
                        return index;
                    }
                    index--;
                }
            }
            if (nums[index] == target) {
                return index;
            }
            return -1;
        }
        ```
    
        - 方法二：
    
          对于旋转数组 nums = [4,5,6,7,0,1,2]，首先根据 nums[0] 与 target 的关系判断 target 是在左段还是右段。
    
          例如 target = 5, 目标值在左半段，因此在 [4, 5, 6, 7, inf, inf, inf] 这个有序数组里找就行了；
          例如 target = 1, 目标值在右半段，因此在 [-inf, -inf, -inf, -inf, 0, 1, 2] 这个有序数组里找就行了。
          如此，我们又将「旋转数组中找目标值」 转化成了 「有序数组中找目标值」
    
          ```java
          public int search(int[] nums, int target) {
              int left = 0, right = nums.length - 1;
              while (left <= right) {
                  int mid = left + (right - left) / 2;
                  if (nums[mid] == target) {
                      return mid;
                  }
                  // 先根据 nums[0] 与 target 的关系判断目标值是在左半段还是右半段
                  if (target >= nums[0]) {
                      // 目标值在左半段时，若 mid 在右半段，则将 mid 索引的值改成 inf
                      // 注意这里的>=，如果target在最左端，就需要>=，比如
                      // nums=[5,1,2,3] target=5
                      if (nums[mid] < nums[0]) {
                          nums[mid] = Integer.MAX_VALUE;
                      }
                  } else {
                    // 目标值在右半段时，若 mid 在左半段，则将 mid 索引的值改成 -inf
                      if (nums[mid] >= nums[0]) {
                          nums[mid] = Integer.MIN_VALUE;
                      }
                  }
                  if (nums[mid] < target) {
                      left = mid + 1;
                  } else {
                      right = mid - 1;
                  }
              }
              return -1;
          }
          ```
    
    18. 在排序数组中查找元素的第一个和最后一个位置(34)：
    
        - 方法一：
    
          先用二分查找找到该元素的位置，然后从该位置不断向前遍历和向后遍历，以此来找到第一个和最后一个位置
    
          ```java
          public int[] searchRange(int[] nums, int target) {
              if (nums.length == 0) {
                  return new int[]{-1, -1};
              }
              int left = 0, right = nums.length - 1;
              int mid = (left + right) / 2, index = -1;
              while (left <= right) {
                  mid = (left + right) / 2;
                  if (nums[mid] == target) {
                      index = mid;
                      break;
                  }
                  if (nums[mid] > target) {
                      right = mid - 1;
                  } else {
                      left = mid + 1;
                  }
              }
              if (index == -1) {
                  return new int[]{-1, -1};
              }
              int indexA = index, indexB = index;
              while (indexA > 0 && nums[indexA] == nums[indexA - 1]) {
                  indexA--;
              }
              while (indexB < nums.length - 1 && nums[indexB] == nums[indexB + 1]) {
                  indexB++;
              }
              return new int[]{indexA, indexB};
          }
          ```
    
        - 二分查找主要用于在有序的列表中查找目标值，注意二分查找的前提条件必须是有序或者部分有序的列表，当列表中目标值存在多个时，我们可以利用二分查找目标值的左边界和右边界，即**左值二分查找**和**右值二分查找**。需要特别注意目标值在列表中找不到的几种情况。
    
          ```java
          /**
          * 左值二分模板
          */
          public int searchLeft(int[] nums, int target) {
              int left = 0;
              int right = nums.length - 1;
              while (left <= right) {
                  // 防止溢出，如果left和right都很大直接相加会溢出，
                  // 最好不要直接相加除以二
                  int middle = (right - left) / 2 + left;
                  if (nums[middle] >= target) {
                      // 继续往左边找
                      right = middle - 1;
                  } else {
                      // 继续往右边找
                      left = middle + 1;
                  }
              }
              // 考虑目标值不在数组中的两种情况,目标值小于最小值left等于0，
              // 目标值大于最大值left等于num.length
              if (left == nums.length || nums[left] != target) left = -1;
              return left;
          }
          
          /**
          * 右值二分模板
          */
          public int searchRight(int[] nums, int target) {
              int left = 0;
              int right = nums.length - 1;
              while (left <= right) {
                  // 防止溢出
                  int middle = (right - left) / 2 + left;
                  if (nums[middle] <= target) {
                      // 继续往右边找
                      left = middle + 1;
                  } else {
                      // 继续往左边找
                      right = middle - 1;
                  }
              }
              // 考虑目标值不在数组中的两种情况，目标值小于最小值right会等于-1，
              // 目标值大于最大值right会等于nums.length-1
              if (right == -1 || nums[right] != target) right = -1;
              return right;
          }
          ```
    
    19. 组数总和(39)：
    
      思路分析：根据示例 1：输入: candidates = [2, 3, 6, 7]，target = 7。
    
      候选数组里有 2，如果找到了组合总和为 7 - 2 = 5 的所有组合，再在之前加上 2 ，就是 7 的所有组合；
      同理考虑 3，如果找到了组合总和为 7 - 3 = 4 的所有组合，再在之前加上 3 ，就是 7 的所有组合，依次这样找下去。
      基于以上的想法，可以画出如下的树形图。建议大家自己在纸上画出这棵树，这一类问题都需要先画出树形图，然后编码实现。
    
      ![image-20220418221514841](image-20220418221514841.png)
    
      编码通过 **深度优先遍历** 实现，使用一个列表，在 深度优先遍历 变化的过程中，遍历所有可能的列表并判断当前列表是否符合题目的要求，成为「回溯算法」。
    
      说明上图：
    
      以 target = 7 为 根结点 ，创建一个分支的时 做减法 ；
      每一个箭头表示：从父亲结点的数值减去边上的数值，得到孩子结点的数值。边的值就是题目中给出的 candidate 数组的每个元素的值；
      减到 0 或者负数的时候停止，即：结点 0 和负数结点成为叶子结点；
      所有从根结点到结点 0 的路径（只能从上往下，没有回路）就是题目要找的一个结果。
      这棵树有 4 个叶子结点的值 0，对应的路径列表是 [[2, 2, 3], [2, 3, 2], [3, 2, 2], [7]]，而示例中给出的输出只有 [[7], [2, 2, 3]]。即：题目中要求每一个符合要求的解是 不计算顺序 的。下面我们分析为什么会产生重复。
    
      产生重复的原因是：在每一个结点，做减法，展开分支的时候，由于题目中说 **每一个元素可以重复使用**，我们考虑了 **所有的** 候选数，因此出现了重复的列表。具体的做法是：每一次搜索的时候设置 **下一轮搜索的起点** `index`，请看下图。
    
    ![image-20220418221816511](image-20220418221816511.png)
    
    ​		即：从每一层的第 2 个结点开始，都不能再搜索产生同一层结点已经使用过的 `candidate` 里的元素。
    
    ​		**剪枝提速**
    
    ​		根据上面画树形图的经验，如果 target 减去一个数得到负数，那么减去一个更大的数依然是负数，同样搜索不到结果。基于这个想法，我们可以对输入数组进行排序（从小到大），添加相关逻辑达到进一步剪枝的目的，下面贴上剪枝提速后的代码
    
    ```java
    public void f(List<List<Integer>> res, List<Integer> integerList, int[] candidates, int target, int index) {
        //[2, 3, 6, 7]  4
        if (target==0){
            //添加的时候拷贝比每次传进来就拷贝效率高
            res.add(new ArrayList<>(integerList));
            return;
        }
        for (int i = index; i < candidates.length; i++) {
            if (target-candidates[i]<0){
                break;//在这里剪枝效率很高
            }
            integerList.add(candidates[i]);
            f(res,integerList,candidates,target-candidates[i],i);
            integerList.remove(integerList.size()-1);
        }
    }
    ```
    
    - 排列问题，讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为不同列表时），需要记录哪些数字已经使用过，此时用 used 数组；
    
    - 组合问题，不讲究顺序（即 [2, 2, 3] 与 [2, 3, 2] 视为相同列表时），需要按照某种顺序搜索，此时使用 begin 变量（就是上面用的index）。
    
    下面详细介绍一下**回溯算法与深度优先遍历**
    
    - 回溯法 采用试错的思想，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将**取消上一步甚至是上几步的计算**，再通过其它的可能的分步解答再次尝试寻找问题的答案。回溯法通常用最简单的递归方法来实现，在反复重复上述的步骤后可能出现两种情况：
    
    ​		找到一个可能存在的正确的答案；
    ​		在尝试了所有可能的分步方法后宣告该问题没有答案。
    
    - 深度优先搜索 算法（英语：Depth-First-Search，DFS）是一种用于遍历或搜索树或图的算法。这个算法会 **尽可能深** 的搜索树的分支。当结点 v 的所在边都己被探寻过，搜索将**回溯到发现结点 v 的那条边的起始结点**。这一过程一直进行到已发现从源结点可达的所有结点为止。如果还存在未被发现的结点，则选择其中一个作为源结点并重复以上过程，整个进程反复进行直到所有结点都被访问为止。
    
    20. 全排列（46）：
    
        用这道题来与上面那道组数总和的回溯算法进行对比，再次深刻理解一下回溯算法和深度优先遍历（DFS）
    
        从全排列问题开始理解回溯算法
        我们尝试在纸上写 3 个数字、4 个数字、5 个数字的全排列，相信不难找到这样的方法。以数组 [1, 2, 3] 的全排列为例。
    
        先写以 1 开头的全排列，它们是：[1, 2, 3], [1, 3, 2]，即 1 + [2, 3] 的全排列（注意：递归结构体现在这里）；
        再写以 2 开头的全排列，它们是：[2, 1, 3], [2, 3, 1]，即 2 + [1, 3] 的全排列；
        最后写以 3 开头的全排列，它们是：[3, 1, 2], [3, 2, 1]，即 3 + [1, 2] 的全排列。
        总结搜索的方法：按顺序枚举每一位可能出现的情况，已经选择的数字在 当前 要选择的数字中不能出现。按照这种策略搜索就能够做到 不重不漏。这样的思路，可以用一个树形结构表示。
    
        ![image-20220422115357752](image-20220422115357752.png)
    
    ```java
    public void dfs(List<List<Integer>> res, List<Integer> integerList, int[] nums, boolean[] isUsed, int countNum) {
        if (countNum == nums.length) {
            res.add(new ArrayList<>(integerList));//每次添加到结果列表中进行拷贝会节省时间
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (isUsed[i]) {
                continue;
            }
            integerList.add(nums[i]);
            isUsed[i]=true;
            dfs(res, integerList, nums, isUsed, countNum + 1);
            isUsed[i] = false;//回溯之前清除这一次操作的影响
            integerList.remove(integerList.size()-1);
        }
    }
    ```
    
    ​		**说明**：
    
    ​		每一个结点表示了求解全排列问题的不同的阶段，这些阶段通过变量的「不同的值」体现，这些变量的不同的值，称之为「状态」；
    ​		使用深度优先遍历有「回头」的过程，**在「回头」以后， 状态变量需要设置成为和先前一样** ，因此在回到上一层结点的过程中，需要**撤销上一次的选择**，这个操作称之为「状态重置」；
    ​		深度优先遍历，借助系统栈空间，保存所需要的状态变量，在编码中只需要注意遍历到相应的结点的时候，状态变量的值是正确的，具体的做法是：**往下走一层的时候，path 变量在尾部追加，而往回走的时候，需要撤销上一次的选择，也是在尾部操作，因此 path 变量是一个栈；**
    ​		深度优先遍历通过「回溯」操作，实现了全局使用一份状态变量的效果。
    ​		使用编程的方法得到全排列，就是在这样的一个树形结构中完成 遍历，从树的根结点到叶子结点形成的路径就是其中一个全排列。
    
    ​		**设计状态变量**
    
    ​		首先这棵树除了根结点和叶子结点以外，每一个结点做的事情其实是一样的，即：在已经选择了一些数的前提下，在剩下的还没有选择的数中，依次选择一个数，这显然是一个 递归 结构；
    ​		递归的终止条件是： 一个排列中的数字已经选够了 ，因此我们**需要一个变量来表示当前程序递归到第几层**，我们把这个变量叫做 depth，或者命名为 index ，表示当前要确定的是某个全排列中下标为 index 的那个数是多少；
    ​		布尔数组 used，初始化的时候都为 false 表示这些数还没有被选择，当我们选定一个数的时候，就将这个数组的相应位置设置为 true ，这样在考虑下一个位置的时候，就能够以 O(1) 的时间复杂度判断这个数是否被选择过，这是一种「以空间换时间」的思想。
    ​		这些变量称为「状态变量」，它们表示了在求解一个问题的时候所处的阶段。需要根据问题的场景设计合适的状态变量。
    
    ​		**每一次尝试都「复制」，则不需要回溯**
    
    ​		如果在每一个 非叶子结点 分支的尝试，都创建 新的变量 表示状态，那么在回到上一层结点的时候不需要「回溯」；在递归终止的时候也不需要做拷贝。
    ​		这样的做法虽然可以得到解，但也会创建很多中间变量，这些中间变量很多时候是我们不需要的，会有一定空间和时间上的消耗。
    
    ​		**剪枝**
    
    ​		回溯算法会应用「剪枝」技巧达到以加快搜索速度。有些时候，需要做一些预处理工作（例如排序）才能达到剪枝的目的。预处理工作虽然也消耗时间，但能够剪枝节约的时间更多；
    ​		提示：剪枝是一种技巧，通常需要根据不同问题场景采用不同的剪枝策略，需要在做题的过程中不断总结。由于回溯问题本身时间复杂度就很高，所以能用空间换时间就尽量使用空间。
    
    ​		**总结**
    
    ​		做题的时候，建议 先画树形图 ，画图能帮助我们想清楚递归结构，想清楚如何剪枝。拿题目中的示例，想一想人是怎么做的，一般这样下来，这棵递归树都不难画出。
    
    ​		在画图的过程中思考清楚：
    
    ​		分支如何产生；题目需要的解在哪里？是在叶子结点、还是在非叶子结点、还是在从跟结点到叶子结点的路径？哪些搜索会产生不需要的解的？例如：产生重复是什么原因，如果在浅层就知道这个分支不能产生需要的结果，应该提前剪枝，剪枝的条件是什么，代码怎么写？
    
    21. 接雨水(42):
    
        方法一：
    
        按行求，先求出数组的最大的高度，然后遍历数组，从最高的一层开始，计算每一层的积水量，思路比较简单，下面贴上代码
    
        ```java
        public int trap(int[] height) {
            //一层一层的计算，这样直接模拟会超时，这是按行求的做法
            int maxHeight=0,res=0;
            for (int i = 0; i < height.length; i++) {
                if (height[i]>maxHeight){
                    maxHeight=height[i];
                }
            }
            int tempMax=maxHeight;
            for (int i = 0; i < maxHeight; i++) {
                int leftIndex=0;
                for (int j = 0; j < height.length; j++) {
                    if (height[j]==tempMax){
                        leftIndex=j;
                        height[j]--;
                        for (int k = leftIndex+1; k < height.length; k++) {
                            if (height[k]==tempMax){
                                res+=k-leftIndex-1;
                                leftIndex=k;
                                height[k]--;
                            }
                        }
                        tempMax--;
                        break;
                    }
                }
            }
            return res;
        }
        ```
    
        方法二：
    
        按列求，求每一列的水，我们只需要关注当前列，以及左边最高的墙，右边最高的墙就够了。装水的多少，当然根据木桶效应，我们只需要看左边最高的墙和右边最高的墙中较矮的一个就够了。所以，根据较矮的那个墙和当前列的墙的高度可以分为两种情况。
    
        - 较矮的墙的高度大于当前列的墙的高度，此时积水量就是较矮的墙的高度减去当前列的墙的高度
        - 较矮的墙的高度小于或等于当前列的墙的高度，此时无法积水
    
        ```java
        public int trap(int[] height) {
            int indexHeight=0,res=0;
            //直接按列计算也会超时，但可以用动态规划进行优化
            for (int i = 1; i < height.length-1; i++) {
                indexHeight=height[i];
                int leftHeightMax=0,rightHeightMax=0;
                for (int j = 0; j < height.length; j++) {
                    if (j<i){
                        if (height[j]>leftHeightMax){
                            leftHeightMax=height[j];
                        }
                    }
                    if (j>i){
                        if (height[j]>rightHeightMax){
                            rightHeightMax=height[j];
                        }
                    }
                }
                int minHeight=Math.min(leftHeightMax,rightHeightMax);
                if (minHeight>indexHeight){
                    res+=minHeight-indexHeight;
                }
            }
            return res;
        }
        ```
    
        方法三：
    
        使用动态规划对方法二按列求的方法进行优化
    
        我们注意到，解法二中。对于每一列，我们求它左边最高的墙和右边最高的墙，都是重新遍历一遍所有高度，这里我们可以优化一下。
    
        首先用两个数组，max_left [i] 代表第 i 列左边最高的墙的高度，max_right[i] 代表第 i 列右边最高的墙的高度。（一定要注意下，第 i 列左（右）边最高的墙，是不包括自身的，和 leetcode 上边的讲的有些不同）
    
        对于 max_left我们其实可以这样求。
    
        max_left [i] = Max(max_left [i-1],height[i-1])。它前边的墙的左边的最高高度和它前边的墙的高度选一个较大的，就是当前列左边最高的墙了。
    
        对于 max_right我们可以这样求。
    
        max_right[i] = Max(max_right[i+1],height[i+1]) 。它后边的墙的右边的最高高度和它后边的墙的高度选一个较大的，就是当前列右边最高的墙了。
    
        这样，我们再利用解法二的算法，就不用在 for 循环里每次重新遍历一次求 max_left 和 max_right 了。
    
        ```java
        public int trap(int[] height) {
            //dp做法
            int[] maxLeft = new int[height.length];//初始化都是0
            int[] maxRight = new int[height.length];
            int indexHeight=0,res=0;
            for (int i = 1; i < maxLeft.length; i++) {//从左往右求
                maxLeft[i] = Math.max(maxLeft[i - 1], height[i - 1]);
            }
            for (int i = maxLeft.length - 2; i >= 0; i--) {//从右往左求
                maxRight[i]=Math.max(maxRight[i+1],height[i+1]);
            }
            for (int i = 1; i < height.length - 1; i++) {
                indexHeight=height[i];
                int minHeight=Math.min(maxLeft[i],maxRight[i]);//直接查表得出两边较矮的高度
                if (minHeight>indexHeight){
                    res+=minHeight-indexHeight;
                }
            }
            return res;
        }
        ```
    
        方法四：
    
        **双指针优化动态规划**
    
        我们先明确几个变量的意思：
    
        ```scss
        left_max：左边的最大值，它是从左往右遍历找到的
        right_max：右边的最大值，它是从右往左遍历找到的
        left：从左往右处理的当前下标
        right：从右往左处理的当前下标
        ```
    
        定理一：在某个位置`i`处，它能存的水，取决于它左右两边的最大值中较小的一个。
    
        定理二：当我们从左往右处理到left下标时，左边的最大值left_max对它而言是可信的，但right_max对它而言是不可信的。（见下图，由于中间状况未知，对于left下标而言，right_max未必就是它右边最大的值）
    
        定理三：当我们从右往左处理到right下标时，右边的最大值right_max对它而言是可信的，但left_max对它而言是不可信的。
    
        ```text
                                           right_max
         left_max                             __
           __                                |  |
          |  |__   __??????????????????????  |  |
        __|     |__|                       __|  |__
                left                      right
        ```
    
        对于位置`left`而言，它左边最大值一定是left_max，右边最大值**大于等于**right_max，这时候，如果`left_max<right_max`成立，那么它就知道自己能存多少水了。无论右边将来会不会出现更大的right_max，都不影响这个结果。 所以当`left_max<right_max`时，我们就希望去处理left下标，反之，我们希望去处理right下标。
    
        ```java
        public int trap(int[] height) {
            int res = 0, left = 1, right = height.length - 2;
            int leftMax = height[0], rightMax = height[height.length - 1];//初始化左边最大高度和右边最大高度
            while (left <= right) {//等于是保证最后一列也会被处理
                if (leftMax < rightMax) {
                    if (leftMax > height[left]) {//先判断能不能积水再对左边最大高度进行更新
                        res += leftMax - height[left];
                    }
                    leftMax = Math.max(leftMax, height[left]);
                    left++;
                } else {//leftMax>=rightMax
                    if (rightMax>height[right]){
                        res+=rightMax-height[right];
                    }
                    rightMax = Math.max(rightMax,height[right]);
                    right--;
                }
            }
            return res;
        }
        ```
    
        方法五：
    
        使用**栈**，说到栈，我们肯定会想到括号匹配了。我们仔细观察蓝色的部分，可以和括号匹配类比下。每次匹配出一对括号（找到对应的一堵墙），就计算这两堵墙中的水。我们用栈保存每堵墙。
    
    ​		当遍历墙的高度的时候，如果当前高度小于栈顶的墙高度，说明这里会有积水，我们将墙的高度的下标入栈。
    
    ​		如果当前高度大于栈顶的墙的高度，说明之前的积水到这里停下，我们可以计算下有多少积水了。计算完，就把当前的墙继续入栈，作为新的积水的墙。
    
    ​		总体的原则就是，当前高度小于等于栈顶高度，入栈，指针后移。当前高度大于栈顶高度，出栈，计算出当前墙和栈顶的墙之间水的多少，然后计算当前的高度和新栈的高度的关系，重复第 2 步。直到当前墙的高度不大于栈顶高度或者栈空，然后把当前墙入栈，指针后移。
    
    ​		而对于计算 current 指向墙和新的栈顶之间的水，根据图的关系，我们可以直接把这两个墙当做之前解法三的 max_left 和 max_right，然后之前弹出的栈顶当做每次遍历的 height [ i ]。水量就是 Min ( max _ left ，max _ right ) - height [ i ]，只不过这里需要乘上两个墙之间的距离。可以根据下面这个图自己推导（验证）一下计算方法即可
    
    ![image-20220422162412824](image-20220422162412824-1662463137547102.png)
    
    ```java
    public int trap(int[] height){
        int res=0;
        Deque<Integer> stack=new LinkedList<>();
        int current=0;
        while (current<height.length){
            //当前高度大于栈顶的高度
            while (!stack.isEmpty()&&height[current]>height[stack.peek()]){
                int h=height[stack.pop()];//每次遍历的 height[i]
                if (stack.isEmpty()){
                    break;//如果栈为空直接将此时的current入栈即可
                }
                int distance=current-stack.peek()-1;//计算两堵墙之间的距离
                int min=Math.min(height[current],height[stack.peek()]);//取两堵墙的较矮的一堵墙的高度
                res+=distance*(min-h);
            }
            stack.push(current);
            current++;
        }
        return res;
    }
    ```
    
    22. 旋转图像(48):
    
        方法一：使用辅助数组
    
        对于矩阵中第 i 行的第 j个元素，在旋转后，它出现在倒数第 i 列的第 j个位置，即第j行第n-i-1列。这样以来，我们使用一个与 matrix 大小相同的辅助数组 newMatrix 临时存储旋转后的结果。我们遍历 matrix 中的每一个元素，根据上述规则将该元素存放到 newMatrix 中对应的位置。在遍历完成之后，再将 newMatrix 中的结果复制到原数组中即可。代码比较简单就不放上来了
    
        方法二：原地旋转
    
        **想出方法一中的等式很重要**
    
        ![image-20220422224637149](image-20220422224637149.png)
    
    ​		不断重复此过程，就可以发现这四项处于一个循环中，并且每一项旋转后的位置就是下一项所在的位置
    
    ![image-20220422224751882](image-20220422224751882-1662463137547101.png)
    
    ​		当我们知道了如何原地旋转矩阵之后，还有一个重要的问题在于：我们应该枚举哪些位置 (row,col) 进行上述的原地交换操作呢？由于每一次原地交换四个位置，因此：
    
    ![image-20220422224901047](image-20220422224901047-1662463137547103.png)
    
    ![image-20220422224918636](image-20220422224918636-1662463137547104.png)
    
    ```java
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; i++) {
            for (int j = 0; j < (n+1) / 2; j++) {
                //如果n是奇数，(n-1)/2=n/2
                //如果n是偶数，(n+1)/2=n/2
                int temp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1] = temp;
            }
        }
    }
    ```
    
    ​		方法三：用翻转代替旋转
    
    ​		可以通过先进行水平翻转，再进行对角线翻转得到结果。
    
    ![image-20220422225301778](image-20220422225301778-1662463137547105.png)
    
    ```java
    public void rotate(int[][] matrix) {
        int n=matrix.length;
        //水平翻转
        for (int i = 0; i < n / 2; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                int temp=matrix[i][j];
                matrix[i][j]=matrix[n-i-1][j];
                matrix[n-i-1][j]=temp;
            }
        }
        //对角线翻转
        for (int i = 0; i < matrix.length; i++) {
            for (int j = i+1; j < matrix[i].length; j++) {
                int temp=matrix[i][j];
                matrix[i][j]=matrix[j][i];
                matrix[j][i]=temp;
            }
        }
    }
    ```
    
    23. 字母异位词分组(49)：
    
        两个字符串互为字母异位词，当且仅当**两个字符串包含的字母相同**。同一组字母异位词中的字符串具备相同特征，可以使用相同特征作为一组字母异位词的标志，使用哈希表存储每一组字母异位词，哈希表的键为一组字母异位词的标志，哈希表的值为一组字母异位词列表。
    
        遍历每个字符串，对于每个字符串，得到该字符串所在的一组字母异位词的标志，将当前字符串加入该组字母异位词的列表中。遍历全部字符串之后，哈希表中的每个键值对即为一组字母异位词。
    
        以下的两种方法分别使用排序和计数作为哈希表的键，也就是用排序和计数两种方式求出的对应的字符串的特征来作为哈希表的键。代码可以很好的复习一下Java的集合的使用。
    
        方法一：排序
    
        由于互为字母异位词的两个字符串包含的字母相同，因此对两个字符串分别进行排序之后得到的字符串一定是相同的，故可以将**排序之后的字符串**作为哈希表的键。
    
        ```java
        public List<List<String>> groupAnagrams(String[] strs){
            HashMap<String,List<String>> map=new HashMap<>();
            for (String str :
                 strs) {
                char[] arr=str.toCharArray();
                Arrays.sort(arr);
                String key=new String(arr);
                //getOrDefault() 方法获取指定 key 对应对 value，如果找不到 key ，则返回设置的默认值。
                //hashmap.getOrDefault(Object key, V defaultValue)
                List<String> list=map.getOrDefault(key,new ArrayList<>());
                list.add(str);
                map.put(key,list);
            }
            return new ArrayList<>(map.values());
        }
        ```
    
        方法二：计数
    
        由于互为字母异位词的两个字符串包含的字母相同，因此两个字符串中的相同字母出现的次数一定是相同的，故可以将每个字母出现的次数使用字符串表示，作为哈希表的键。
    
        由于字符串只包含小写字母，因此对于每个字符串，可以使用长度为 26 的数组记录每个字母出现的次数。
    
        ```java
        public List<List<String>> groupAnagrams(String[] strs) {
            HashMap<String,List<String>> map=new HashMap<>();
            for (String str :
                 strs) {
                int[] counts=new int[26];
                for (int i = 0; i < str.length(); i++) {
                    counts[str.charAt(i)-'a']++;
                }
                // 将每个出现次数大于 0 的字母和出现次数按顺序拼接成字符串，作为哈希表的键
                StringBuffer stringBuffer=new StringBuffer();
                for (int i = 0; i < 26; i++) {
                    if (counts[i]!=0){
                        stringBuffer.append((char)('a'+i));
                        stringBuffer.append(counts[i]);
                    }
                }
                String key=stringBuffer.toString();
                List<String> list=map.getOrDefault(key,new ArrayList<>());
                list.add(str);
                map.put(key,list);//设置哈希表的键值对
            }
            return new ArrayList<>(map.values());
        }
        ```
    
    24. 最大子数组和(53)：
    
        方法一：动态规划
    
        设计状态思路：**把不确定的因素确定下来**，进而**把子问题定义清楚，把子问题定义得简单**。动态规划的思想通过解决了一个一个简单的问题，进而把简单的问题的解组成了复杂的问题的解。
    
        编写动态规划解题的步骤，把「状态定义」「状态转移方程」「初始化」「输出」「是否可以空间优化」全都写出来。
    
        定义状态（定义子问题）
    
        `dp[i]`：表示以 `nums[i]` **结尾** 的 **连续** 子数组的最大和。
    
        **说明**：「结尾」和「连续」是关键字。
    
        状态转移方程（描述子问题之间的联系）
    
        根据状态的定义，由于 nums[i] 一定会被选取，并且以 nums[i] 结尾的连续子数组与以 nums[i - 1] 结尾的连续子数组只相差一个元素 nums[i] 。
    
        - 如果 `dp[i - 1] > 0`，那么可以把 `nums[i]` 直接接在 `dp[i - 1]` 表示的那个数组的后面，得到和更大的连续子数组；
        - 如果 dp[i - 1] <= 0，那么 nums[i] 加上前面的数 dp[i - 1] 以后值不会变大。于是 dp[i] 「另起炉灶」，此时单独的一个 nums[i] 的值，就是 dp[i]。
    
        以上两种情况的最大值就是 `dp[i]` 的值，写出如下状态转移方程：
    
        ![image-20220423152015663](image-20220423152015663-1662463137547106.png)
    
        状态转移方程还可以这样写，反正求的是最大值，也不用分类讨论了，就这两种情况，取最大即可，因此还可以写出状态转移方程如下：
    
        ![image-20220423152050928](image-20220423152050928-1662463137547107.png)
    
        友情提示：求解动态规划的问题经常要分类讨论，这是因为动态规划的问题本来就有「最优子结构」的特点，即大问题的最优解通常由小问题的最优解得到。因此我们在设计子问题的时候，就需要把求解出所有子问题的结果，进而选出原问题的最优解。
    
        思考初始值
    
        `dp[0]` 根据定义，只有 1 个数，一定以 `nums[0]` 结尾，因此 `dp[0] = nums[0]`。
    
        思考输出
    
        **注意**：这里状态的定义不是题目中的问题的定义，**不能直接将最后一个状态返回回去**，而要返回dp数组的最大值
    
    25. 跳跃游戏(55)：
    
        方法一：
    
        根据题目所给条件`0 <= nums[i] <= 105`，如果数组中的数字全大于0，那么每次跳一步一定可以到达终点，那么就只需判断等于0的位置，看在等于0的位置前面有没有位置可以跳过0，这样又能化归到全部大于0的情况，~~偷鸡做法~~。
    
        > 开始想用回溯法来做，会超时，思路是从起点开始遍历所有可能走的情况看最终能不能走到终点，类似dfs
    
        ```java
        public boolean canJump(int[] nums) {
            //[2,0,0]，这种情况需要特别判断
            if (nums.length==1){
                return true;
            }
            for (int i = 0; i < nums.length; i++) {
                if (nums[i]==0){
                    int temp=i-1;
                    boolean canJumpZero=false;
                    while (temp>=0){
                        //如果从temp位置可以直接跳到结尾当然也能跳过0，这是防止结尾的数是0导致算法不正确
                        if (nums[temp]>=i-temp+1||nums[temp]>=nums.length-1-temp){
                            canJumpZero=true;
                            break;
                        }
                        temp--;
                    }
                    if (!canJumpZero){//不能跳过零就会卡在零的位置直接返回false即可
                        return false;
                    }
                }
            }
            return true;
        }
        ```
    
        方法二：贪心
    
        设想一下，对于数组中的任意一个位置 y，我们如何判断它是否可以到达？根据题目的描述，只要存在一个位置 x，它本身可以到达，并且它跳跃的最大长度为 x+nums[x]，这个值大于等于 y，即x+nums[x]≥y，那么位置 y 也可以到达。
    
        换句话说，对于每一个可以到达的位置 x，它使得 x+1,x+2,⋯,x+nums[x] 这些连续的位置都可以到达。
    
        这样以来，我们依次遍历数组中的每一个位置，并实时维护 最远可以到达的位置。对于当前遍历到的位置 x，如果它在 最远可以到达的位置 的范围内，那么我们就可以从起点通过若干次跳跃到达该位置，因此我们可以用 x+nums[x] 更新 最远可以到达的位置。
    
        在遍历的过程中，如果 最远可以到达的位置 大于等于数组中的最后一个位置，那就说明最后一个位置可达，我们就可以直接返回 True 作为答案。反之，如果在遍历结束后，最后一个位置仍然不可达，我们就返回 False 作为答案。
    
        以题目中的示例一[2, 3, 1, 1, 4]为例：
    
        我们一开始在位置 0，可以跳跃的最大长度为 2，因此最远可以到达的位置被更新为 2；
    
        我们遍历到位置 1，由于1≤2，因此位置 1 可达。我们用 1 加上它可以跳跃的最大长度 3，将最远可以到达的位置更新为 1+3=4。由于 4 大于等于最后一个位置 4，因此我们直接返回 True。
    
        ```java
        public boolean canJump(int[] nums) {
            int cur=0;
            int canReach=cur+nums[cur];
            for (int i = 1; i < nums.length; i++) {
                if (canReach>= nums.length-1){
                    return true;//稍微优化一下，如果能够到达终点就直接返回true了不要继续遍历
                }
                if (canReach>=i){
                    canReach=Math.max(canReach,i+nums[i]);
                }else{
                    return false;
                }
            }
            return true;
        }
        ```
    
    26. 合并区间(56)：
    
        如果我们按照区间的左端点排序，那么在排完序的列表中，可以合并的区间一定是连续的。如下图所示，标记为蓝色、黄色和绿色的区间分别可以合并成一个大区间，它们在排完序的列表中是连续的：
    
        ![image-20220423194439426](image-20220423194439426-1662463137547108.png)
    
        我们用数组 merged 存储最终的答案。
    
        首先，我们将列表中的区间按照左端点升序排序。然后我们将第一个区间加入 merged 数组中，并按顺序依次考虑之后的每个区间：
    
        - 如果当前区间的左端点在数组 merged 中最后一个区间的右端点之后，那么它们不会重合，我们可以直接将这个区间加入数组 merged 的末尾；
    
        - 否则，它们重合，我们需要用当前区间的右端点更新数组 merged 中最后一个区间的右端点，将其置为二者的较大值。举几个例子，`[1,3] 和 [3,6]`，`[1,3]和[2,6]`，但当前区间的左端点一定小于数组末尾的左端点，因为前面已经排过序了
    
        ```java
        public int[][] merge(int[][] intervals) {
            Arrays.sort(intervals, (o1, o2) -> o1[0]-o2[0]);//按照数组的第一个元素升序排序
            List<int[]> merged=new ArrayList<>();
            for (int i = 0; i < intervals.length; i++) {
                int left=intervals[i][0];
                int right=intervals[i][1];
                //当前区间的左端点在数组 merged 中最后一个区间的右端点之后
                if (merged.size()==0||merged.get(merged.size()-1)[1]<left){
                    merged.add(intervals[i]);
                }else{//重置数组末尾的右端点
                    merged.get(merged.size()-1)[1]=Math.max(merged.get(merged.size()-1)[1],right);
                }
            }
            return merged.toArray(new int[merged.size()][]);
        }
        ```
    
    27. 不同路径(62)：
    
        方法一：递归，~~虽然不能AC~~还是贴上代码
    
        ```java
        public void findPaths(int m, int n, int x, int y, int[] paths) {
            if (x == m && y == n) {
                paths[0]++;
                return;
            }
            if (x > m || y > n) {
                //越界就直接返回
                return;
            }
            findPaths(m, n, x + 1, y, paths);//向下走
            findPaths(m, n, x, y + 1, paths);//向右走
        }
        
        public int uniquePaths(int m, int n) {
            int[] paths = new int[1];//传数组进去是为了能在函数里面修改值
            findPaths(m, n, 1, 1, paths);
            return paths[0];
        }
        ```
    
        方法二：动态规划
    
        用f（i，j）表示从左上角走到第i行第j列位置的路径总数，1=<i<=m，1=<j<=n
    
        由于我们每一步只能从向下或者向右移动一步，因此要想走到 (i, j)，如果向下走一步，那么会从 (i-1, j) 走过来；如果向右走一步，那么会从 (i, j-1) 走过来。
    
        因此我们可以写出动态规划转移方程：`f(i, j) = f(i-1, j) + f(i, j-1)`
        需要注意**初始条件**，dp [1] [1] = 1，表示从（1，1）走到（1，1）有一种走法，这是为了保证m和n等于一时代码也是正确的，其次是第一行和第一列都是1，因为只能向右走和向下走
    
        ```java
        public int uniquePaths(int m, int n) {
            int[][] dp = new int[m + 1][n + 1];
            dp[1][1] = 1;
            for (int i = 2; i < m + 1; i++) {
                dp[i][1] = 1;
            }
            for (int i = 2; i < n + 1; i++) {
                dp[1][i] = 1;
            }
            for (int i = 2; i < m + 1; i++) {
                for (int j = 2; j < n + 1; j++) {
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
                }
            }
            return dp[m][n];
        }
        ```
    
        总结：
    
        **动态规划的解题套路**
    
        什么样的问题可以考虑使用动态规划解决呢？
    
        如果一个问题，可以把所有可能的答案穷举出来，并且穷举出来后，发现存在重叠子问题，就可以考虑使用动态规划。比如一些求最值的场景，如**最长递增子序列、最小编辑距离、背包问题、凑零钱问题**等等，都是动态规划的经典应用场景。
    
        动态规划的核心思想就是**拆分子问题，记住过往，减少重复计算。** 并且动态规划一般都是自底向上的，因此到这里总结了一下我做动态规划的思路：
    
        - 穷举分析
        - 确定边界
        - 找出规律，确定最优子结构
        - 写出状态转移方程
    
        **动态规划是自底向上解决问题，递归是自顶向下解决问题**
    
    28. 最小路径和(64)：
    
        与62题的动态规划大同小异，由于路径的方向只能是向下或向右，因此网格的第一行的每个元素只能从左上角元素开始向右移动到达，网格的第一列的每个元素只能从左上角元素开始向下移动到达，此时的路径是唯一的，因此每个元素对应的最小路径和即为对应的路径上的数字总和。
    
        对于不在第一行和第一列的元素，可以从其上方相邻元素向下移动一步到达，或者从其左方相邻元素向右移动一步到达，元素对应的最小路径和等于其上方相邻元素与其左方相邻元素两者对应的最小路径和中的最小值加上当前元素的值。由于每个元素对应的最小路径和与其相邻元素对应的最小路径和有关，因此可以使用动态规划求解。
    
        创建二维数组 dp，与原始网格的大小相同，dp [i] [j] 表示从左上角出发到 (i,j) 位置的最小路径和。显然，dp[0] [0]=grid[0] [0]。对于 dp 中的其余元素，通过以下状态转移方程计算元素值。
    
        当 i>0 且 j=0 时，`dp [i] [j]=dp [i-1] [0]+grid[i] [0]`
    
        当 i=0 且 j>0 时，`dp [i] [j]=dp[0] [j-1]+grid[0] [j]`
    
        当 i>0 且 j>0 时，`dp[i] [j]=Math.min(dp[i-1] [j],dp[i] [j-1])+grid[i] [j]`
    
        最后得到 dp[m-1] [n-1] 的值即为从网格左上角到网格右下角的最小路径和。
    
        ```java
        public int minPathSum(int[][] grid) {
            int[][] dp=new int[grid.length][grid[0].length];
            dp[0][0]=grid[0][0];
            int m=grid.length,n=grid[0].length;
            for (int i = 1; i < m; i++) {
                dp[i][0]=dp[i-1][0]+grid[i][0];
            }
            for (int i = 1; i < n; i++) {
                dp[0][i]=dp[0][i-1]+grid[0][i];
            }
            for (int i = 1; i < m; i++) {
                for (int j = 1; j < n; j++) {
                    dp[i][j]=Math.min(dp[i-1][j],dp[i][j-1])+grid[i][j];
                }
            }
            return dp[m-1][n-1];
        }
        ```
    
    29. 爬楼梯(70)：
    
        方法一：动态规划
    
        经典的动态规划和递归的问题，比较简单直接贴代码
    
        ```java
        public int climbStairs(int n) {
            if (n==1){
                return 1;
            }
            int[] dp = new int[n + 1];
            dp[1] = 1;
            dp[2] = 2;
            for (int i = 3; i < n + 1; i++) {
                dp[i]=dp[i-1]+dp[i-2];
            }
            return dp[n];
        }
        ```
    
        方法二：矩阵快速幂
    
        ![image-20220423221916169](image-20220423221916169-1662463137547109.png)
    
        快速幂算法求$M^n$可以使时间复杂度降为O(log n),下面简单介绍一下快速幂算法
    
        **递归快速幂**
    
        ![image-20220423230325266](image-20220423230325266-1662463137547110.png)
    
        计算a的n次方，如果n是偶数（不为0），那么就**先计算a的n/2次方，然后平方**；如果n是奇数，那么就**先计算a的n-1次方，再乘上a**；递归出口是**a的0次方为1**。
    
        递归快速幂的思路非常自然，代码也很简单（直接把递归方程翻译成代码即可）：
    
        ```java
        //递归快速幂
        public int quickPow(int a, int n)
        {
            if (n == 0)
                return 1;
            else if (n % 2 == 1)
                return qpow(a, n - 1) * a;
            else
            {
                int temp = qpow(a, n / 2);
                return temp * temp;
            }
        }
        ```
    
        注意，这个temp变量是必要的，因为如果不把quickPow算出的结果记录下来，直接写成quickPow(a, n /2)*quickPow(a, n /2)，那会计算两次a^(n/2)，整个算法就退化为了 O(n)。
    
        在实际问题中，题目常常会要求对一个大素数取模，这是因为计算结果可能会非常巨大，但是在这里考察高精度又没有必要。这时我们的快速幂也应当进行取模，此时应当注意，原则是**步步取模**，如果MOD较大，还应当**开long long**。
    
        递归虽然**简洁**，但会产生**额外的空间开销**。我们可以把递归改写为循环，来避免对栈空间的大量占用，也就是**非递归快速幂**。
    
        **非递归快速幂**
    
        ```java
        //非递归快速幂
        public int quickPow(int a, int n){
            int ans = 1;
            while(n>0){
                if((n&1)==1) {       //如果n的当前末位为1
                    ans *= a;
                }//ans乘上当前的a
                a *= a;        //a自乘
                n >>= 1;       //n往右移一位
            }
            return ans;
        }
        ```
    
        上面这段代码可以作为非递归快速幂的模板代码，尽量记住
    
        矩阵的快速幂只需要把上面代码中的乘法改成矩阵乘法，把ans初始化为单位矩阵即可，下面贴上代码
    
        ```java
        public int[][] matrixMultiply(int[][] a, int[][] b) {
            int[][] c = new int[2][2];
            for (int i = 0; i < 2; i++) {
                for (int j = 0; j < 2; j++) {
                    c[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
                }
            }
            return c;
        }
        
        public int[][] quickPow(int[][] a, int n) {
            int[][] res = {{1, 0}, {0, 1}};//初始化为单位矩阵
            while (n > 0) {
                if ((n & 1) == 1) {
                    res = matrixMultiply(res, a);
                }
                a = matrixMultiply(a, a);//a=a*a
                n = n >> 1;
            }
            return res;
        }
        
        public int climbStairs(int n) {
            int[][] matrix = {{1, 1}, {1, 0}};
            int[][] res = quickPow(matrix, n);
            return res[1][0]+res[1][1];
        }
        ```
    
    30. 编辑距离(72)：
    
        动态规划问题，dp[i] [j]表示word1中长度为i的字符串（下标从 0 到 i-1 ）转换成word2中长度为j的字符串（下标从 0 到 j-1 ）所需要的最小步数。
    
        状态转移方程：
    
        当 `word1[i] == word2[j]，dp[i] [j] = dp[i-1] [j-1]`；
    
        当 `word1[i] != word2[j]，dp[i] [j] = min(dp[i-1] [j-1], dp[i-1] [j], dp[i] [j-1]) + 1`
    
        其中，`dp[i-1] [j-1]`表示**替换**word1[i-1]为word2[j-1]操作，`dp[i-1] [j]` 表示**删除**word1[i-1]操作，`dp[i] [j-1]` 表示在word1后**插入**word[j-1]操作。三种操作中取步骤数最小的然后加上本次操作。 
    
        注意，针对第一行，第一列要单独考虑，我们引入 `''` 下图所示：
    
        ![image-20220424201017152](image-20220424201017152-1662463137547111.png)
    
        第一行，是 `word1` 为空变成 `word2` 最少步数，就是插入操作
    
        第一列，是 `word2` 为空，需要的最少步数，就是删除操作
    
        ```java
        public int minDistance(String word1, String word2) {
            //动态规划，dp数组里的i j分别表示word1和word2的长度
            int[][] dp = new int[word1.length() + 1][word2.length() + 1];
            dp[0][0] = 0;
            for (int i = 1; i < word1.length() + 1; i++) {
                dp[i][0] = dp[i - 1][0] + 1;
            }
            for (int i = 1; i < word2.length() + 1; i++) {
                dp[0][i] = dp[0][i - 1] + 1;
            }
            for (int i = 1; i < word1.length() + 1; i++) {
                for (int j = 1; j < word2.length() + 1; j++) {
                    if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                        dp[i][j] = dp[i - 1][j - 1];
                    } else {
                        dp[i][j] = Math.min(dp[i - 1][j], Math.min(dp[i][j - 1], dp[i - 1][j - 1])) + 1;
                    }
                }
            }
            return dp[word1.length()][word2.length()];
        }
        ```
    
    31. 颜色分类(75)：
    
        方法一：~~偷鸡做法~~
    
        因为一共只有三种ya颜色分别是0，1，2，所以遍历一遍原数组找出每种颜色有多少，在按照题目要求的颜色的顺序填回到原数组即可
    
        ```java
        public void sortColors(int[] nums) {
            int[] colors=new int[3];
            for (int i = 0; i < nums.length; i++) {
                colors[nums[i]]++;
            }
            int indexColors=0,indexRes=0;
            while (indexColors<3){
                while (colors[indexColors]>0){
                    nums[indexRes]=indexColors;
                    colors[indexColors]--;
                    indexRes++;
                }
                indexColors++;
            }
        }
        ```
    
        方法二：
    
        直接~~调用排序AP~~I来做就好，自己手写排序算法即可
    
    32. 最小覆盖子串(76)：**滑动窗口**
    
        本问题要求我们返回字符串 s 中包含字符串 t 的全部字符的最小窗口。我们称包含 t 的全部字母的窗口为「可行」窗口。
    
        我们可以用滑动窗口的思想解决这个问题。在滑动窗口类型的问题中都会有两个指针，一个用于「延伸」现有窗口的 r 指针，和一个用于「收缩」窗口的 l 指针。在任意时刻，只有一个指针运动，而另一个保持静止。**我们在 s 上滑动窗口，通过移动 r 指针不断扩张窗口。当窗口包含 t 全部所需的字符后，如果能收缩，我们就收缩窗口直到得到最小窗口。如果收缩后不满足条件，则让r指针不断扩张窗口，寻找新的满足条件的滑动窗口。**一直重复此过程直到 j 超出了字符串 S 范围。
    
        ![image-20220425220202755](image-20220425220202755.png)
    
        这道题目的解题代码中就用了两个HashMap来实现并维护s中出现的需要的字符数量表（简称已有字符表），实现了一次遍历统计出t中每个字符的数量（简称所需字符表），可以好好领悟一下HashMap的使用
    
        ```java
        public class MinWindow {
            HashMap<Character, Integer> ori = new HashMap<Character, Integer>();
            HashMap<Character, Integer> cnt = new HashMap<Character, Integer>();
            public String minWindow(String s, String t) {
                if (s==null|| s.equals("") ||t==null||t.equals("")||s.length()<t.length()){
                    return "";
                }
                int tLen=t.length();
                //利用HashMap的特点统计t中每个字符出现的次数，getOrDefault经典操作
                for (int i = 0; i < tLen; i++) {
                    char c=t.charAt(i);
                    ori.put(c, ori.getOrDefault(c,0)+1);
                }
                //l窗口的左边界，r窗口的右边界,r初始化为-1是为了匹配下面while循环一开始就r++，len是窗口的最小长度
                int l=0,r=-1;
                int len=Integer.MAX_VALUE,ansL=-1,ansR=-1;
                int sLen=s.length();
                while (r<sLen){
                    r++;
                    //向右扩展一格窗口，如果此时右边界的字符是所需要的字符，则更新已有字符表
                    if (r<sLen&& ori.containsKey(s.charAt(r))){
                        cnt.put(s.charAt(r),cnt.getOrDefault(s.charAt(r),0)+1);
                    }
                    //check满足条件后才开始不断收缩左边界直到不满足包含t中所有字符
                    //l<=r,窗口会不断收缩l++但l不能超过r可以等于r
                    while (check()&&l<=r){
                        if (r-l+1<len){//更新窗口的最小长度
                            len=r-l+1;
                            ansL=l;
                            ansR=r;
                        }
                        //收缩的过程中如果左边界的字符是t所需要的，l++后该字符就没有了，需要更新已有字符表
                        if (ori.containsKey(s.charAt(l))){
                            cnt.put(s.charAt(l),cnt.get(s.charAt(l))-1);
                        }
                        l++;
                    }
                }
                return (ansL==-1&&ansR==-1)?"":s.substring(ansL,ansR+1);
            }
            public boolean check(){
                for (Map.Entry<Character, Integer> entry :
                     ori.entrySet()) {
                    Character key = entry.getKey();
                    Integer value= entry.getValue();
                    //s的子串的某些字符的数量大于等于t中每一个字符的数量都是可以的，但如果某一个小于就不符合条件了
                    if (cnt.getOrDefault(key,0)<value){
                        return false;
                    }
                }
                return true;
            }
        }
        ```
    
    33. 子集(78)：
    
        方法一：迭代法实现子集枚举
    
        用01序列表示特征函数，1表示该位置的数在子集里，0表示该位置的数不在子集里，可以发现 01 序列对应的二进制数正好从 0 到 2^n - 1，这样我们可以吗，枚举完所有的情况
    
        ```java
        public void update(int[] index) {
            for (int i = 0; i < index.length; i++) {//枚举01序列的函数
                if (index[i] == 0) {
                    index[i] = 1;
                    break;
                } else {
                    index[i] = 0;
                }
            }
        }
        
        public List<List<Integer>> subsets(int[] nums) {
            int[] index = new int[nums.length];
            //00 10 01 11
            //000 100 010 110 001 101 011 111
            int cnt = (int) Math.pow(2, nums.length);//计算出需要枚举的次数
            List<List<Integer>> res = new ArrayList<>();
            for (int i = 0; i < cnt; i++) {
                List<Integer> temp = new ArrayList<>();
                for (int j = 0; j < index.length; j++) {
                    if (index[j] == 1) {
                        temp.add(nums[j]);
                    }
                }
                res.add(temp);
                update(index);
            }
            return res;
        }
        ```
    
        方法二：递归法实现子集枚举
    
        对于集合中的每一个数，只有两种情况，选取该数字加入子集中，不选取该数字，和上面的01序列相似
    
        ```java
        public void dfs(List<List<Integer>> res, List<Integer> temp,int cur,int[] nums){
            if (cur== nums.length){
                res.add(new ArrayList<>(temp));
                return;
            }
            temp.add(nums[cur]);//取当前位置的数字
            dfs(res, temp, cur+1, nums);
            temp.remove(temp.size()-1);//不取当前位置的数字
            dfs(res, temp, cur+1, nums);
        }
        public List<List<Integer>> subsets(int[] nums) {
            List<List<Integer>> res = new ArrayList<>();
            dfs(res,new ArrayList<>(),0,nums);
            return res;
        }
        ```
    
    34. 单词搜索(79)：
    
        与递归走迷宫类似，从某一个起点出发，递归的向上走，向下走，向左走，向右走，看四个方向的哪个方向可以满足单词的字符，如果都不能满足直接返回false即可，还要注意不能走回头，走过的路不能重复再走一次，需要维护一个`boolean[] isVisited`，访问过就标记为已访问。如果此路不通则要回退为没用访问过
    
        ```java
        public class Exist {
            public boolean findWord(char[][] board, String word, int x, int y, int wordIndex, boolean[][] isVisited) {
                if (wordIndex == word.length()) {
                    return true;//搜索到单词的末尾返回true
                }
                if (x + 1 < board.length && board[x + 1][y] == word.charAt(wordIndex) && !isVisited[x + 1][y]) {
                    isVisited[x + 1][y] = true;//先标记为已访问再递归进行搜索
                    if (findWord(board, word, x + 1, y, wordIndex + 1, isVisited)) {
                        return true;
                    } else {
                        isVisited[x + 1][y] = false;//如果此路不通需要回退为没用访问过
                    }
                }
                if (x - 1 >= 0 && board[x - 1][y] == word.charAt(wordIndex) && !isVisited[x - 1][y]) {
                    isVisited[x - 1][y] = true;
                    if (findWord(board, word, x - 1, y, wordIndex + 1, isVisited)) {
                        return true;
                    } else {
                        isVisited[x - 1][y] = false;
                    }
                }
                if (y + 1 < board[0].length && board[x][y + 1] == word.charAt(wordIndex) && !isVisited[x][y + 1]) {
                    isVisited[x][y + 1] = true;
                    if (findWord(board, word, x, y + 1, wordIndex + 1, isVisited)) {
                        return true;
                    } else {
                        isVisited[x][y + 1] = false;
                    }
                }
                if (y - 1 >= 0 && board[x][y - 1] == word.charAt(wordIndex) && !isVisited[x][y - 1]) {
                    isVisited[x][y - 1] = true;
                    if (findWord(board, word, x, y - 1, wordIndex + 1, isVisited)) {
                        return true;
                    } else {
                        isVisited[x][y - 1] = false;
                    }
                }
                return false;
            }
        
            public boolean exist(char[][] board, String word) {
                if (board.length * board[0].length < word.length()) {
                    return false;//判断特殊情况直接返回false
                }
                ArrayList<Point> points = new ArrayList<>();
                for (int i = 0; i < board.length; i++) {
                    for (int j = 0; j < board[i].length; j++) {
                        if (board[i][j] == word.charAt(0)) {
                            points.add(new Point(i, j));//找到满足第一个字符的所有位置，依次从这些位置开始搜索
                        }
                    }
                }
                for (Point start :
                     points) {
                    boolean[][] isVisited = new boolean[board.length][board[0].length];
                    isVisited[start.x][start.y] = true;//将起始位置标记为已访问，这一步很重要!
                    //防止后面递归搜索路径的时候又搜索到开始的位置
                    if (findWord(board, word, start.x, start.y, 1, isVisited)) {
                        return true;//有一条路存在直接返回true
                    }
                }
                return false;
            }
        }
        
        class Point {
            public int x;
            public int y;
        
            public Point(int x, int y) {
                this.x = x;
                this.y = y;
            }
        
        }
        ```
    
    35. 柱状图中最大的矩形(84)：这道题可以好好的理解一下**单调栈**
    
        方法一：暴力模拟（超时）
    
        依次遍历柱形的高度，对于每一个高度分别向两边扩散，求出以当前高度为矩形的最大宽度多少。
    
        为此，我们需要：
    
        左边看一下，看最多能向左延伸多长，找到大于等于当前柱形高度的最左边元素的下标；
    
        右边看一下，看最多能向右延伸多长；找到大于等于当前柱形高度的最右边元素的下标。
    
        对于每一个位置，我们都这样操作，得到一个矩形面积，求出它们的最大值。
    
        ```java
        public int largestRectangleArea(int[] heights) {
            int len = heights.length;
            if (len == 0) {
                return 0;//特殊情况直接返回
            }
            int res = 0;
            for (int i = 0; i < len; i++) {
                int left = i;
                while (left - 1 >= 0 && heights[left - 1] >= heights[i]) {//先判断前一位是否满足条件再left--
                    left--;
                }
                int right = i;
                while (right + 1 < len && heights[right + 1] >= heights[i]) {
                    right++;
                }
                int temp = heights[i] * (right - left + 1);
                if (temp > res) {
                    res = temp;
                }
            }
            return res;
        }
        ```
    
        方法二：**单调栈**
    
        先说明一下单调栈是什么
    
        - 单调递增栈：从**栈底到栈顶**，栈中的值单调递增
        - 单调递减栈：从**栈底到栈顶**，栈中的值单调递减
    
        单调栈则主要用于解决**NGE问题**（Next Greater Element），也就是，对序列中每个元素，找到下一个比它大的元素。（当然，“下一个”可以换成“上一个”，“比它大”也可以换成“比他小”，原理不变。）
    
        我们维护一个栈，表示“**待确定NGE的元素**”，然后遍历序列。当我们碰上一个新元素，我们知道，越靠近栈顶的元素离新元素位置越近。所以不断比较新元素与栈顶，如果新元素比栈顶大，则可断定新元素就是栈顶的NGE，于是弹出栈顶并继续比较。直到新元素不比栈顶大，再将新元素压入栈。显然，这样形成的栈是单调递减的。
    
        我们归纳一下枚举「高」的方法：
    
        首先我们枚举某一根柱子 i 作为高 h=heights[i]；
    
        随后我们需要进行向左右两边扩展，使得**扩展到的柱子的高度均不小于 h**。换句话说，我们需要**找到左右两侧最近的高度小于 h 的柱子**，这样这两根柱子之间（不包括其本身）的所有柱子高度均不小于 h，并且就是 i 能够扩展到的最远范围。
    
        那么我们先来看看如何求出**一根柱子的左侧且最近的小于其高度的柱子**，可以用单调递增栈来实现，栈中存放的是每根柱子的下标
    
        栈中存放了 j 值。从栈底到栈顶，j 的值严格单调递增，同时对应的高度值也严格单调递增；
    
        当我们枚举到第 i 根柱子时，我们从栈顶不断地移除 height[j]≥height[i] 的 j 值。在移除完毕后，栈顶的 j 值就一定满足 height[j]<height[i]，此时 j 就是 i 左侧且最近的小于其高度的柱子，我们再将 i 放入栈顶。
    
        这里会有一种特殊情况。如果我们移除了栈中所有的 j 值，那就说明 i 左侧所有柱子的高度都大于等于 height[i]，那么我们可以认为 i 左侧且最近的小于其高度的柱子在位置 j=-1，它是一根「虚拟」的、高度无限低的柱子。这样的定义不会对我们的答案产生任何的影响，我们也称这根「虚拟」的柱子为「哨兵」。相对应的最右侧也会有哨兵。
    
        下面的代码是通过从左向右和从右向左两次遍历分别求出一根柱子的左侧且最近的小于其高度的柱子，一根柱子的右侧且最近的小于其高度的柱子
    
        ```java
        public int largestRectangleArea(int[] heights) {
            int n = heights.length;
            int[] left = new int[n];
            int[] right = new int[n];
            Deque<Integer> stack = new LinkedList<>();
            for (int i = 0; i < n; i++) {
                while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                    stack.pop();
                }
                if (stack.isEmpty()) {
                    left[i] = -1;
                } else {
                    left[i] = stack.peek();
                }
                stack.push(i);
            }
            stack.clear();
            for (int i = n - 1; i >= 0; i--) {
                while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
                    stack.pop();
                }
                if (stack.isEmpty()) {
                    right[i] = n;
                } else {
                    right[i] = stack.peek();
                }
                stack.push(i);
            }
            int res = 0;
            for (int i = 0; i < n; i++) {
                res = Math.max(res, (right[i] - left[i] - 1) * heights[i]);
            }
            return res;
        }
        ```
    
        除了两次遍历之外，我们还可以对该方法进行优化，只用一次遍历就找出left和right数组.
    
        在方法一中，我们在对位置 i 进行入栈操作时，确定了它的左边界。从直觉上来说，与之对应的我们在对位置 i 进行出栈操作时可以确定它的右边界！仔细想一想，这确实是对的。当位置 i 被弹出栈时，说明此时遍历到的位置的高度小于等于 height[i]，并且在 i0 与 i 之间没有其他高度小于等于 height[i] 的柱子。这是因为，如果在 i 和i0之间还有其它位置的高度小于等于 height[i] 的，那么在遍历到那个位置的时候，i 应该已经被弹出栈了。所以位置 i0就是位置 i的右边界。
    
        等等，我们需要的是「一根柱子的左侧（或右侧）且最近的小于其高度的柱子」，但这里我们求的是小于等于，那么会造成什么影响呢？答案是：我们确实无法求出正确的右边界，但对最终的答案没有任何影响。这是因为在答案对应的矩形中，如果有若干个柱子的高度都等于矩形的高度，那么**最右侧的那根柱子是可以求出正确的右边界**的，而我们没有对求出左边界的算法进行任何改动，因此最终的答案还是可以从最右侧的与矩形高度相同的柱子求得的。下面举一个例子来说明这一点
    
        ![image-20220502225551702](image-20220502225551702-1662463137547113.png)
    
        该算法有两个规则：
    
        - 当有柱子假设为柱子i入栈时，栈顶的柱子就是柱子i的左边界（因为是单调递增栈）
        - 当有柱子（记为柱子i）出栈时，此时遍历到的柱子（记为index）的高度小于等于柱子i，柱子i的右边界为柱子index
    
        首先要明确当有柱子出栈和入栈时，可以确定柱子的左边界或右边，还要时刻记得栈中的数据从栈底到栈顶是严格递增的。如上图所示，当我们遍历到柱子4的时候，栈中只有柱子3（因为柱子4最近的小于其高度的柱子是柱子3，如果不明白可以从头开始推），我们将柱子4入栈，那么可以确定柱子4的左边界为柱子3，然后遍历到柱子5，此时柱子5的高度大于等于柱子4，柱子4将出栈，按照规则此时可以确定柱子4的右边界是柱子5（虽然柱子5不是右边最近的小于其高度的柱子，耐心接着往下看），然后柱子5比栈中的柱子3高，柱子5入栈，此时可以确定柱子5的左边界是柱子3，然后接着遍历到了柱子6，柱子6的高度小于此时栈顶柱子（为柱子5）的高度，柱子5出栈，可以确定柱子5的右边界为柱子6，所以柱子5的边界为柱子3和柱子6，虽然柱子4的右边界不正确。至此就可以理解如果有若干个柱子的高度都等于矩形的高度，那么**最右侧的那根柱子是可以求出正确的右边界**的.
    
        在遍历结束后，栈中仍然有一些位置，这些位置对应的右边界就是位置为 n 的「哨兵」。我们可以将它们依次出栈并更新右边界，也可以在初始化右边界数组时就将所有的元素的值置为 n。
    
        ```java
        public int largestRectangleArea(int[] heights){
            int n=heights.length;
            int[] left=new int[n];
            int[] right=new int[n];
            Arrays.fill(right,n);//初始化右边界哨兵为n
            Deque<Integer> stack = new LinkedList<>();
            for (int i = 0; i < n; i++) {
                while (!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                    right[stack.peek()]=i;
                    stack.pop();
                }
                if (stack.isEmpty()){
                    left[i]=-1;
                }else{
                    left[i]=stack.peek();
                }
                stack.push(i);
            }
            int res=0;
            for (int i = 0; i < n; i++) {
                res=Math.max(res,(right[i]-left[i]-1)*heights[i]);
            }
            return res;
        }
        ```
    
    36. 最大矩形(85)：
    
        如果能发现这道题是上一题的一个变种就比较好做，题目要求的是在二维矩阵中找出最大矩形，可以**一行一行的看**，当作是**在一维数组中找到最大的矩形**，如下图所示
    
        ![image-20220507143621905](image-20220507143621905.png)
    
        需要注意的是，比如说以第四行为坐标轴的时候，第四行第二列的元素为0，那么这一行的高度就为0，而不要考虑上面有多高，不会出现悬浮的列，这样才能和84题统一。
    
        ```java
        public int largestRectangleArea(int[] heights){
            int n=heights.length;
            int[] left=new int[n];
            int[] right=new int[n];
            Arrays.fill(right,n);//记得初始化右哨兵为n
            Deque<Integer> stack=new LinkedList<>();//依旧是单调栈解法找出第一个比他小的元素
            for (int i = 0; i < n; i++) {
                while (!stack.isEmpty()&&heights[stack.peek()]>=heights[i]){
                    right[stack.pop()]=i;
                }
                if (stack.isEmpty()){
                    left[i]=-1;
                }else{
                    left[i]=stack.peek();
                }
                stack.push(i);
            }
            int res=0;
            for (int i = 0; i < n; i++) {
                res=Math.max(res,heights[i]*(right[i]-left[i]-1));
            }
            return res;
        }
        public int maximalRectangle(char[][] matrix) {
            if (matrix.length==0){
                return 0;
            }
            int[] heights=new int[matrix[0].length];
            int maxArea=0;
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (matrix[i][j]=='1'){
                        heights[j]+=1;
                    }else{
                        heights[j]=0;
                    }
                }
                maxArea=Math.max(maxArea,largestRectangleArea(heights));
            }
            return maxArea;
        }
        ```
    
    37. 二叉树的中序遍历(94)：
    
        简单题，一定要记得二叉树的三种递归遍历方式
    
    38. 不同的二叉搜索树(96)：
    
        这道题是一个[卡塔兰树](https://baike.baidu.com/item/catalan/7605685?fr=aladdin)问题，~~虽然我不知道卡塔兰树~~
    
        题目要求是计算不同二叉搜索树的个数。为此，我们可以定义两个函数：
    
        G(n): 长度为 n 的序列能构成的不同二叉搜索树的个数。
    
        F(i, n): 以 i 为根、序列长度为 n 的不同二叉搜索树个数 (1≤i≤n)。
    
        可见，G(n) 是我们求解需要的函数。稍后我们将看到，G(n) 可以从 F(i, n) 得到，而 F(i, n)又会递归地依赖于 G(n)。不同的二叉搜索树的总数 G(n)，是对遍历所有 i (1≤i≤n) 的 F(i, n) 之和。对于边界情况，当序列长度为 1（只有根）或为 0（空树）时，只有一种情况，即：G(0)=1,G(1)=1
    
        给定序列 1 2 3 ⋯ n，我们选择数字 i 作为根，则根为 i 的所有二叉搜索树的集合是左子树集合和右子树集合的笛卡尔积，对于笛卡尔积中的每个元素，加上根节点之后形成完整的二叉搜索树，如下图所示：
    
        ![image-20220507211036783](image-20220507211036783-1662463137547115.png)
    
        注意到 G(n)和序列的内容无关，只和序列的长度有关。 因此，我们可以得到以下公式：
    
        `F(i, n) = G(i-1)*G(n-i)`
        最终G(n)的递归表达式为：
    
        ![image-20220507211414749](image-20220507211414749-1662463137547116.png)
    
        至此，我们从小到大计算 G函数即可，因为 G(n)的值依赖于 G(0)⋯G(n−1)。
    
        ```java
        public int numTrees(int n) {
            if (n==0||n==1){
                return 1;
            }
            int[] dp=new int[n+1];
            dp[0]=1;
            dp[1]=1;
            for (int i = 2; i <= n; i++) {
                for (int j = 1; j <= i; j++) {
                    dp[i]+=dp[j-1]*dp[i-j];
                }
            }
            return dp[n];
        }
        ```
    
    39. 验证二叉搜索树(98)：
    
        方法一：递归
    
        由题目给出的信息我们可以知道：如果该二叉树的左子树不为空，则左子树上所有节点的值均小于它的根节点的值； 若它的右子树不空，则右子树上所有节点的值均大于它的根节点的值；它的左右子树也为二叉搜索树。
    
        这启示我们设计一个递归函数 helper(root, lower, upper) 来递归判断，函数表示考虑以 root 为根的子树，判断子树中所有节点的值是否都在 (l,r) 的范围内（注意是开区间）。如果 root 节点的值 val 不在 (l,r) 的范围内说明不满足条件直接返回，否则我们要继续递归调用检查它的左右子树是否满足，如果都满足才说明这是一棵二叉搜索树。
    
        那么根据二叉搜索树的性质，在递归调用左子树时，我们需要把上界 upper 改为 root.val，即调用 helper(root.left, lower, root.val)，因为左子树里所有节点的值均小于它的根节点的值。同理递归调用右子树时，我们需要把下界 lower 改为 root.val，即调用 helper(root.right, root.val, upper)。
    
        函数递归调用的入口为 helper(root, -inf, +inf)， inf 表示一个无穷大的值。
    
        ```java
        public boolean helper(TreeNode node,long low,long high){
            if (node==null){
                return true;
            }
            if (node.val<=low||node.val>=high){
                return false;
            }
            return helper(node.left,low,node.val)&&helper(node.right,node.val,high);
        }
        public boolean isValidBST(TreeNode root) {
            return helper(root,Long.MIN_VALUE,Long.MAX_VALUE);//取为long防止数据过大达到int的最大值
        }
        ```
    
        还有另一种递归的方式，中序遍历时，判断当前节点是否大于中序遍历的前一个节点，如果大于，说明满足 BST，继续遍历；否则直接返回 false。
    
        ```java
        long pre = Long.MIN_VALUE;//初始化前驱结点的值为最小值保证根结点处的正确性
        public boolean isValidBST(TreeNode root) {
            if (root == null) {
                return true;
            }
            // 访问左子树
            if (!isValidBST(root.left)) {
                return false;
            }
            // 访问当前节点：如果当前节点小于等于中序遍历的前一个节点，说明不满足BST，返回 false；否则继续遍历。
            if (root.val <= pre) {
                return false;
            }
            pre = root.val;//向右递归时，将此时的结点作为pre结点
            // 访问右子树
            return isValidBST(root.right);
        }
        ```
    
        方法二：
    
        二叉搜索树中序遍历得到严格升序的序列，对给定的二叉树中序遍历，结果记录于res之中，检验res是否为严格的升序，若是则为true，反之false。因为中序遍历得到的序列（记为res）一定严格递增，所以如果有一处位置i不满足单调递增，一定有res[i]>=res[i+1]，换句话说就是不满足递增的位置一定是相邻的两个位置，假设位置不相邻，比如序列 i , j , k，res[i]>=res[k]，但是res[j]>=res[i]，所以res[j]>=res[k]，所以一定可以找到一个不满足递增的相邻位置。
    
        这一点上有时候直觉（直接的感觉）挺有用的，可以先按照直觉写出代码跑一下，如果不正确根据错误用例来改就行
    
        ```java
        public void inorderTraversal(TreeNode root, ArrayList<Integer> res) {
            if (root.left != null) {
                inorderTraversal(root.left, res);
            }
            res.add(root.val);
            if (root.right != null) {
                inorderTraversal(root.right, res);
            }
        }
        
        public boolean isValidBST(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<>();
            inorderTraversal(root, res);
            Integer[] seq = new Integer[res.size()];
            res.toArray(seq);
            //遍历一次如果有相邻的两个数不满足单调递增直接返回false即可
            for (int i = 0; i < seq.length - 1; i++) {
                if (seq[i] >= seq[i + 1]) {
                    return false;
                }
            }
            return true;
        }
        ```
    
    40. 二叉树的层序遍历(102)：广度优先搜索
    
        首先根元素入队
        当队列不为空的时候
    
        - 求当前队列的长度 si 
        - 依次从队列中取 si个元素进行拓展，然后进入下一次迭代
    
        它和普通广度优先搜索的区别在于，普通广度优先搜索每次只取一个元素拓展，而这里**每次取 si个元素**。在上述过程中的第 i 次迭代就得到了二叉树的第 i 层的 si个元素。
    
        ```java
        public List<List<Integer>> levelOrder(TreeNode root) {
            if (root == null) {
                return new ArrayList<>();
            }
            Deque<TreeNode> deque = new LinkedList<>();
            List<List<Integer>> res = new ArrayList<>();
            deque.add(root);
            while (deque.size() > 0) {
                int nodeCount = deque.size();
                List<Integer> temp = new ArrayList<>();
                for (int i = 0; i < nodeCount; i++) {//每次取si个元素进行扩展
                    TreeNode cur = deque.removeFirst();
                    temp.add(cur.val);
                    if (cur.left != null) {//左子节点为空或右子节点为空就不要加入队列
                        deque.add(cur.left);
                    }
                    if (cur.right != null) {
                        deque.add(cur.right);
                    }
                }
                res.add(temp);
            }
            return res;
        }
        ```
    
    41. 二叉树的最大深度(104)：
    
        方法一：深度优先搜索
    
        如果我们知道了左子树和右子树的最大深度 l 和 r，那么该二叉树的最大深度即为`max(l,r)+1`
    
        而左子树和右子树的最大深度又可以以同样的方式进行计算。因此我们可以用「深度优先搜索」的方法来计算二叉树的最大深度。具体而言，在计算当前二叉树的最大深度时，可以先递归计算出其左子树和右子树的最大深度，然后在 O(1)时间内计算出当前二叉树的最大深度。递归在访问到空节点时退出。**这是求二叉树深度的经典算法最好背下来**
    
        ```java
        public int maxDepth(TreeNode root) {
            if (root == null) {
                return 0;
            }
            int left = maxDepth(root.left);
            int right = maxDepth(root.right);
            return Math.max(left, right) + 1;
        }
        ```
    
        方法二：广度优先搜索
    
        我们也可以用「广度优先搜索」的方法来解决这道题目，但我们需要对其进行一些修改，此时我们广度优先搜索的队列里存放的是「当前层的所有节点」。每次拓展下一层的时候，不同于广度优先搜索的每次只从队列里拿出一个节点，我们需要将队列里的所有节点都拿出来进行拓展，这样能保证每次拓展完的时候队列里存放的是当前层的所有节点，即我们是一层一层地进行拓展，最后我们用一个变量 ans 来维护拓展的次数，该二叉树的最大深度即为 ans。
    
        ```java
        public int maxDepth(TreeNode root) {
            if (root == null) {
                return 0;
            }
            Deque<TreeNode> deque = new LinkedList<>();
            deque.add(root);
            int res = 0;
            while (!deque.isEmpty()) {
                int cnt = deque.size();
                res++;
                for (int i = 0; i < cnt; i++) {
                    TreeNode cur = deque.removeFirst();
                    if (cur.left != null) {
                        deque.add(cur.left);
                    }
                    if (cur.right != null) {
                        deque.add(cur.right);
                    }
                }
            }
            return res;
        }
        ```
    
    42. 从前序与中序遍历序列构造二叉树(105)：
    
        方法一：递归
    
        对于任意一颗树而言，前序遍历的形式总是 [ 根节点, [左子树的前序遍历结果], [右子树的前序遍历结果] ]
        即根节点总是前序遍历中的第一个节点。
    
        而中序遍历的形式总是[ [左子树的中序遍历结果], 根节点, [右子树的中序遍历结果] ]
        只要我们在中序遍历中定位到根节点，那么我们就可以分别知道左子树和右子树中的节点数目。由于同一颗子树的前序遍历和中序遍历的长度显然是相同的，因此我们就可以对应到前序遍历的结果中，对上述形式中的所有左右括号进行定位。
    
        这样以来，我们就知道了左子树的前序遍历和中序遍历结果，以及右子树的前序遍历和中序遍历结果，我们就可以递归地对构造出左子树和右子树，再将这两颗子树接到根节点的左右位置。
    
        **细节**
    
        在中序遍历中对根节点进行定位时，一种简单的方法是直接扫描整个中序遍历的结果并找出根节点，但这样做的时间复杂度较高。我们可以考虑使用哈希表来帮助我们快速地定位根节点。对于哈希映射中的每个键值对，键表示一个元素（节点的值），值表示其在中序遍历中的出现位置。在构造二叉树的过程之前，我们可以对中序遍历的列表进行一遍扫描，就可以构造出这个哈希映射。在此后构造二叉树的过程中，我们就只需要 O(1)的时间对根节点进行定位了。
    
        ```java
        private HashMap<Integer, Integer> indexMap;
        
        public TreeNode myBuildTree(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
            if (preorder_left > preorder_right) {//说明此时得到的是叶子结点，直接返回null就行
                return null;
            }
            // 前序遍历中的第一个节点就是根节点
            // 在中序遍历中定位根节点
            int inorder_root = indexMap.get(preorder[preorder_left]);
            // 先把根节点建立出来
            TreeNode root = new TreeNode(preorder[preorder_left]);
            // 得到左子树中的节点数目
            int size_left_subtree = inorder_root - inorder_left;
            // 递归地构造左子树，并连接到根节点
            // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
            root.left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
            // 递归地构造右子树，并连接到根节点
            // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
            root.right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
            return root;
        }
        
        public TreeNode buildTree(int[] preorder, int[] inorder) {
            int n = preorder.length;
            // 构造哈希映射，帮助我们快速定位根节点
            indexMap = new HashMap<Integer, Integer>();
            for (int i = 0; i < n; i++) {
                indexMap.put(inorder[i], i);
            }
            return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
        }
        ```
    
        方法二：迭代
    
        了解一下，这个做法不一定能想得出来[从前序与中序遍历序列构造二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/cong-qian-xu-yu-zhong-xu-bian-li-xu-lie-gou-zao-9/)
    
    43. 二叉树展开为链表(114)：
    
        方法一：前序遍历
    
        将二叉树展开为单链表之后，单链表中的节点顺序即为二叉树的前序遍历访问各节点的顺序。因此，可以对二叉树进行前序遍历，获得各节点被访问到的顺序。由于将二叉树展开为链表之后会破坏二叉树的结构，因此在前序遍历结束之后更新每个节点的左右子节点的信息，将二叉树展开为单链表。可以通过递归或迭代的方式实现前序遍历
    
        ```java
        public void preorder(TreeNode node, ArrayList<Integer> res) {
            res.add(node.val);
            if (node.left != null) {
                preorder(node.left, res);
            }
            if (node.right != null) {
                preorder(node.right, res);
            }
        }
        
        public void flatten(TreeNode root) {
            if (root == null) {
                return;
            }
            ArrayList<Integer> res = new ArrayList<>();
            preorder(root, res);
            TreeNode cur = root;
            for (int i = 1; i < res.size(); i++) {
                cur.left = null;
                cur.right = new TreeNode(res.get(i));
                cur = cur.right;
            }
        }
        ```
    
        方法二：前序遍历和展开同时进行
    
        使用方法一的前序遍历，由于将节点展开之后会破坏二叉树的结构而丢失子节点的信息，因此前序遍历和展开为单链表分成了两步。能不能在不丢失子节点的信息的情况下，将前序遍历和展开为单链表同时进行？
    
        之所以会在破坏二叉树的结构之后丢失子节点的信息，是因为在对左子树进行遍历时，没有存储右子节点的信息，在遍历完左子树之后才获得右子节点的信息。只要对前序遍历进行修改，**在遍历左子树之前就获得左右子节点的信息**，并存入栈内，子节点的信息就不会丢失，就可以将前序遍历和展开为单链表同时进行。
    
        该做法不适用于递归实现的前序遍历，只适用于迭代实现的前序遍历。修改后的前序遍历的具体做法是，每次从栈内弹出一个节点作为当前访问的节点，获得该节点的子节点，如果子节点不为空，则**依次将右子节点和左子节点压入栈内（注意入栈顺序）**。
    
        展开为单链表的做法是，维护上一个访问的节点 prev，每次访问一个节点时，令当前访问的节点为 curr，将 prev 的左子节点设为 null 以及将 prev 的右子节点设为 curr，然后将 curr 赋值给 prev，进入下一个节点的访问，直到遍历结束。需要注意的是，初始时 prev 为 null，只有在 prev 不为 null 时才能对 prev 的左右子节点进行更新。
    
        ```java
        public void flatten(TreeNode root) {
            if (root == null) {
                return;
            }
            Deque<TreeNode> stack = new LinkedList<>();
            stack.push(root);
            TreeNode pre = null;
            while (!stack.isEmpty()) {
                TreeNode cur = stack.pop();
                if (pre != null) {
                    pre.left = null;
                    pre.right = cur;
                }
                TreeNode left = cur.left, right = cur.right;
                //先压栈的是右节点，然后再压入左节点，出栈是就是先左再右的顺序
                if (right != null) {
                    stack.push(right);
                }
                if (left != null) {
                    stack.push(left);
                }
                //第一次pre等于null的时候将pre写为cur，不能写成pre=pre.right，第一次pre为null
                pre = cur;
            }
        }
        ```
    
        方法三：寻找前驱结点
    
        注意到前序遍历访问各节点的顺序是根节点、左子树、右子树。如果一个节点的左子节点为空，则该节点不需要进行展开操作。如果一个节点的左子节点不为空，则该节点的左子树中的最后一个节点被访问之后，该节点的右子节点被访问。该节点的左子树中最后一个被访问的节点是左子树中的最右边的节点，也是该节点的前驱节点。因此，问题转化成寻找当前节点的前驱节点。找前驱结点在**线索化二叉树**的时候做过，找到叶子结点 的前驱结点和后继结点。
    
        具体做法是，对于当前节点，如果其左子节点不为空，则在其左子树中找到最右边的节点，作为前驱节点，将当前节点的右子节点赋给前驱节点的右子节点（就是把后继结点挂上去），然后将当前节点的左子节点赋给当前节点的右子节点，并将当前节点的左子节点设为空，（向右展开）。对当前节点处理结束后，继续处理链表中的下一个节点（下一个右子节点），直到所有节点都处理结束。这个地方需要画图好好理解一下。
    
        ```java
        public void flatten(TreeNode root) {
            TreeNode cur=root;
            while (cur!=null){
                if (cur.left!=null){
                    //记录下当前结点的左子结点
                    TreeNode next=cur.left;
                    TreeNode pre=next;
                   //对于当前节点cur，如果其左子节点不为空，则在其左子树中找到最右边的节点，作为前驱节点
                    while (pre.right!=null){
                        pre=pre.right;
                    }
                    //将当前节点的右子节点赋给前驱节点的右子节点（就是把后继结点挂上去）
                    pre.right=cur.right;
                    //将当前节点的左子节点赋给当前节点的右子节点，并将当前节点的左子节点设为空
                    cur.left=null;
                    cur.right=next;
                }
                cur=cur.right;
            }
        }
        ```
    
    44. 买卖股票的最佳时机(121)：
    
        一道简单题，翻译一下就是找到数组中后一个数与前一个数的最大差值（差值必须是真的，如果只能是负的就返回0），可以通过一次遍历来实现这个结果
    
        ```java
        public int maxProfit(int[] prices) {
            int res=0,buy=prices[0];//买入的价格初始化为第一天的价格，结果初始化为0
            for (int i = 1; i < prices.length; i++) {
                //如果某一天的价格大于买入的价格就尝试卖出，并且和之前卖出的收入进行比较，取收入高的那个
                if (prices[i]>buy){
                    res=Math.max(res,prices[i]-buy);
                //如果某天的价格比买入的价格低，那么就应该从这一天买入
                }else if (prices[i]<buy){
                    buy=prices[i];
                }
            }
            return res;
        }
        ```
    
    45. 二叉树中的最大路径和(124)：
    
        这题目的难点在于理解题意和转化题意。
    
        **思路**
    
        1. 「可以从任意节点出发, 到达任意节点」 的路径, 
           一定是先上升（ 0 ～ n 个）节点, 到达顶点, 后下降（ 0 ～ n 个）节点。当我们站在顶点的位置来看，就是向左子树走一条路径，向右子树走一条路径，在左子树中不能既向左走又向右走，这样就无法从左子树走回到右子树了（下面还会仔细说明这个地方）。我们可以通过枚举顶点的方式来枚举路径。
    
        2. 我们枚举顶点时, 可以把路径分拆成3部分： 左侧路径、右侧路径和顶点。
           如下面的路径, 顶点为 20, 左侧路径为 6 -> 15, 右侧为 6 -> 7。
    
            ``` 
              -10
              /  \
             9   20
                /  \
               15   7
              /    / \
             6    4   6
            ```
    
           以当前节点为顶点的路径中, 最大和为 两侧路径的最大和 + 节点的值。需要注意的是, 两侧路径也可能不选, 此时取 0
    
        **实现细节**
    
        首先，考虑实现一个简化的函数 maxGain(node)，该函数计算二叉树中的一个节点的最大贡献值，具体而言，就是在以该节点为根节点的子树中寻找以该节点为起点的一条路径，使得该路径上的节点值之和最大。
    
        具体而言，该函数的计算如下。
    
        - 空节点的最大贡献值等于 0。
    
        - 非空节点的最大贡献值等于节点值与其子节点中的最大贡献值之和（对于叶节点而言，最大贡献值等于节点值）。
    
        例如，考虑如下二叉树。
    
         ```  
            -10
           /   \
          9    20
              /  \
            15    7
         ```
    
        叶节点 9、15、7 的最大贡献值分别为 9、15、7。
    
        得到叶节点的最大贡献值之后，再计算非叶节点的最大贡献值。节点 20 的最大贡献值等于 20+max(15,7)=35，节点 -10 的最大贡献值等于 −10+max(9,35)=25。
    
        上述计算过程是递归的过程，因此，对根节点调用函数 maxGain，即可得到每个节点的最大贡献值。
    
        再次说明一下这里的最大贡献值，某一个结点的最大贡献值只能取左子树的最大贡献值和右子树的最大贡献值中较大的一个并且和结点的值相加，然后会将该结果返回到递归的上一层，这样对于上一层来看，拿到的就是左子树的最大贡献值，并且可以从左子树走到这一层的顶点，然后再往右子树走
    
        根据函数 maxGain 得到每个节点的最大贡献值之后，如何得到二叉树的最大路径和？对于二叉树中的一个节点，**该节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值**，如果子节点的最大贡献值为正，则计入该节点的最大路径和，否则不计入该节点的最大路径和。维护一个全局变量 maxSum 存储最大路径和，在递归过程中更新 maxSum 的值，最后得到的 maxSum 的值即为二叉树中的最大路径和。
    
        ```java
        private int res=Integer.MIN_VALUE;
        public int maxPathSum(TreeNode root) {
            if (root==null){
                return 0;//保证传入空结点时的正确性
            }
            maxGain(root);//求出每个结点的最大贡献值，这是个递归的过程
            return res;
        }
        public int maxGain(TreeNode node){
            if (node==null){
                return 0;
            }
            int left=Math.max(maxGain(node.left),0);//左结点的最大贡献值如果小于0就不要贡献了，越加越小肯定是不对的
            int right=Math.max(maxGain(node.right),0);
            res=Math.max(res,node.val+left+right);
            return node.val+Math.max(left,right);//这里体现了这个递归的精髓
            //将该结点与左子树贡献和右子树贡献最大的一个向上返回（只返回左右子树的一边），对于父亲结点（大问题）来看，就是在父结点中找到了加起来的值最大的一条通路
        }
        ```
    
        **复盘总结递归**
        递归一个树，会对每个子树做同样的事（你写的处理逻辑），所以你需要思考要对每个子树做什么事，即思考子问题是什么，大问题怎么拆解成子问题。
        通过求出每个子树对外提供的最大路径和（返回出来给父节点），从递归树底部向上，不断求出了每个子树内部的最大路径和，后者是求解的目标，它的求解需要前者，搞清楚二者的关系。
        每个子树的内部最大路径和，都挑战一下最大纪录，递归结束时，最大纪录就有了。
        思考递归问题，别纠结细节实现，内部细节是子递归帮你去做的，应结合求解的目标，自顶而下、屏蔽细节地思考，思考递归子问题的定义。随着递归出栈，子问题自下而上地解决，最后解决了整个问题。
        要做的只是**写好递归的处理逻辑，怎么处理当前子树？需要返回什么吗？怎么设置递归的出口？**
        没有思路的时候，试着画画递归树找思路。就算做对了，画图也能加深对递归算法的理解。
    
        画图的时候可以反过来自底向上的画，先画出最简单的情况，然后看得出简单情况的结果后，如果该情况是一个大问题的子问题，那么该子问题可以怎么运用到大问题的解决当中去
    
    46. 最长连续序列(128)：
    
        这是一道很好的使用**哈希表**的题目（~~虽然我想到了用哈希表但还是没做出来~~），我们考虑枚举数组中的每个数 x，考虑以其为起点，不断尝试匹配 x+1,x+2,⋯ 是否存在，假设最长匹配到了 x+y，那么以 x 为起点的最长连续序列 x,x+1,x+2,⋯,x+y，其长度为 y+1，我们不断枚举并更新答案即可。
    
        对于匹配的过程，暴力的方法是 O(n) 遍历数组去看是否存在这个数，但其实更高效的方法是用一个哈希表存储数组中的数，这样查看一个数是否存在即能优化至 O(1) 的时间复杂度。
    
        仅仅是这样我们的算法时间复杂度最坏情况下还是会达到 O(n^2)（即外层需要枚举 O(n) 个数，内层需要暴力匹配 O(n) 次），无法满足题目的要求。但仔细分析这个过程，我们会发现其中执行了很多不必要的枚举，如果已知有一个 x,x+1,x+2,⋯,x+y 的连续序列，而我们却重新从 x+1，x+2 或者是 x+y 处开始尝试匹配，那么得到的结果肯定不会优于枚举 x为起点的答案，因此我们在外层循环的时候碰到这种情况跳过即可。
    
        那么怎么判断是否跳过呢？由于我们**要枚举的数 x 一定是在数组中不存在前驱数 x−1 的**，不然按照上面的分析我们会从 x-1 开始尝试匹配，因此我们每次在哈希表中检查是否存在 x-1 即能判断是否需要跳过了。
    
        ```java
        public int longestConsecutive(int[] nums) {
            HashSet<Integer> numsSet = new HashSet<>();//创建HashSet去重并且实现了HashMap
            for (int num :
                 nums) {
                numsSet.add(num);
            }
            int longest=0;
            for (int num :
                 numsSet) {
                if (!numsSet.contains(num-1)){//不存在前继才开始向后枚举
                    int curLong=1;
                    int temp=num+1;
                    while (numsSet.contains(temp)){
                        curLong+=1;
                        temp+=1;
                    }
                    longest=Math.max(longest,curLong);
                }
            }
            return longest;
        }
        ```
    
    47. 只出现一次的数字(136)：
    
        方法一：利用HashSet，遍历数组，当该数字出现在HashSet中就将HashSet中的该数字删掉，如果不存在就将该数字加入到HashSet中。
    
        ```java
        public int singleNumber(int[] nums) {
            HashSet<Integer> set = new HashSet<>();
            for (int num : nums) {
                if (set.contains(num)) {
                    set.remove(num);
                } else {
                    set.add(num);
                }
            }
            int res=0;
            for (int num :
                 set) {
                res=num;
                break;
            }
            return res;
        }
        ```
    
        方法二：异或运算
    
        经典的异或运算的题目，根据异或运算的交换律和结合律，以及下面两条性质`A^A=0, A^0=A`，对数组中的数字一次进行异或运算，最后得到的值就是结果
    
        ```java
        public int singleNumber(int[] nums){
            int res=nums[0];
            for (int i = 1; i < nums.length; i++) {
                res=res^nums[i];
            }
            return res;
        }
        ```
    
    48. 单词拆分(139)：
    
        方法一：**动态规划**
    
        我们定义 dp[i] 表示字符串 s 前 i 个字符组成的字符串 s[0..i−1] 是否能被空格拆分成若干个字典中出现的单词。从前往后计算考虑转移方程，每次转移的时候我们需要枚举包含位置 i−1 的最后一个单词，看它是否出现在字典中以及除去这部分的字符串是否合法即可。公式化来说，我们需要枚举 s[0..i−1] 中的分割点 j ，看 s[0..j−1] 组成的字符串s~1~（默认 j = 0 时 s~1~ 为空串）和  s[j..i−1] 组成的字符串 s~2~ 是否都合法，如果两个字符串均合法，那么按照定义 s~1~ 和 s~2~ 拼接成的字符串也同样合法。由于计算到 dp[i] 时我们已经计算出了 dp[0..i−1] 的值，因此字符串 s~1~ 是否合法可以直接由 dp[j] 得知，剩下的我们只需要看 s~2~ 是否合法即可，因此我们可以得出如下转移方程：
        `dp[i]=dp[j] && check(s[j..i−1])`，其中 check(s[j..i−1]) 表示子串 s[j..i−1] 是否出现在字典中。
    
        对于检查一个字符串是否出现在给定的字符串列表里一般可以考虑哈希表来快速判断，同时也可以做一些简单的剪枝，枚举分割点的时候倒着枚举，如果分割点 j 到 i 的长度已经大于字典列表里最长的单词的长度，那么就结束枚举，但是需要注意的是下面的代码给出的是不带剪枝的写法。
    
        对于边界条件，我们定义 dp[0]=true 表示空串且合法。
    
        ```java
        public boolean wordBreak(String s, List<String> wordDict) {
            HashSet<String> stringHashSet = new HashSet<>(wordDict);
            boolean[] dp = new boolean[s.length() + 1];
            dp[0] = true;
            for (int i = 1; i <= s.length(); i++) {
                for (int j = 0; j < i; j++) {
                    if (dp[j] && stringHashSet.contains(s.substring(j, i))) {
                        dp[i] = true;
                        break;
                    }
                }
            }
            return dp[s.length()];
        }
        ```
    
    49. 环形链表(141)：
    
        方法一：哈希表
    
        最容易想到的方法是遍历所有节点，每次遍历到一个节点时，判断该节点此前是否被访问过。
    
        具体地，我们可以使用哈希表来存储所有已经访问过的节点。每次我们到达一个节点，如果该节点已经存在于哈希表中，则说明该链表是环形链表，否则就将该节点加入哈希表中。重复这一过程，直到我们遍历完整个链表即可。
    
        ```java
        public boolean hasCycle(ListNode head) {
            HashSet<ListNode> set = new HashSet<>();
            while (head != null) {
                if (set.contains(head)) {
                    return true;
                }
                set.add(head);
                head = head.next;
            }
            return false;
        }
        ```
    
        方法二：快慢指针
    
        本方法需要对「Floyd 判圈算法」（又称龟兔赛跑算法）有所了解。
    
        假想「乌龟」和「兔子」在链表上移动，「兔子」跑得快，「乌龟」跑得慢。当「乌龟」和「兔子」从链表上的同一个节点开始移动时，如果该链表中没有环，那么「兔子」将一直处于「乌龟」的前方；如果该链表中有环，那么「兔子」会先于「乌龟」进入环，并且一直在环内移动。等到「乌龟」进入环时，由于「兔子」的速度快，它一定会在某个时刻与乌龟相遇，即套了「乌龟」若干圈。
    
        我们可以根据上述思路来解决本题。具体地，我们定义两个指针，一快一满。慢指针每次只移动一步，而快指针每次移动两步。初始时，慢指针和慢指针都在位置 head。这样一来，如果在移动的过程中，快指针反过来追上慢指针，就说明该链表为环形链表。否则快指针将到达链表尾部，该链表不为环形链表。
    
        ```java
        public boolean hasCycle(ListNode head) {
            ListNode slow = head;
            ListNode fast = head;
            do {//注意这里只能用do-while循环
                //先进行判断，防止移动快指针的时候出现空指针异常
                if (fast == null || fast.next == null) {
                    return false;//fast走到结尾则一定没有环
                }
                slow = slow.next;
                fast = fast.next.next;
            } while (slow != fast);
            return true;//while循环退出说明追上了
        }
        ```
    
    50. 环形链表ⅱ
    
        方法一：哈希表
    
        一个非常直观的思路是：我们遍历链表中的每个节点，并将它记录下来；一旦遇到了此前遍历过的节点，就可以判定链表中存在环。借助哈希表可以很方便地实现。代码很简单和上一题差不多，就不放上来了
    
        方法二：快慢指针
    
        可以当作一个追及问题来列方程，我们使用两个指针，fast 与 slow。它们起始都位于链表的头部。随后，slow 指针每次向后移动一个位置，而 fast 指针向后移动两个位置。如果链表中存在环，则 fast 指针最终将再次与 slow 指针在环中相遇。
    
        > 解释一下为何慢指针第一圈没走完一定会和快指针相遇： 首先，第一步，快指针先进入环 第二步：当慢指针刚到达环的入口时，快指针此时在环中的某个位置(也可能此时相遇) 第三步：设此时快指针和慢指针距离为x，若在第二步相遇，则x = 0； 第四步：设环的周长为n，那么看成快指针追赶慢指针，需要追赶n-x； 第五步：快指针每次都追赶慢指针1个单位，设慢指针速度1/s，快指针2/s，那么追赶需要(n-x)s 第六步：在n-x秒内，慢指针走了n-x单位，因为x>=0（x=0会在第二步相遇），则慢指针走的路程小于等于n，即走不完一圈就和快指针相遇
    
        如下图所示，设链表中环外部分的长度为 a。slow 指针进入环后，又走了 b 的距离与fast 相遇。此时，fast 指针已经走完了环的 n 圈，因此它走过的总距离为 a+n(b+c)+b = a+(n+1)b+nc。
    
        ![image-20220514215556947](image-20220514215556947-1662463137547117.png)
    
        根据题意，任意时刻，fast 指针走过的距离都为 slow 指针的 2 倍。因此，我们有
        a+(n+1)b+nc=2(a+b)⟹a=c+(n−1)(b+c)
    
        有了 a=c+(n-1)(b+c) 的等量关系，我们会发现：从相遇点到入环点的距离加上 n−1 圈的环长，恰好等于从链表头部到入环点的距离。
    
        因此，当发现 slow 与 fast 相遇时，我们再额外使用一个指针 ptr。起始，它指向链表头部；随后，它和 slow 每次向后移动一个位置。最终，它们会在入环点相遇。
    
        ```java
        public ListNode detectCycle(ListNode head){
            if (head==null){
                return null;
            }
            ListNode slow=head,fast=head;
            while (fast!=null){
                slow=slow.next;
                if (fast.next!=null){//特判fast的next是否为空，防止空指针异常
                    fast=fast.next.next;
                }else{
                    return null;//说明没有环
                }
                if (fast==slow){
                    ListNode ptr=head;
                    while (ptr!=slow){
                        ptr=ptr.next;
                        slow=slow.next;
                    }
                    return ptr;
                }
            }
            return null;//此时fast=null
        }
        ```
    
    51. LRU缓存(146)：
    
        **哈希表+双向链表**
    
        很好的一道面向对象的题目，同时也考察了Java集合和一些常用的数据结构
    
        LRU 缓存机制可以通过哈希表辅以双向链表实现，我们用一个哈希表和一个双向链表维护所有在缓存中的键值对。
    
        双向链表**按照被使用的顺序**存储了这些键值对，**靠近头部的键值对是最近使用的，而靠近尾部的键值对是最久未使用的。**
    
        哈希表即为普通的哈希映射（HashMap），**通过缓存数据的键映射到其在双向链表中的位置**。
    
        这样以来，我们首先使用哈希表进行定位，找出缓存项在双向链表中的位置，随后将其移动到双向链表的头部，即可在 O(1) 的时间内完成 get 或者 put 操作。具体的方法如下：
    
        对于 get 操作，首先判断 key 是否存在：
    
        - 如果 key 不存在，则返回 -1；
    
        - 如果 key 存在，则 key 对应的节点是最近被使用的节点。通过哈希表定位到该节点在双向链表中的位置，并将其移动到双向链表的头部，最后返回该节点的值。
    
        对于 put 操作，首先判断 key 是否存在：
    
        - 如果 key 不存在，使用 key 和 value 创建一个新的节点，在双向链表的头部添加该节点，并将 key 和该节点添加进哈希表中。然后判断双向链表的节点数是否超出容量，如果超出容量，则删除双向链表的尾部节点，并删除哈希表中对应的项；
    
        - 如果 key 存在，则与 get 操作类似，先通过哈希表定位，再将对应的节点的值更新为 value，并将该节点移到双向链表的头部。
    
        上述各项操作中，访问哈希表的时间复杂度为 O(1)，在双向链表的头部添加节点、在双向链表的尾部删除节点的复杂度也为 O(1)。而将一个节点移到双向链表的头部，可以分成「删除该节点」和「在双向链表的头部添加节点」两步操作，都可以在 O(1) 时间内完成。
    
        小贴士
    
        在双向链表的实现中，使用一个伪头部（dummy head）和伪尾部（dummy tail）标记界限，这样在添加节点和删除节点的时候就不需要检查相邻的节点是否存在。==**在链表有关的题目中都可以使用哑结点这个技巧**==
    
        ```java
        public class LRUCache {
            class LinkedNode {
                int key;
                int value;
                LinkedNode prev;
                LinkedNode next;
        
                public LinkedNode() {
        
                }
        
                public LinkedNode(int key, int value) {
                    this.key = key;
                    this.value = value;
                }
            }
        
            private HashMap<Integer, LinkedNode> cache = new HashMap<>();
            private int capacity;
            private int size;
            private LinkedNode head, tail;//head和tail是哑结点，删除和插入的时候要注意
        
            public LRUCache(int capacity) {
                this.capacity = capacity;
                this.size = 0;
                head = new LinkedNode();
                tail = new LinkedNode();
                head.next = tail;
                tail.prev = head;
            }
        
            public int get(int key) {
                LinkedNode node = cache.get(key);
                if (node == null) {
                    return -1;
                }
                moveToHead(node);
                return node.value;
            }
        
            public void put(int key, int value) {
                LinkedNode node = cache.get(key);
                if (node == null) {
                    LinkedNode newNode = new LinkedNode(key, value);
                    cache.put(key, newNode);
                    addToHead(newNode);
                    size++;
                    if (size > capacity) {
                        LinkedNode tail = removeTail();
                        cache.remove(tail.key);
                        size--;
                    }
                } else {
                    node.value = value;
                    moveToHead(node);
                }
            }
        
            public void addToHead(LinkedNode node) {
                node.prev = head;
                node.next = head.next;
                head.next.prev = node;
                head.next = node;
            }
        
            public void removeNode(LinkedNode node) {
                node.prev.next = node.next;
                node.next.prev = node.prev;
            }
        
            public void moveToHead(LinkedNode node) {
                removeNode(node);
                addToHead(node);
            }
        
            public LinkedNode removeTail() {
                LinkedNode res = tail.prev;
                removeNode(res);
                return res;
            }
        }
        /**
         * Your LRUCache object will be instantiated and called as such:
         * LRUCache obj = new LRUCache(capacity);
         * int param_1 = obj.get(key);
         * obj.put(key,value);
         */
        ```
    
    52. 排序链表(148)：
    
        方法一：**自顶向下归并排序**
    
        对链表自顶向下归并排序的过程如下。
    
        找到链表的中点，以中点为分界，将链表拆分成两个子链表。寻找链表的中点可以使用快慢指针的做法，快指针每次移动 2 步，慢指针每次移动 1 步，当快指针到达链表末尾时，慢指针指向的链表节点即为链表的中点。
    
        对两个子链表分别排序。
    
        将两个排序后的子链表合并，得到完整的排序后的链表。可以使用合并两个有序链表的做法，将两个有序的子链表进行合并。
    
        上述过程可以通过递归实现。递归的终止条件是链表的节点个数小于或等于 1，即当链表为空或者链表只包含 1 个节点时，不需要对链表进行拆分和排序。下面有一个草图帮助理解该算法
    
        ![image-20220521160851670](image-20220521160851670-1662463137547118.png)
    
        ```java
        public ListNode sortList(ListNode head) {
            return sortList(head,null);//排序是不包含第二个参数tail一起的，前一个参数包含后一个不包含
        }
        public ListNode sortList(ListNode head,ListNode tail){
            if (head==null){
                return head;//如果传入的是null也在这里统一处理了
            }
            if (head.next==tail){
                head.next=null;//把head的next标记为null，做到真正将子链表分开，方便后面找到中点和merge
                return head;
            }
            ListNode slow=head,fast=head;
            while (fast!=tail){
                slow=slow.next;
                fast=fast.next;
                if (fast!=tail){
                    fast=fast.next;
                }
            }
            ListNode mid=slow;
            ListNode list1=sortList(head,mid);//排序是不包含第二个参数tail一起的，前一个参数包含后一个不包含
            ListNode list2=sortList(mid,tail);
            return merge(list1,list2);
        }
        
        public ListNode merge(ListNode head1, ListNode head2) {
            ListNode dummy=new ListNode(0);
            ListNode temp=dummy,temp1=head1,temp2=head2;
            while (temp1!=null&&temp2!=null){
                if (temp1.val<=temp2.val){
                    temp.next=temp1;
                    temp1=temp1.next;
                }else{
                    temp.next=temp2;
                    temp2=temp2.next;
                }
                temp=temp.next;
            }
            if (temp1!=null){
                temp.next=temp1;
            }else if (temp2!=null){
                temp.next=temp2;
            }
            return dummy.next;
        }
        ```
    
        方法二：自底向上归并排序
    
        使用自底向上的方法实现归并排序，则可以达到 O(1) 的空间复杂度。
    
        首先求得链表的长度length，然后将链表拆分成子链表进行合并。具体做法如下。
    
        用 subLength 表示每次需要排序的子链表的长度，初始时 subLength=1。
    
        每次将链表拆分成若干个长度为subLength 的子链表（最后一个子链表的长度可以小于 subLength），按照每两个子链表一组进行合并，合并后即可得到若干个长度为subLength×2 的有序子链表（最后一个子链表的长度可以小于 subLength×2）。合并两个子链表仍然使用 合并两个有序链表的做法。
    
        将 subLength 的值加倍，重复第 2 步，对更长的有序子链表进行合并操作，直到有序子链表的长度大于或等于 length，整个链表排序完毕。
    
        ```java
        // 自底向上归并排序
        public ListNode sortList(ListNode head) {
            if(head == null){
                return head;
            }
        
            // 1. 首先从头向后遍历,统计链表长度
            int length = 0; // 用于统计链表长度
            ListNode node = head;
            while(node != null){
                length++;
                node = node.next;
            }
        
            // 2. 初始化 引入dummynode
            ListNode dummyHead = new ListNode(0);
            dummyHead.next = head;
        
            // 3. 每次将链表拆分成若干个长度为subLen的子链表 , 并按照每两个子链表一组进行合并
            for(int subLen = 1;subLen < length;subLen <<= 1){ // subLen每次左移一位（即sublen = sublen*2） PS:位运算对CPU来说效率更高
                ListNode prev = dummyHead;
                ListNode curr = dummyHead.next;     // curr用于记录拆分链表的位置
        
                while(curr != null){               // 如果链表没有被拆完
                    // 3.1 拆分subLen长度的链表1
                    ListNode head_1 = curr;        // 第一个链表的头 即 curr初始的位置
                    for(int i = 1; i < subLen && curr != null && curr.next != null; i++){     // 拆分出长度为subLen的链表1
                        curr = curr.next;
                    }
        
                    // 3.2 拆分subLen长度的链表2
                    ListNode head_2 = curr.next;  // 第二个链表的头  即 链表1尾部的下一个位置
                    curr.next = null;             // 断开第一个链表和第二个链表的链接
                    curr = head_2;                // 第二个链表头 重新赋值给curr
                    for(int i = 1;i < subLen && curr != null && curr.next != null;i++){      // 再拆分出长度为subLen的链表2
                        curr = curr.next;
                    }
        
                    // 3.3 再次断开 第二个链表最后的next的链接
                    ListNode next = null;        
                    if(curr != null){
                        next = curr.next;   // next用于记录 拆分完两个链表的结束位置
                        curr.next = null;   // 断开连接
                    }
        
                    // 3.4 合并两个subLen长度的有序链表
                    ListNode merged = mergeTwoLists(head_1,head_2);
                    prev.next = merged;        // prev.next 指向排好序链表的头
                    while(prev.next != null){  // while循环 将prev移动到 subLen*2 的位置后去
                        prev = prev.next;
                    }
                    curr = next;              // next用于记录 拆分完两个链表的结束位置
                }
            }
            // 返回新排好序的链表
            return dummyHead.next;
        }


​        
​        // 此处是Leetcode21 --> 合并两个有序链表
​        public ListNode mergeTwoLists(ListNode l1,ListNode l2){
​            ListNode dummy = new ListNode(0);
​            ListNode curr  = dummy;
​        
            while(l1 != null && l2!= null){ // 退出循环的条件是走完了其中一个链表
                // 判断l1 和 l2大小
                if (l1.val < l2.val){
                    // l1 小 ， curr指向l1
                    curr.next = l1;
                    l1 = l1.next;       // l1 向后走一位
                }else{
                    // l2 小 ， curr指向l2
                    curr.next = l2;
                    l2 = l2.next;       // l2向后走一位
                }
                curr = curr.next;       // curr后移一位
            }
        
            // 退出while循环之后,比较哪个链表剩下长度更长,直接拼接在排序链表末尾
            if(l1 == null) curr.next = l2;
            if(l2 == null) curr.next = l1;
        
            // 最后返回合并后有序的链表
            return dummy.next; 
        }
        ```
    
    53. 乘积最大子数组(152)：
    
        方法一：暴力枚举
    
        直接枚举每个位置开始的所有的乘积，注意只有一个数字的乘积也要参与比较
    
        ```java
        public int maxProduct(int[] nums) {
            //0 2
            int max = Integer.MIN_VALUE;
            for (int i = 0; i < nums.length; i++) {
                int temp = nums[i];
                if (temp > max) {//只有一个数也要与最大值比较一下
                    max = temp;
                }
                for (int j = i + 1; j < nums.length; j++) {
                    temp = temp * nums[j];
                    if (temp > max) {
                        max = temp;
                    }
                }
            }
            return max;
        }
        ```
    
        方法二：动态规划
    
        我们可以根据当前位置的正负性进行分类讨论。
    
        如果当前位置是一个负数的话，那么我们希望以它前一个位置结尾的某个段的积也是个负数，这样就可以负负得正，并且我们希望这个积尽可能「负得更多」，即尽可能小。
    
        如果当前位置是一个正数的话，我们更希望以它前一个位置结尾的某个段的积也是个正数，并且希望它尽可能地大。
    
        于是这里我们可以再维护一个它表示以第 i 个元素结尾的最小子数组的乘积，那么我们可以得到这样的动态规划转移方程：
    
        `max[i]=max(max[i-1] * nums[i],nums[i],min[i-1] * nums[i])`
        `min[i]=min(min[i-1] * nums[i],nums[i],max[i-1] * nums[i])`
    
        ```java
        public int maxProduct(int[] nums) {
            //    2 -3   2   -3
            //max 2 -3   2   36
            //min 2 -6  -12  -6
            //max[i]=max(max[i-1]*nums[i],nums[i],min[i-1]*nums[i])
            //min[i]=min(min[i-1]*nums[i],nums[i],max[i-1]*nums[i])
            //max[i]表示以元素nums[i]结尾的序列的乘积的最大值
            int[] max = new int[nums.length];
            int[] min = new int[nums.length];
            max[0] = nums[0];
            min[0] = nums[0];
            for (int i = 1; i < nums.length; i++) {
                max[i] = Math.max(Math.max(max[i - 1] * nums[i], nums[i]), min[i - 1] * nums[i]);
                min[i] = Math.min(Math.min(min[i - 1] * nums[i], nums[i]), max[i - 1] * nums[i]);
            }
            //结果需要遍历以数组中每一个元素作为结尾的所有情况找出最大的
            int res = max[0];
            for (int i = 1; i < max.length; i++) {
                if (max[i] > res) {
                    res = max[i];
                }
            }
            return res;
        }
        ```
    
    54. 最小栈（155）：
    
        用**辅助栈**来实现（第一次做这题真没想到这种做法）
    
        要做出这道题目，首先要理解栈结构**后进先出**的性质。
    
        对于栈来说，如果一个元素 a 在入栈时，栈里有其它的元素 b, c, d，那么无论这个栈在之后经历了什么操作，只要 a 在栈中，b, c, d 就一定在栈中，因为在 a 被弹出之前，b, c, d 不会被弹出。
    
        因此，在操作过程中的任意一个时刻，只要栈顶的元素是 a，那么我们就可以确定栈里面现在的元素（从栈顶到栈底）一定是 a, b, c, d。
    
        那么，我们可以在每个元素 a 入栈时把当前栈的最小值 m 存储起来。在这之后无论何时，如果栈顶元素是 a，我们就可以直接返回存储的最小值 m。
    
        我们可以使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值。
    
        - 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；
    
        - 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；
    
        - 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中。
    
        ```java
        public class MinStack {
            private Deque<Integer> stack;
            private Deque<Integer> minStack;
        
            public MinStack() {
                stack=new LinkedList<>();
                minStack=new LinkedList<>();
                minStack.push(Integer.MAX_VALUE);//先放入最大值做为标记
            }
        
            public void push(int val) {
                int min=minStack.peek();
                minStack.push(Math.min(val, min));//minStack中放入当前元素与栈顶元素较小的那个
                stack.push(val);
            }
        
            public void pop() {
                stack.pop();//同时弹出
                minStack.pop();
            }
        
            public int top() {
                return stack.peek();
            }
        
            public int getMin() {
                return minStack.peek();
            }
        }
        ```
    
    55. 相交链表（160）：
    
        方法一：HashSet
    
        判断两个链表是否相交，可以使用哈希集合存储某一条链表的所有节点。
    
        首先遍历链表 headA，并将链表 headA 中的每个节点加入哈希集合中。然后遍历链表 headB，对于遍历到的每个节点，判断该节点是否在哈希集合中：
    
        如果当前节点不在哈希集合中，则继续遍历下一个节点；
    
        如果当前节点在哈希集合中，则后面的节点都在哈希集合中，即从当前节点开始的所有节点都在两个链表的相交部分，因此在链表 headB 中遍历到的第一个在哈希集合中的节点就是两个链表相交的节点，返回该节点。
    
        如果链表 headB 中的所有节点都不在哈希集合中，则两个链表不相交，返回 null。
    
        ```java
        public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
            HashSet<ListNode> hashSet = new HashSet<>();
            while (headA != null) {
                hashSet.add(headA);
                headA = headA.next;
            }
            while (headB != null) {
                if (hashSet.contains(headB)) {
                    return headB;
                }
                headB = headB.next;
            }
            return null;
        }
        ```
    
        方法二：双指针
    
        根据题目意思，如果两个链表相交，那么相交点之后的长度是相同的。
    
        我们需要做的事情是，让**两个链表从同距离末尾同等距离的位置开始遍历**。为此，我们必须**消除两个链表的长度差**。有一个重要的前提是，如果让指针a遍历完A链表后再遍历B链表，让指针b遍历完B链表后再遍历A链表，这样两个指针最后一定会走过相同的距离，如果两个链表相交两个指针就一定会落在同一个结点。
    
        下面是算法的具体步骤：
    
        - 当链表 headA 和 headB 都不为空时，创建两个指针 pA 和 pB，初始时分别指向两个链表的头节点 headA 和 headB
        - 每步操作需要同时更新指针 pA 和 pB。
        - 如果指针 pA 不为空，则将指针 pA 移到下一个节点；如果指针 pB 不为空，则将指针 pB 移到下一个节点。
        - 如果指针 pA 为空，则将指针 pA 移到链表 headB 的头节点；如果指针 pB 为空，则将指针 pB 移到链表 headA 的头节点。
        - 当指针 pA 和 pB 指向同一个节点或者都为空时，返回它们指向的节点或者 null。
    
        下面提供双指针方法的正确性证明。考虑两种情况，第一种情况是两个链表相交，第二种情况是两个链表不相交。
    
        情况一：两个链表相交
    
        链表headA 和 headB 的长度分别是 m 和 n。假设链表 headA 的不相交部分有 a 个节点，链表 headB 的不相交部分有 b 个节点，两个链表相交的部分有 c 个节点，则有 a+c=m，b+c=n。
    
        如果 a=b，则两个指针会同时到达两个链表相交的节点，此时返回相交的节点；
    
        如果 a != b，则指针 pA 会遍历完链表 headA，指针 pB 会遍历完链表 headB，两个指针不会同时到达链表的尾节点，然后指针 pA 移到链表 headB 的头节点，指针 pB 移到链表 headA 的头节点，然后两个指针继续移动，在指针 pA 移动了 a+c+b 次、指针 pB 移动了 b+c+a 次之后，两个指针会同时到达两个链表相交的节点，该节点也是两个指针第一次同时指向的节点，此时返回相交的节点。
    
        ![image-20220521220004531](image-20220521220004531.png)
    
        情况二：两个链表不相交
    
        链表 headA 和 headB 的长度分别是 m 和 n。考虑当 m=n 和 m!=n  时，两个指针分别会如何移动：
    
        如果 m=n，则两个指针会同时到达两个链表的尾节点，然后同时变成空值null，此时返回 null；
    
        如果 m != n ，则由于两个链表没有公共节点，两个指针也不会同时到达两个链表的尾节点，因此两个指针都会遍历完两个链表，在指针 pA 移动了 m+n 次、指针 \pB 移动了 n+m 次之后，两个指针会同时指向链表末尾变成空值 null，此时返回 null。
    
        ```java
        public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
            ListNode pa=headA,pb=headB;
            while (pa!=pb){
                pa=pa==null?headB:pa.next;
                pb=pb==null?headA:pb.next;
            }
            return pb;
        }
        ```
    
    56. 多数元素（169）：
    
        方法一：哈希表
    
        我们使用哈希映射（HashMap）来存储每个元素以及出现的次数。对于哈希映射中的每个键值对，键表示一个元素，值表示该元素出现的次数。
    
        我们用一个循环遍历数组 nums 并将数组中的每个元素加入哈希映射中。在这之后，我们遍历哈希映射中的所有键值对，找到出现次数大于n/2的键即可。
    
        ```java
        public int majorityElement(int[] nums) {
            HashMap<Integer, Integer> hashMap = new HashMap<>();
            for (int num : nums) {
                int cnt = hashMap.getOrDefault(num, 0);
                hashMap.put(num, cnt + 1);
            }
            int res = 0;
            for (Map.Entry<Integer, Integer> entry :
                 hashMap.entrySet()) {
                if (entry.getValue() > nums.length / 2) {
                    res = entry.getKey();
                    break;
                }
            }
            return res;
        }
        ```
    
        方法二：排序
    
        如果将数组 nums 中的所有元素按照单调递增或单调递减的顺序排序，那么下标为 n/2 的元素（下标从 0 开始）一定是众数。
    
        算法
    
        对于这种算法，我们先将 nums 数组排序，然后返回上文所说的下标对应的元素。下面的图中解释了为什么这种策略是有效的。在下图中，第一个例子是 n 为奇数的情况，第二个例子是 n 为偶数的情况。
    
        ![image-20220521223252489](image-20220521223252489-1662463137547119.png)
    
        对于每种情况，数组下面的线表示如果众数是数组中的最小值时覆盖的下标，数组上面的线表示如果众数是数组中的最大值时覆盖的下标。对于其他的情况，这条线会在这两种极端情况的中间。对于这两种极端情况，它们会在下标为 n/2 的地方有重叠。因此，无论众数是多少，返回 n/2 下标对应的值都是正确的。
    
        ```java
        public int majorityElement(int[] nums) {
            Arrays.sort(nums);
            return nums[nums.length / 2];
        }
        ```
    
        方法三：[Boyer-Moore 投票算法](https://leetcode.cn/problems/majority-element/solution/duo-shu-yuan-su-by-leetcode-solution/)
    
        注意看题解的评论，有指出官方题解错误的地方
    
    57. 打家劫舍（198）：
    
        经典**动态规划问题**，可以好好领悟一下。
    
        动态规划的的四个解题步骤是：
    
        - 定义子问题
        - 写出子问题的递推关系
        - 确定 DP 数组的计算顺序
        - 空间优化（可选）
    
        步骤一：定义子问题
        稍微接触过一点动态规划的朋友都知道动态规划有一个“子问题”的定义。什么是子问题？子问题是和原问题相似，但规模较小的问题。例如这道小偷问题，原问题是“从全部房子中能偷到的最大金额”，将问题的规模缩小，子问题就是“从 k 个房子中能偷到的最大金额”，用 f(k) 表示。
    
        可以看到，子问题是参数化的，我们定义的子问题中有参数 k。假设一共有 n 个房子的话，就一共有 n 个子问题。动态规划实际上就是通过求这一堆子问题的解，来求出原问题的解。这要求子问题需要具备两个性质：
    
        原问题要能由子问题表示。例如这道小偷问题中，k=n 时实际上就是原问题。否则，解了半天子问题还是解不出原问题，那子问题岂不是白解了。
        一个子问题的解要能通过其他子问题的解求出。例如这道小偷问题中，f(k) 可以由 f(k-1) 和 f(k-2) 求出，具体原理后面会解释。这个性质就是教科书中所说的“最优子结构”。如果定义不出这样的子问题，那么这道题实际上没法用动态规划解。
        小偷问题由于比较简单，定义子问题实际上是很直观的。一些比较难的动态规划题目可能需要一些定义子问题的技巧。
    
        步骤二：写出子问题的递推关系
    
        这一步是求解动态规划问题最关键的一步。然而，这一步也是最无法在代码中体现出来的一步。在做题的时候，最好把这一步的思路用注释的形式写下来。做动态规划题目不要求快，而要确保无误。否则，写代码五分钟，找 bug 半小时，岂不美哉？
    
        我们来分析一下这道小偷问题的递推关系：
    
        首先考虑最简单的情况。如果只有一间房屋，则偷窃该房屋，可以偷窃到最高总金额。如果只有两间房屋，则由于两间房屋相邻，不能同时偷窃，只能偷窃其中的一间房屋，因此选择其中金额较高的房屋进行偷窃，可以偷窃到最高总金额。
    
        如果房屋数量大于两间，应该如何计算能够偷窃到的最高总金额呢？对于第k (k>2) 间房屋，有两个选项：
    
        - 偷窃第 k 间房屋，那么就不能偷窃第 k-1 间房屋，偷窃总金额为前 k−2 间房屋的最高总金额与第 k 间房屋的金额之和。
    
        - 不偷窃第 k 间房屋，偷窃总金额为前 k-1 间房屋的最高总金额。
    
        在两个选项中选择偷窃总金额较大的选项，该选项对应的偷窃总金额即为前 k 间房屋能偷窃到的最高总金额。
    
        这时我们就可以写出状态转移方程了：`dp[i]=max(dp[i-2]+nums[i],dp[i-1])`，注意边界条件只有一间房屋和两间房屋的情况
    
        ```java
        public int rob(int[] nums) {
            if (nums.length==1){//特殊情况判断
                return nums[0];
            }
            int[] dp=new int[nums.length];
            dp[0]=nums[0];
            dp[1]=Math.max(nums[0],nums[1]);
            for (int i = 2; i < nums.length; i++) {
                dp[i]=Math.max(dp[i-2]+nums[i],dp[i-1]);
            }
            return dp[nums.length-1];
        }
        ```
    
    58. 岛屿数量(200)：
    
        方法一：广度优先搜索
    
        为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 1，则将其加入队列，开始进行广度优先搜索。在广度优先搜索的过程中，**每个搜索到的 1 都会被重新标记为 0**，这样就相当于把岛屿所联通的区域清除，表示已经搜索过了（标记为2是更好的做法，更能区分出这是已经遍历过的位置），不会干扰后面接着扫描二维网络。直到队列为空，搜索结束。
    
        最终岛屿的数量就是我们进行广度优先搜索的次数。
    
        ```java
        public int numIslands(char[][] grid) {
            if (grid == null || grid.length == 0) {
                return 0;
            }
            int res = 0;
            //t=x*column+y,注意一下这里的写法，将二维的信息保存在一维中，
            //因为后面的y一定小于column，所以t%column可以正确求出y,t/column求出x
            int m = grid.length, n = grid[0].length;
            for (int i = 0; i < grid.length; i++) {
                for (int j = 0; j < grid[0].length; j++) {
                    if (grid[i][j] == '1') {
                        res++;
                        ArrayList<Integer> island = new ArrayList<>();
                        island.add(i * n + j);
                        while (island.size() > 0) {
                            Integer cur = island.remove(0);
                            int x = cur / n, y = cur % n;
                            grid[x][y] = '0';
                            if (x + 1 < m && grid[x + 1][y] == '1') {
                                grid[x + 1][y] = '0';
                                island.add((x + 1) * n + y);
                            }
                            if (x - 1 >= 0 && grid[x - 1][y] == '1') {
                                grid[x - 1][y] = '0';
                                island.add((x - 1) * n + y);
                            }
                            if (y + 1 < n && grid[x][y + 1] == '1') {
                                grid[x][y + 1] = '0';
                                island.add(x * n + y + 1);
                            }
                            if (y - 1 >= 0 && grid[x][y - 1] == '1') {
                                grid[x][y - 1] = '0';
                                island.add(x * n + y - 1);
                            }
                        }
                    }
                }
            }
            return res;
        }
        ```
    
        方法二：深度优先搜索
    
        我们可以将二维网格看成一个无向图，竖直或水平相邻的 1 之间有边相连。
    
        为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 1，则以其为起始节点开始进行深度优先搜索。在深度优先搜索的过程中，每个搜索到的 1 都会被重新标记为 0，表示已经搜索过了。
    
        最终岛屿的数量就是我们进行深度优先搜索的次数。
    
        ```java
        public void dfs(int x, int y, char[][] grid) {
            if (x >= grid.length || x < 0 || y >= grid[0].length || y < 0 || grid[x][y] == '0') {
                return;
            }
            grid[x][y] = '0';
            dfs(x + 1, y, grid);
            dfs(x - 1, y, grid);
            dfs(x, y + 1, grid);
            dfs(x, y - 1, grid);
        }
        
        public int numIslands(char[][] grid) {
            if (grid == null || grid.length == 0) {
                return 0;
            }
            int res = 0;
            for (int i = 0; i < grid.length; i++) {
                for (int j = 0; j < grid[0].length; j++) {
                    if (grid[i][j] == '1') {
                        res++;
                        dfs(i, j, grid);
                    }
                }
            }
            return res;
        }
        ```
    
    59. 翻转链表(206)：
    
        简单题，可以复习一下链表，做链表题目的时候先画图，重点就是把图画出来按着图去做
    
        ```java
        public ListNode reverseList(ListNode head) {
            if (head==null){
                return null;
            }
            ListNode cur=head;
            head=head.next;
            cur.next=null;
            while (head!=null){
                ListNode temp=head.next;
                head.next=cur;
                cur=head;
                head=temp;
            }
            return cur;
        }
        ```
    
    60. 课程表(207)：
    
        本题是一道经典的「拓扑排序」问题。
    
        给定一个包含 n 个节点的有向图 G，我们给出它的节点编号的一种排列，如果满足：对于图 G 中的任意一条有向边 (u, v)，u 在排列中都出现在 v 的前面。那么称该排列是图 G 的「拓扑排序」。根据上述的定义，我们可以得出两个结论：
    
        如果图 G 中存在环（即图 G 不是「有向无环图」），那么图 G 不存在拓扑排序。
    
        如果图 G 是有向无环图，那么它的拓扑排序可能不止一种。举一个最极端的例子，如果图 G 值包含 n 个节点却没有任何边，那么任意一种编号的排列都可以作为拓扑排序。
    
        有了上述的简单分析，我们就可以将本题建模成一个求拓扑排序的问题了：
    
        我们将每一门课看成一个节点；
    
        如果想要学习课程 A 之前必须完成课程 B，那么我们从 B 到 A 连接一条有向边。这样以来，在拓扑排序中，B 一定出现在 A 的前面。
    
        求出该图是否存在拓扑排序，就可以判断是否有一种符合要求的课程学习顺序
    
        方法一：深度优先搜索
    
        **思路**
    
        我们可以将深度优先搜索的流程与**拓扑排序**(离散学过)的求解联系起来，用一个栈来存储所有已经搜索完成的节点。
    
        对于一个节点 u，如果它的所有相邻节点都已经搜索完成，那么在搜索回溯到 u 的时候，u 本身也会变成一个已经搜索完成的节点。这里的「相邻节点」指的是从 u 出发通过一条有向边可以到达的所有节点。
    
        假设我们当前搜索到了节点 u，如果它的所有相邻节点都已经搜索完成，那么这些节点都已经在栈中了，此时我们就可以把 u 入栈。可以发现，如果我们从栈顶往栈底的顺序看，由于 u 处于栈顶的位置，那么 u 出现在所有 u 的相邻节点的前面。因此对于 u 这个节点而言，它是满足拓扑排序的要求的。
    
        这样以来，我们对图进行一遍深度优先搜索。当每个节点进行回溯的时候，我们把该节点放入栈中。最终从栈顶到栈底的序列就是一种拓扑排序。
    
        **算法**
    
        对于图中的任意一个节点，它在搜索的过程中有三种状态，即：
    
        「未搜索」：我们还没有搜索到这个节点；
    
        「搜索中」：我们搜索过这个节点，但还没有回溯到该节点，即该节点还没有入栈，还有相邻的节点没有搜索完成）；
    
        「已完成」：我们搜索过并且回溯过这个节点，即该节点已经入栈，并且所有该节点的相邻节点都出现在栈的更底部的位置，满足拓扑排序的要求。
    
        通过上述的三种状态，我们就可以给出使用深度优先搜索得到拓扑排序的算法流程，在每一轮的搜索搜索开始时，我们任取一个「未搜索」的节点开始进行深度优先搜索。
    
        我们将当前搜索的节点 u 标记为「搜索中」，遍历该节点的每一个相邻节点 v：
    
        如果 v 为「未搜索」，那么我们开始搜索 v，待搜索完成回溯到 u；
    
        如果 v 为「搜索中」，那么我们就找到了图中的一个环，也就是递归搜索的时候遇到了之前搜索过的点，所以存在环，因此是不存在拓扑排序的；
    
        如果 v 为「已完成」，那么说明 v 已经在栈中了，而 u 还不在栈中，因此 u 无论何时入栈都不会影响到 (u, v)之前的拓扑关系，以及不用进行任何操作。
    
        当 u 的所有相邻节点都为「已完成」时，我们将 u 放入栈中，并将其标记为「已完成」。
    
        在整个深度优先搜索的过程结束后，如果我们没有找到图中的环，那么栈中存储这所有的 n 个节点，从栈顶到栈底的顺序即为一种拓扑排序。
    
        ```java
        // 存储有向图，存的是每一个顶点可以到的所有其他顶点，这样来表示边
        List<List<Integer>> edges;
        // 标记每个节点的状态：0=未搜索，1=搜索中，2=已完成
        int[] visited;
        // 判断有向图中是否有环
        boolean valid = true;
        
        public boolean canFinish(int numCourses, int[][] prerequisites) {
            edges = new ArrayList<>();
            for (int i = 0; i < numCourses; i++) {
                edges.add(new ArrayList<>());
            }
            visited = new int[numCourses];
            for (int[] prereq :
                 prerequisites) {
                //要学0 先学1，所以存在从1到0的边，get(1).add(0)
                edges.get(prereq[1]).add(prereq[0]);
            }
            // 每次挑选一个「未搜索」的节点，开始进行深度优先搜索
            for (int i = 0; i < numCourses && valid; i++) {
                if (visited[i]==0){
                    dfs(i);
                }
            }
            return valid;
        }
        
        private void dfs(int u) {
            //先将u标记为搜索中
            visited[u]=1;
            for (int v :
                 edges.get(u)) {
                // 如果「未搜索」那么搜索相邻节点v
                if (visited[v]==0) {
                    dfs(v);
                    //剪枝，搜v结点已经有环了，就不用在搜索和u相邻的其他顶点了
                    if (!valid){
                        return;
                    }
                }else if (visited[v]==1){
                    valid=false;
                    return;
                }
            }
            //u的所有相邻顶点搜索完后，再将u标记为已完成
            visited[u]=2;
        }
        ```
    
        方法二：广度优先搜索
    
        **思路**
    
        方法一的深度优先搜索是一种「逆向思维」：最先被放入栈中的节点是在拓扑排序中最后面的节点。我们也可以使用正向思维，顺序地生成拓扑排序，这种方法也更加直观。
    
        我们考虑拓扑排序中最前面的节点，**该节点一定不会有任何入边，也就是它没有任何的先修课程要求**。当我们将一个节点加入答案中后，我们就可以移除它的所有出边，代表着**它的相邻节点少了一门先修课程的要求**。如果某个相邻节点变成了「没有任何入边的节点」，那么就代表着这门课可以开始学习了。按照这样的流程，我们不断地将没有入边的节点加入答案，直到答案中包含所有的节点（得到了一种拓扑排序）或者不存在没有入边的节点（图中包含环）。
    
        上面的想法类似于广度优先搜索，因此我们可以将广度优先搜索的流程与拓扑排序的求解联系起来。
    
        **算法**
    
        我们使用一个队列来进行广度优先搜索。开始时，所有入度为 0 的节点都被放入队列中，他们没有先修课程的要求，可以直接学，它们就是可以作为拓扑排序最前面的节点，并且它们之间的相对顺序是无关紧要的。
    
        在广度优先搜索的每一步中，我们取出队首的节点 u：
    
        我们将 u 放入答案中；
    
        我们移除 u 的所有出边，也就是将 u 的所有相邻节点的入度减少 1。如果某个相邻节点 v 的入度变为 0，那么我们就将 v 放入队列中。
    
        在广度优先搜索的过程结束后。如果答案中包含了这 n 个节点，那么我们就找到了一种拓扑排序，否则说明图中存在环，也就不存在拓扑排序了。
    
        ```java
        public boolean canFinish(int numCourses, int[][] prerequisites) {
            List<List<Integer>> edges = new ArrayList<>();
            int[] indeg = new int[numCourses];
            for (int i = 0; i < numCourses; i++) {
                edges.add(new ArrayList<>());
            }
            for (int[] info :
                 prerequisites) {
                //从1出发指向0的边
                edges.get(info[1]).add(info[0]);
                indeg[info[0]]++;
            }
            Deque<Integer> queue = new LinkedList<>();
            for (int i = 0; i < numCourses; i++) {
                // 将所有入度为 0 的节点放入队列中，意味着队列中的课可以先学不需要先修课程
                if (indeg[i] == 0) {
                    queue.add(i);
                }
            }
            int res = 0;
            while (!queue.isEmpty()) {
                int u = queue.removeFirst();
                res++;
                for (int v :
                     edges.get(u)) {
                    //将u的相邻结点的入度减少一
                    indeg[v]--;
                    // 如果相邻节点 v 的入度为 0，就可以选 v 对应的课程了
                    if (indeg[v] == 0) {
                        queue.add(v);
                    }
                }
            }
            return res == numCourses;
        }
        ```
    
        **总结**：深度优先搜索的时候做标记一定要先看到底有几种标记，想好每一种标记在什么时候打上，这和实际问题有关，但模板都差不多，这个题对图的这种存储方式很值得学习，用`List<List<Integer>> edges = new ArrayList<>();`存储边，edges.get(0)意味着拿到从顶点0出发可以到达的所有点的一个List
    
    61. 数组中的第k个最大元素(215)：
    
        我直接排序后（升序）从后往前找出第k个就好了，想了解快速选择算法去看看[官方题解](https://leetcode.cn/problems/kth-largest-element-in-an-array/solution/shu-zu-zhong-de-di-kge-zui-da-yuan-su-by-leetcode-/)，这题也可以用堆排序来做，建立大顶堆，做 k - 1 次删除堆顶的操作后堆顶元素就是我们要找的答案
    
    62. 会议室2：
    
        ![image-20220620111447008](image-20220620111447008.png)
    
        **思路**：
    
        先对所有的会议安排按照开始时间升序排列。
    
        安排第一个会议，此时一个会议室都没有，直接开放一间会议室使用；
    
        安排第 i 个会议，查看当前有没有会议室是已开放且空闲的，没有则接着开放新的会议室；
    
        查看是否有会议室已开放且空闲，是看当前正在使用会议室的会议中，**最早结束的那场会议的结束时间**，如果现在还没结束，说明其他会议更不可能结束，直接开放新的会议室。
    
        若在已开放的会议室中，最早结束的那场会议已经结束，说明现在存在空闲会议室，直接加入即可。
    
        **算法**
    
        1、将最先开始的会议的结束时间加入小顶堆，最先开始的会议肯定要先进行
    
        2、接着对 [1, size-1] 个会议依次进行操作：对比当前会议的开始时间和小顶堆的堆顶元素值，若小于，说明当前所有会议室正在进行的会议中，最早结束的都还没结束，只能新建会议室了，也就是将其加入小顶堆；
    
        3、若当前会议的开始时间大于等于小顶堆的堆顶元素值，说明会议室正在进行的会议中，最早结束的会议已经结束，可以把它从小顶堆删除，自己进入小顶堆（重复利用会议室）。
    
        4、等最后一个会议时间进入小顶堆，此时的小顶堆元素个数即至少需要的会议室数量。
    
        ```java
        public int minMeetingRooms(int[][] intervals) {
            if (intervals == null || intervals.length == 0) {
                return 0;
            }
            PriorityQueue<Integer> heap = new PriorityQueue<>((o1, o2) -> o1 - o2);//小顶堆
            Arrays.sort(intervals, (o1, o2) -> o1[0] - o2[0]);//按照会议开始时间升序排列
            //将最早开始的会议的结束时间加入小顶堆，最早开始的会议肯定要先安排会议室
            heap.add(intervals[0][1]);
            // 小顶堆中堆顶始终是最早结束的会议的时间
            for (int i = 1; i < intervals.length; i++) {
                // 如果当前会议的开始时间大于等于前面已经开始的会议中最早结束的时间
                // 说明有会议室空闲出来了，可以直接重复利用
                // 把堆顶会议删除，当前的会议结束时间加入堆，意味着当前会议在进行
                if (intervals[i][0] >= heap.peek()) {
                    heap.poll();
                }
                //把当前会议的结束时间加入小顶堆
                heap.add(intervals[i][1]);
            }
            // 当所有会议遍历完毕，还在最小堆里面的，说明会议还没结束，此时的数量就是会议室的最少数量
            return heap.size();
        }
        ```
    
    63. 最大正方形：
    
        方法一：暴力模拟
    
        遍历矩阵中的每个元素，每次遇到 1，则将该元素作为正方形的左上角；
    
        确定正方形的左上角后，根据**左上角所在的行和列计算可能的最大正方形的边长**（正方形的范围不能超出矩阵的行数和列数），在该边长范围内寻找只包含 1 的最大正方形；
    
        每次在**下方新增一行**以及在**右方新增一列**，判断新增的行和列是否满足所有元素都是 1。
    
        ```java
        public int maximalSquare(char[][] matrix) {
            if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
                return 0;
            }
            int maxSide = 0, m = matrix.length, n = matrix[0].length;
            for (int i = 0; i < matrix.length; i++) {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (matrix[i][j] == '1') {
                        maxSide = Math.max(maxSide, 1);
                        // 计算可能的最大正方形边长
                        int curMax = Math.min(m - i, n - j);
                        for (int k = 1; k < curMax; k++) {
                            boolean flag = true;
                            //新增的一行一列的右下角那个特判
                            if (matrix[i + k][j + k] == '0') {
                                break;
                            }
                            for (int l = 0; l < k; l++) {
                                //判断新增的一行一列是不是都是1
                                if (matrix[i + k][j + l] == '0' 
                                    || matrix[i + l][j + k] == '0') {
                                    flag = false;
                                    break;
                                }
                            }
                            if (flag) {
                                maxSide = Math.max(maxSide, k + 1);
                            } else {
                                break;
                            }
                        }
                    }
                }
            }
            return maxSide * maxSide;
        }
        ```
    
        方法二：动态规划
    
        可以使用动态规划降低时间复杂度。我们用 dp(i,j) 表示以 (i, j) 为右下角，且只包含 1 的正方形的边长最大值。如果我们能计算出所有 dp(i,j) 的值，那么其中的最大值即为矩阵中只包含 1 的正方形的边长最大值，其平方即为最大正方形的面积。
    
        那么如何计算 dp 中的每个元素值呢？对于每个位置 (i,j)，检查在矩阵中该位置的值：
    
        如果该位置的值是 0，则 dp(i,j)=0，因为当前位置不可能在由 1 组成的正方形中；
    
        如果该位置的值是 1，则 dp(i,j) 的值由其上方、左方和左上方的三个相邻位置的 dp 值决定。具体而言，当前位置的元素值等于三个相邻位置的元素中的最小值加 1，状态转移方程如下：
    
        `dp(i, j)=min(dp(i−1, j), dp(i−1, j−1), dp(i, j−1))+1`
    
        此外，还需要考虑边界条件。如果 i 和 j 中至少有一个为 0，则以位置 (i,j) 为右下角的最大正方形的边长只能是 1，因此 dp(i,j)=1。
    
        该状态转移方程的推导可以看看[官方题解](https://leetcode.cn/problems/count-square-submatrices-with-all-ones/solution/tong-ji-quan-wei-1-de-zheng-fang-xing-zi-ju-zhen-2/)，看懂后该类正方形的问题应该都能明白
    
        ```java
        public int maximalSquare(char[][] matrix) {
            if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
                return 0;
            }
            int maxSide = 0;
            int rows = matrix.length, column = matrix[0].length;
            int[][] dp = new int[rows][column];
            for (int i = 0; i < rows; i++) {
                for (int j = 0; j < column; j++) {
                    if (matrix[i][j] == '1') {
                        if (i == 0 || j == 0) {
                            dp[i][j] = 1;
                        } else {
                            dp[i][j] = Math.min(dp[i - 1][j], Math.min(dp[i - 1][j - 1], dp[i][j - 1])) + 1;
                        }
                        maxSide = Math.max(maxSide, dp[i][j]);
                    }
                }
            }
            return maxSide*maxSide;
        }
        ```
    
    64. 除自身以外数组的乘积
    
        方法一：左右乘积列表
    
        思路
    
        我们不必将所有数字的乘积除以给定索引处的数字得到相应的答案，而是利用索引左侧所有数字的乘积和右侧所有数字的乘积（即前缀与后缀）相乘得到答案。
    
        对于给定索引 i，我们将使用它左边所有数字的乘积乘以右边所有数字的乘积。
    
        `left[i]=left[i-1]*nums[i-1]`, `right[i]=right[i+1]*nums[i+1]`
    
        ```java
        public int[] productExceptSelf(int[] nums) {
            int[] left = new int[nums.length];
            int[] right = new int[nums.length];
            //left[i]=left[i-1]*nums[i-1]
            left[0]=1;
            for (int i = 1; i < nums.length; i++) {
                left[i]=left[i-1]*nums[i-1];
            }
            right[nums.length-1]=1;
            for (int i = nums.length-2; i >=0; i--) {
                right[i]=right[i+1]*nums[i+1];
            }
            int[] res=new int[nums.length];
            for (int i = 0; i < nums.length; i++) {
                res[i]=left[i]*right[i];
            }
            return res;
        }
        ```
    
        上述方法还可以优化空间，我们先把输出数组当作left数组，计算出left数组的值，然后**从右向左遍历**，动态的构造出right数组，只需要一个变量保存right当前的值就好
    
        ```java
        public int[] productExceptSelf(int[] nums) {
            int len=nums.length;
            int[] ans = new int[len];
            ans[0]=1;
            for (int i = 1; i < len; i++) {
                ans[i]=ans[i-1]*nums[i-1];
            }
            int right=1;
            for (int i = len-1; i >=0 ; i--) {
                // 对于索引 i，左边的乘积为 answer[i]，右边的乘积为 R
                ans[i]=ans[i]*right;
                // R 需要包含右边所有的乘积，所以计算下一个结果时需要将当前值乘到 R 上
                right=right*nums[i];
            }
            return ans;
        }
        ```
    
    65. 搜索二维矩阵2(240)：
    
        方法一：每行进行二分查找
    
        ```java
        public boolean searchMatrix(int[][] matrix, int target) {
            for (int[] row :
                 matrix) {
                int res = search(row, target);
                if (res >= 0) {
                    return true;
                }
            }
            return false;
        }
        
        public int search(int[] nums, int target) {
            int left = 0, right = nums.length - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (nums[mid] > target) {
                    right = mid - 1;
                } else if (nums[mid] < target) {
                    left = mid + 1;
                } else {
                    return mid;
                }
            }
            return -1;
        }
        ```
    
        方法二：z字形查找
    
        我们可以从矩阵 matrix 的右上角 (0,n−1) 进行搜索。在每一步的搜索过程中，如果我们位于位置 (x,y)，那么我们希望在以 matrix 的左下角为左下角、以 (x,y) 为右上角的矩阵中进行搜索，即行的范围为 [x,m−1]，列的范围为 [0,y]：
    
        小tips：matrix[x,y]始终保持在上述的搜索区域是一行最大，一列最小的元素
    
        如果 matrix[x,y]=target，说明搜索完成；
    
        如果 matrix[x,y]>target，由于每一列的元素都是升序排列的，那么在当前的搜索矩阵中，所有位于**第 y 列**的元素都是严格大于 target 的，因此我们可以将它们全部忽略，即将 y 减少 1；
    
        如果 matrix[x,y]<target，由于每一行的元素都是升序排列的，那么在当前的搜索矩阵中，所有位于**第 x 行**的元素都是严格小于 target 的，因此我们可以将它们全部忽略，即将 x 增加 1。
    
        在搜索的过程中，如果我们超出了矩阵的边界，那么说明矩阵中不存在 target。
    
        ```java
        public boolean searchMatrix(int[][] matrix, int target){
            int m=matrix.length,n=matrix[0].length;
            int x=0,y=n-1;//矩阵的右上角，行最大的元素，列最小的元素
            while (x<m&&y>=0){
                if (target>matrix[x][y]){
                    x++;
                }else if (target<matrix[x][y]){
                    y--;
                }else{
                    return true;
                }
            }
            return false;
        }
        ```
    
    66. 完全平方数(279)：
    
        方法一：动态规划
    
        我们可以依据题目的要求写出状态表达式：f[i]表示最少需要多少个数的平方来表示整数 i。这些数必然落在区间 [1,sqrt(n)]。我们可以枚举这些数，假设当前枚举到 j，那么我们还需要取若干数的平方，构成 i-j^2 此时我们发现该子问题和原问题类似，只是规模变小了。这符合了动态规划的要求，于是我们可以写出状态转移方程 `dp[i]=min(dp[i],dp[i-j*j]+1),j从1枚举到sqrt(n)`
    
        边界条件，将dp[0]初始化为0，为了满足j*j恰好为n的情况
    
        题目有个坑，不一定每次都取最大的平方数，结果就是用了最少的数字，比如41，如果取36，那么还需要5=4+1，总共3个数字，但可以直接取41=25+16总共2个数字
    
        ```java
        public int numSquares(int n) {
            int[] dp = new int[n + 1];
            dp[0] = 0;
            // 5
            // 0 1 2 3 1 2
            for (int i = 1; i <= n; i++) {
                //将当前数字先更新为最大的结果，
                //即 dp[i]=i，比如 i=4，最坏结果为 4=1+1+1+1 即为 4 个数字
                dp[i] = i;
                for (int j = 1; j * j <= i; j++) {
                    dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
                }
            }
            return dp[n];
        }
        ```
    
    67. 实现Trie（前缀树）：208题
    
        ```c++
        class Trie {
            private:
            bool isEnd;
            Trie* next[26];
            public:
            Trie() {
                isEnd = false;
                memset(next, 0, sizeof(next));
            }
        /**
         * 这个操作和构建链表很像。首先从根结点的子结点开始与 word 第一个字符进行匹配，
         * 一直匹配到前缀链上没有对应的字符，这时开始不断开辟新的结点，直到插入完 word 的最后一个字符，
         * 同时还要将最后一个结点isEnd = true;，表示它是一个单词的末尾。
         * @param word 
         */
            void insert(string word) {
                Trie* node = this;
                for (char c : word) {
                    if (node->next[c-'a'] == NULL) {
                        node->next[c-'a'] = new Trie();
                    }
                    node = node->next[c-'a'];
                }
                node->isEnd = true;
            }
        /**
         *从根结点的子结点开始，一直向下匹配即可，如果出现结点值为空就返回 false，
         * 如果匹配到了最后一个字符，那我们只需判断 node->isEnd即可。
         * @param word
         * @return
         */
            bool search(string word) {
                Trie* node = this;
                for (char c : word) {
                    node = node->next[c - 'a'];
                    if (node == NULL) {
                        return false;
                    }
                }
                return node->isEnd;
            }
        /**
         * 和 search 操作类似，只是不需要判断最后一个字符结点的isEnd，
         * 因为既然能匹配到最后一个字符，那后面一定有单词是以它为前缀的。
         * @param prefix
         * @return
         */
            bool startsWith(string prefix) {
                Trie* node = this;
                for (char c : prefix) {
                    node = node->next[c-'a'];
                    if (node == NULL) {
                        return false;
                    }
                }
                return true;
            }
        };
        ```
    
        **总结**
        通过以上介绍和代码实现我们可以总结出 Trie 的几点性质：
    
        - Trie 的形状和单词的插入或删除顺序无关，也就是说对于任意给定的一组单词，Trie 的形状都是唯一的。
    
        - 查找或插入一个长度为 L 的单词，访问 next 数组的次数最多为 L，和 Trie 中包含多少个单词无关。
    
        - Trie 的每个结点中都保留着一个字母表，这是很耗费空间的。如果 Trie 的高度为 n，字母表的大小为 m，最坏的情况是 Trie 中还不存在前缀相同的单词，那空间复杂度就为 O(m^n)
    
        最后，关于 Trie 的应用场景，希望你能记住 8 个字：**一次建树，多次查询**。(慢慢领悟叭~~)


​        
