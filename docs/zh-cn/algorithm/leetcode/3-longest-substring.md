## 一、前言
<br>
&emsp;&emsp;1024程序节，祝大家节日快乐呀！
<br>

&emsp;&emsp;最近有小伙伴和我谈心，觉得刷算法题太难了，完全没有思路，很有挫败感，想要放弃了。想想自己也深有感触，有这些想法真都挺正常的，**其实我们刷算法就是为了培养一个思考问题、解决问题的思维，这个思维养成并不是一蹴而就的，而是循序渐进的。**所以，这个是成长的必经之路，取经还要经历九九百十一难呢，但只要坚持下来，每天都是一个重生的自己，小白变成大神的路注定不会简单，但你愿意一直碌碌为为，还安慰自己平凡可贵？
<br>

&emsp;&emsp;坚持下来，就能剥茧成蝶！请继续跟小诚一起学习，小诚会尽自己最大的努力，将每道算法题从题目剖析到解决方案都讲得更加通俗理解，与大家共同成长。**如果大家刷题时，感觉到挫败了，想要放弃了，欢迎找小诚吐槽，小诚会给你充满能量再出发！**
<br>

## 二、无重复字符的最长子串
<br>

&emsp;&emsp;心里话讲完了，来看看今天遇到的Boss: 《无重复字符的最长子串》。
<br>

&emsp;&emsp;Boss介绍：给定一个字符串 s ，请你找出其中不含有重复字符的最长子串的长度。
<br>

&emsp;&emsp;示例：
<br>

```

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例2：

输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例三：

输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

示例4：

输入: s = ""
输出: 0
```
<br>

&emsp;&emsp;通过提示：
<br>

- 0 <= s.length <= 5 * 10^4^
- s 由英文字母、数字、符号和空格组成

<br>

## 三、题目分析
<br>

&emsp;&emsp;这个题目看起来其实相对来说比较简单，但是如果不能很好的理解它其中包含的细节，也很容易被误导方向。
<br>

&emsp;&emsp;**细节一：子串和子序列的区别**
<br>

&emsp;&emsp;**子串：**子串是必须能够从原字符串中找到的，在原字符串必须连续的一段。如：字符串abc中ab、bc都为子串。
<br>

&emsp;**&emsp;子序列：**原序列中可以不连续的一段字符，如;字符串abc中ac即为字符序列
<br>

&emsp;&emsp;**细节二：字符串由英文字母、数字、符号和空格组成**
<br>

&emsp;&emsp;字符串不包含中文，这样方便我们使用第四种解决方案(猜猜是啥)

<br>

## 四、通关方式一：暴力破解

<br>

&emsp;&emsp;**方案介绍：**暴力破解，通过多个循环以穷举的方式 找出答案。简单且易想到的方案，包括之前我们刷的两道算法题也可以使用暴力破解来解决，缺点就是虽然简单，但是效率非常低，并非是最优方案。
<br>

&emsp;&emsp;**具体思路：**将字符串每个字符作为一个循环，和子串的组合进行比较，从而获取最长字串长度。如字符串abc，则第一轮比较为: 字符 a和子串a、ab、abc比较，第二轮则为字符b和子串b、bc比较，以此类推，最后获取不重复的子串长度。
<br>

**&emsp;&emsp;解题代码：**
<br>

```java
 /**
     * 方案一：暴力破解
          *
          * @param s
               * @return
               */
            public static Integer lengthOfLongestSubstring(String s) {
            int maxLength = 0;
            //  每轮是用一个字符和子串进行比较，如果不存在重复字符则获取最大长度，知道最后一个字符位置
            for (int i = 0; i < s.length(); i++) {
                for (int j = i + 1; j <= s.length(); j++) {
                if (!judgeCharacterExist(i, j, s)) {
                    maxLength = Math.max(maxLength, j - i);
                }
                }
            }
            return maxLength;
            }


/**
 * 判断子串中是否存在重复字符
 *
 * @param start
 * @param end
 * @param param
 * @return
 */
private static Boolean judgeCharacterExist(int start, int end, String param) {
    HashSet<Character> hashSet = new HashSet<>();
    for (int i = start; i < end; i++) {
        if (hashSet.contains(param.charAt(i))) {
            return true;
        }
        hashSet.add(param.charAt(i));
    }
    return false;
}
```
<br>

&emsp;&emsp;**算法时间复杂度推导：** 
<br>

&emsp;&emsp;通过上面实现的代码可知，**随着输入问题规模增大(既字符串长度编程)，需要进行判断的逻辑之间的函数为f(n) = n^3^，根据大O记法可推导出，暴力破解算法的时间复杂度为O(n^3^)**，三次方的函数，可想而知当输入规模变大时它的效率有多低了。
<br>


&emsp;&emsp;**解题结果：**放到leetcode上执行代码是没有问题的，但是提交的时候会提示超过，因为提交时需要验证987个案例，有些案例字符串包含字符非常多，最后导致执行超时，因此这个方案是绝对不推荐的！

<br>



![image-20211024193021675](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326175906.png)


<br>

## 五、通关方式二：滑动窗口法

<br>

&emsp;&emsp;滑动窗口：是数组和字符串中一个抽象的概念，可以分为滑动和窗口两个概念理解。<br>

&emsp;&emsp;**窗口：即表示一个范围，通常是字符串和数组从开始到结束两个索引范围中间包含的一系列元素集合。**如字符串abcd，如果开始索引和结束索引分别为0、2的话，这个窗口包含的字符则为：abc。
<br>

&emsp;&emsp;**滑动：它表示窗口的开始和结束索引是可以往某个方向移动的。**如上面的例子开始索引和结束索引分别为0、2的话，当开始索引和结束索引都往右移动一位时，它们的索引值则分别为1、3，这个窗口包含的字符为：bcd。
<br>

&emsp;&emsp;下面使用具体的图片来更加形象地认识“滑动窗口”：
<br>

![image-20211024195335027](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326175914.png)

<br>

&emsp;&emsp;介绍完“滑动窗口”的概念后，下面我们来看看如何通过这个方式来解决无重复字符的最长子串问题，思路如下：
<br>

&emsp;**&emsp;使用一个HashSet来实现滑动窗口，用来检查重复字符。**维护开始和结束两个索引，默认都是从0开始，然后随着循环【向右移动结束索引】，遇到不是重复字符则放入窗里，遇到重复字符则【向右侧移动开始索引】，最终得到结果，下面来看具体图解：
<br>

&emsp;&emsp;代码如下：

```java
    /**
     * 方案二：滑动窗口
          *
          * @param s
               * @return
               */
         public static Integer lengthOfLongestSubstring2(String s) {
            int maxLength = 0;
            // 左指针
            int leftPoint = 0;
            // 右指针
            int rightPoint = 0;
            // 用于判断重复的Set集合(窗口)
            Set<Character> set = new HashSet<>();
            while (leftPoint < s.length() && rightPoint < s.length()) {
                if (!set.contains(s.charAt(rightPoint))) {
                // 不存在重复字符时将字符存储入set，并将右边指针向后移动一位
                set.add(s.charAt(rightPoint++));
                maxLength = Math.max(maxLength, rightPoint - leftPoint);
                } else {
                // 存在重复元素，则将左边指针向右移动一位(删除与当前相同的字符)
                set.remove(s.charAt(leftPoint++));
                }
            }
            return maxLength;
            }

```

<br>

![image-20211024202212197](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326175924.png)



&emsp;&emsp;**算法时间复杂度推导：** 
<br>

&emsp;&emsp;通过上面实现的代码可知，随着输入问题规模增大(既字符串长度编程)，需要进行判断的逻辑之间的函数为f(n) = n，根据大O记法可推导出，暴力破解算法的时间复杂度为O(n)，相比于暴力破解，它的效率大大提高了。
<br>

&emsp;&emsp;**执行结果：**
<br>

![image-20211024203629134](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326175932.png)

<br>

## 五、通关方式三：滑动窗口法优化(一)


<br>


&emsp;&emsp;虽然方式使用了滑动窗口时间复杂度只有O(n)，但是如果存在重复字符还需要移动开始索引，因此我们可以考虑借助之前其他算法谈到的“空间换时间”的想法，通过借助HashMap建立字符和索引映射，避免手动移动索引。
<br>


&emsp;&emsp;代码如下：
<br>


```java
 /**
     * 滑动窗口优化一
     *
     * @param s
     * @return
     */
    public static Integer lengthOfLongestSubstring(String s) {
        char[] charArray = s.toCharArray();
        // 使用HashMap来建立字符和索引的映射
        HashMap<Character, Integer> keyMapping = new HashMap<>();
        Integer maxLength = 0;
        // 开始索引
        Integer leftIndex = 0;
        for (int i = 0; i < charArray.length; i++) {
            if (keyMapping.containsKey(charArray[i])) {
            	//  存在重复数据则获取索引值最大的
                leftIndex = Math.max(leftIndex, keyMapping.get(charArray[i]));
            }
            // 值重复则进行覆盖
            keyMapping.put(charArray[i], i + 1);
            maxLength = Math.max(maxLength, i - leftIndex + 1);
        }
        return maxLength;
    }
    
```
<br>

&emsp;&emsp;**算法时间复杂度推导：** 
<br>

&emsp;&emsp;通过上面实现的代码可知，随着输入问题规模增大(既字符串长度编程)，需要进行判断的逻辑之间的函数为f(n) = n，根据大O记法可推导出，暴力破解算法的时间复杂度为O(n)，虽然和上一种方案的时间复杂度是一样的，但是效率还是有一定的提高(思考问题时要思考是否还有其他有优质的方案，培养发散思维)。
<br>

&emsp;&emsp;**执行结果：**
<br>

![image-20211024204035861](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326175940.png)



<br>

## 七、通关方式四：字符与ASCII映射

<br>

&emsp;&emsp;上面题目分析的时候就提到了要注意题目中提到的：【字符串英文字母、数字、符号和空格组成】，这些字符是可以使用ASCII表示(如字符a的ASCII值为97，想具体了解的可以百度下)，那么我们就可以建议字符与ASCII的映射关系，从而实现重复字符的排除。
<br>

&emsp;&emsp;代码如下：

```
public static Integer lengthOfLongestSubstring(String s) {
        int[] index = new int[128];
        int len = 0;
        int length = s.length();
        for (int i = 0, j = 0; j < length; j++) {
            // 如果存在重复字符，则获取最大的下标
            i = Math.max(index[s.charAt(j)], i);
            // 每轮都拿之前与【当前字符一样】最大的下标跟当前下标的差做为最大的字串长度
            len = Math.max(len, j - i + 1);
            index[s.charAt(j)] = j + 1;
        }
        return len;
    }

```
<br>

&emsp;&emsp;**算法时间复杂度推导：** 
<br>

&emsp;&emsp;通过上面实现的代码可知，随着输入问题规模增大(既字符串长度编程)，需要进行判断的逻辑之间的函数为f(n) = n，根据大O记法可推导出，暴力破解算法的时间复杂度为O(n)。
<br>

&emsp;&emsp;**执行结果：**
<br>

![image-20211024213726230](https://it-diary-1308244209.cos.ap-guangzhou.myqcloud.com//image20220326175949.png)


<br>

## 八：算法思想在实际业务中的使用

<br>

&emsp;&emsp;1、滑动窗口算法常用于解决字符串、数组中涉及子元素的一些问题。如本题中的查找无重复字符串最长子串长度。
<br>

&emsp;&emsp;2、滑动窗口算法也可以用于优化for循环，将多重循环转换成单层循环，用于降低时间复杂度，提高执行性能。

<br>

## 九：写在最后

<br>

&emsp;&emsp;一个问题，有些时间不要只纠结于一种解法，可以通过发散思维去多方面考虑，寻找解决方案。发散思维也不是一开始就有，需要不断的联系，积累。因此，每刷一道题，都需要学会自我总结，转换成自己的东西。
<br>

