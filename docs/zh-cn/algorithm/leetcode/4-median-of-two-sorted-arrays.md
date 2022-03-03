

## 一、前言

<br>

&emsp;&emsp;大家好，又到了三分钟算法修行时间，之前挑选的算法都是中低难度的，这次找个难度较高的，看看会遇到啥问题。至于难到啥程度，来看看Leetcode下解题的网友评论。
<br>

<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211104205241116.png" /></center>
<br>

&emsp;&emsp;**本片文章大纲：**
<br>

<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211106175219075.png" /></center>

<br>

## 二、 题目

<br>

&emsp;&emsp;名称：寻找两个正序数组的中位数
<br>

&emsp;&emsp;题意：给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。**请你找出并返回这两个正序数组的中位数 。**
<br>

```java

示例 1：

    输入：nums1 = [1,3], nums2 = [2]
    输出：2.00000
    解释：合并数组 = [1,2,3] ，中位数 2
    
示例 2：

    输入：nums1 = [1,2], nums2 = [3,4]
    输出：2.50000
    解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
    
示例 3：

    输入：nums1 = [0,0], nums2 = [0,0]
    输出：0.00000
    
示例 4：

    输入：nums1 = [], nums2 = [1]
    输出：1.00000
    
示例 5：

    输入：nums1 = [2], nums2 = []
    输出：2.00000

提示：
	
	nums1.length == m
    nums2.length == n
    0 <= m <= 1000
    0 <= n <= 1000
    1 <= m + n <= 2000
    -106 <= nums1[i], nums2[i] <= 106


```
<br>

&emsp;&emsp;**进阶要求：你能设计一个时间复杂度为 O(log (m+n)) 的算法解决此问题吗(注意这个要求，这个要求才是解决这道题目的关键)**

<br>

## 三、题目解析


<br>

&emsp;&emsp;这道题目很简单，就是从两个有序的数组中查询到它们的中文数，**难点在于如何设计一个事件复杂度为O(log(m+n))的算法。**下面抽取题目关键信息来解读题目：
<br>

&emsp;**&emsp;中位数：指顺序排序一组数据中居于中间位置的数值。**它分为两种情况，一是当数组长度为奇数时，正中间的数为中位数，二是当数组长度为偶数时，通常是将最中间的两个数相加取平均值作 为中位数，具体看下图：
<br>

<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211104212750140.png" /></center>



## 解法一：暴力破解

<br>

&emsp;&emsp;相信很多小伙伴一看到题目，脑海中就已经有了这种解题的思路：**将两个数组合并起来 ，然后重新排序，再根据奇偶情况获取合并后的新数组的中位数。**
<br>

### 1、解题代码：
<br>

```java

/**
     * 
     * 方式一：时间复杂度和空间复杂度都为O(m+n)
     *
     * @param nums1
     * @param nums2
     * @return
     */
    public static double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int numLength = nums1.length + nums2.length;
        // 定义合并后的集合
        List<Integer> list = new ArrayList();
		// 使用for循环将元素添加到新的数组(方式一)
        //for (int i : nums1) {
        //    list.add(i);
        //}
        //for (int i : nums2) {
        //    list.add(i);
        //}
        // 使用lambda表达式将数组合并到集合中(方式二)
        list.addAll(Arrays.stream(nums1).boxed().collect(Collectors.toList()));
        list.addAll(Arrays.stream(nums2).boxed().collect(Collectors.toList()));
        // 使用结合工具类的排序方式对存放了两个数组的集合进行排序(sort底层的排序方式是：)
        Collections.sort(list);
        int middle = list.size() / 2;
        // 两数组元素之和为偶数
        if (numLength % 2 == 0) {
            int leftMiddle = middle - 1;
            double middleValue = (Double.valueOf(list.get(leftMiddle)) + Double.valueOf(list.get(middle))) / 2;
            return middleValue;
        } else {
            return Double.valueOf(list.get(middle));
        }
    }

```

<br>

### 2、时间复杂度推导：
<br>

&emsp;&emsp;通过上面的代码，**我们会发现随着输入规模的增大(即数组元素增多)，程序需要花费执行时间处理的语句主要是在将[两个数组元素放入新的集合]以及对这个[新的集合进行排序]的过程。**
<br>

&emsp;&emsp;将两个数组元素合并到一个数组执行函数可以使用函数：f(x)=m +  n(m,n分别为两个数组的长度)表示，根据大O记法的推导可以得到时间复杂度为：O(m + n)
<br>

&emsp;&emsp;对新数组排序的Collections.sort()方法的最坏情况下时间复杂度为：O(n * log(n))。

<br>&emsp;**&emsp;因此，使用暴力破解的方式总的时间复杂度为：O(m+n)  +  O(n * log(n))，这个复杂度如果当数组长度变长后，效率是会比较低的，不推荐使用。**

<br>

### 3、空间复杂度推导：
<br>

&emsp;&emsp;因为每次合并都需要申请一个新的集合来存放两个数组的元素，所以需要申请空间的函数可以表示为：f(x) = m + n，根据大O记法标准推导，**可以得到空间复杂度为：O(m+  n)。**
<br>

### 4、执行结果：

<br>

<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211104224658975.png" /></center>

<br>

## 解法二、双指针法
<br>

&emsp;&emsp;暴力破解法，当输入规模大的时候，效率极低，且没有满足题目的进阶要求，能否对时间复杂度和空间复杂度进一步优化呢？
<br>

&emsp;&emsp;答案是可以的。我们的目的是查询两个有序数组的中位数，**在暴力破解中我们是通过申请新的数组集合来存放两个数组的值然后进行排序，最终得出结果，这一步是否真的需要呢？**
<br>

&emsp;&emsp;答案是不需要，我们目的是查询中位数，中位数无非根据数组长度有奇偶两种情况，**我们可以使用两个指针来指向对应的元素即可，这样我们就可以省略[申请新的数组空间]和对[新数组重新排序]的两个步骤**，从而优化了时间复杂度和空间复杂度，下面来看看具体的代码。
<br>

```
    public static double findMedianSortedArrays2(int[] nums1, int[] nums2) {
        int num1Length = nums1.length;
        int num2Length = nums2.length;
        int sumLength = num1Length + num2Length;
        // 第一个中位数指针
        int preItem = 0;
        // 第二个中位数指针(如果两个数组总长度为奇数，则直接返回该值即可)
        int curItem = 0;
        // 遍历的数组1的元素下标
        int num1Index = 0;
         // 遍历的数组2的元素下标
        int num2Index = 0;
        // 需要遍历的次数(查询中位数，并不需要将两个数组元素都遍历完)
        int foreachTime = sumLength / 2;
        for (int i = 0; i <= foreachTime; i++) {
            // 保证preItem总是在curItem前面
            preItem = curItem;
            // num2Index >= num2Length需要放在nums1[num1Index] < nums2[num2Index]前面，否则会出现下标越界
            // nums1[num1Index] < nums2[num2Index]的目的就是为了按照模拟从小到大的顺序循环两个数组的元素
            if (num1Index < num1Length && (num2Index >= num2Length || nums1[num1Index] < nums2[num2Index])) {
                curItem = nums1[num1Index++];
            } else {
                curItem = nums2[num2Index++];
            }
        }
        // 如果是偶数，则取中间两个元素的平均值
        if (sumLength % 2 == 0) {
            return Double.valueOf(preItem + curItem) / 2;
        }
        return curItem;
    }

```

<br>

### 1、时间复杂度推导

<br>

&emsp;&emsp;根据上面代码可知，随着输入规模的增大(即数组元素增多)，**程序需要执行花费时间处理的语句主要是在for循环中(for循环又只跟两个数组的长度相关)，可以用函数表示为：f(n) = m + n，根据大O记法规则推断，该方式的时间复杂度为：O(m + n)。**
<br>

### 2、空间复杂度推导

<br>

&emsp;&emsp;根据上面解题代码可知，**使用双指针法只需要申请两个指针和一些存储长度、下标的变量对应的空间，并且这些空间并不会随着问题规模的增大(即数组元素的增多)而变化，因此该算法的空间复杂度为：O(1)**

<br>

### 3、执行结果
<br>

<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211104232923375.png" /></center>

<br>

### 4、小结
<br>

&emsp;&emsp;通过定义双指针模拟指向中位数，我们去除了不必要的空间申请和重新排序，进一步优化了算法的时间复杂度和空间复杂度，在平常的业务中，**如果遇到相似的业务要求，可以优先考虑维护指针的方式来避免不必要的空间申请。**
<br>

## 解法三、二分查找法

<br>

&emsp;&emsp;通过双指针，我们将算法的时间复杂度降低到了O(m +n)，但是依然没有达到题目中O(log(m + n))的要求。如何满足这个要求呢，现在好像没有一个比较清晰的思路，这时候不妨再反过来思考下题目的要求。
<br>

&emsp;&emsp;题目中要求时间复杂度需要达到O(log(m + n))，回想下我们之前接触到的算法中，有没有与log(对数)相关的东西，没错，比较常见的就是二分法，每次循环都排除n/2的元素，最终得出结果，下面来看看这个题目如何提取成二分法的形式。
<br>

&emsp;&emsp;题目最终结果是要求中位数，中位数又分为奇偶情况，那我们就可以将抽象求中位数成求有序数组中的第k小数，其中k就是对应的中位数(即 (m + n) /2，或者(m+n)/2 +1)，这样我们就可以对k进行二分查找法找到符合条件的数值。
<br>

### 1、求解第k小数的思路

<br>

&emsp;&emsp;假设存在数组A和数组B，它们的中位数为k，此时要求k的值，则可以通过二分法，**即每轮都对A[k/2]和B[k/2]进行比较(注意：这里的k是表示第几个，如果转换成数组对应的元素的话需要减去1)，如果A[k/2]>=B[k/2]，则B[k/2]和B[k/2]之前的元素就可排除掉**，原因如下：
<br>

&emsp;&emsp;**当A[k/2]>=B[k/2]时，在A数组中比A[k/2]元素小的值有k/2-1个，在数组B中比B[k/2]小的有k-1个，即使A数组中A[k/2]之前的所有元素都比B[k/2]元素小，那总的个数也是等于：k/2-1 + k/2-1 = k-2个，所以B[k/2]最多只能是k-1小的数，而不是第k小数，所以B[k/2]之前的数组更不可能是第k小数，故B[k/2]及之前的元素可以排除掉。反之亦然。**
<br>

&emsp;**&emsp;因为排除掉的元素一定位于数组的前面(数组是有序的)，所以每轮之后k的值也需要减去排除掉的元素的个数**，然后再进行下一轮的第k小数查询，步骤依次类推。
<br>

&emsp;&emsp;通过上面的思路整理，我们可以看出此处使用了递归的思想，**递归的出口则是当某个数组长度为了0时(此时中位数就是可以取不为0的数组中的值即可)或者是k=1(即求第1个小数，此时中位数则取两个数组中起始下标对应值最小的元素)时**，需要注意的是：因为k/2的值可能大于数组的长度，所以每次比较 min(k/2，len(数组) 对应的数字，把小的那个对应的数组的数字排除，将两个新数组进入递归。

<br>

### 2、图解步骤讲解

<br>

&emsp;&emsp;假设存在数组A元素有[1,2,3]和数组B元素有[1,2,3,4,5,6]，因为k=(A数组长度 + B数组长度 + 1)/2 = 5，则根据第k小数的方式查询中位数的步骤如下：
<br>

&emsp;&emsp;第一轮循环：
<br>


<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211106143911682.png" /></center>

<br>

&emsp;&emsp;第二轮循环：
<br>


<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211106143819817.png" /></center>


<br>

&emsp;&emsp;第三轮循环：
<br>


<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211106143733848.png" /></center>


<br>

&emsp;&emsp;第四轮循环：
<br>


<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211106143616678.png" /></center>

<br>

### 3、代码讲解
<br>


```java
 public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int m = nums2.length;
        //奇数时k值，除法是向下取整，因此需要+1
        int left = (n + m + 1) / 2;
     	// 偶数时取两个中间值的平均值做中位数
        int right = (n + m + 2) / 2;
        //将偶数和奇数的情况区分
        if((n + m) % 2 == 0){
        return (getMedian(nums1, 0, n - 1, nums2, 0, m - 1, left) + getMedian(nums1, 0, n - 1, nums2, 0, m - 1, right)) * 0.5;
        } else {
            return getMedian(nums1, 0, n - 1, nums2, 0, m - 1, left);
        }
    }

   public static double getMedian(int[] num1, int num1StartIndex, int num1EndIndex, int[] num2, int num2StartIndex, int num2EndIndex, int k) {
        // 获取需要遍历数组的长度(不能直接用num1.length获取，这样会导致数组的长度一致不会变动)
        int num1Length = num1EndIndex - num1StartIndex + 1;
        int num2Length = num2EndIndex - num2StartIndex + 1;
        // 总长度
        int totalLength = num1Length + num2Length;
        // 如果num1长度大于num2，则进行交换位置，保证num1数组为包含元素最少的数组
        if (num1Length > num2Length) {
            return getMedian(num2, num2StartIndex, num2EndIndex, num1, num1StartIndex, num1EndIndex, k);
        }
        // 如果最短的数组长度为0，则中位数获取剩下还有元素的数组
        if (num1Length == 0) {
            return num2[num2StartIndex + k - 1];
        }
        // 如果k为1(表示查询第一小的数值),表示数组第一个元素为中位数,则取两个数组中位数中最小的值
        if (k == 1) {
            return Math.min(num1[num1StartIndex], num2[num2StartIndex]);
        }
        // 计算两个数组起始下标的位置
        // 疑问：为什么比较的两个值需要添加num1StartIndex和num2StartIndex，原因是比较的是k/2的元素，但是每轮递归后
        // k需要减去上一轮排除的元素的个数
        // 减去1是因为前面计算的都是长度，但是offset对应的是数组下标
        // Math.min是为了防止数组长度比k/2长度小而导致数组越界的问题，使用它表示如果数组长度小于k/2时，则直接将数组坐标指到最后一个
        int num1CompareItem = num1StartIndex + Math.min(num1Length, k / 2) - 1;
        int num2CompareItem = num2StartIndex + Math.min(num2Length, k / 2) - 1;
        // 判断两个数组中位数的值
        if (num1[num1CompareItem] > num2[num2CompareItem]) {
            return getMedian(num1, num1StartIndex, num1EndIndex, num2, num2CompareItem + 1, num2EndIndex, k - (num2CompareItem - num2StartIndex+ 1));
        } else {
            return getMedian(num1, num1CompareItem + 1, num1EndIndex, num2, num2StartIndex, num2EndIndex, k - (num1CompareItem -num1StartIndex + 1));
        }
    }
    
```

<br>

### 4、执行结果

<br>

<center><img src="https://gitee.com/whose-white-moon/blog-image/raw/master/image-20211106171515812.png" /></center>

<br>

### 5、时间复杂度推导

<br>

&emsp;&emsp;根据上面的图解和代码，可以看到每循环一次，就可以排除对应数组的 k/2个元素。即如有n个元素，循环次数的就是呈现下面的规律：n,n/2,n/4,....n/2^k^,1（接下来操作元素的剩余个数），**用函数表示则为：f(k) => n /2^k^ = 1 ,即f(k) = log2^n^,根据大O记法，可以推断出时间复杂度为:O(log(n))，其中n表示的是元素个数即等于两个元素数组之和，故写成：O(log(m + n))。**

<br>

### 6、空间复杂度推导

<br>

&emsp;&emsp;上面的解法中我们**使用到了尾递归的方式，不需要重复推栈**，所以需要申请的空间的只是一些临时的变量，输入规模的大小并不会影响它们，因此空间复杂度为O(1)。
<br>

&emsp;**&emsp;尾递归和递归之间的联系和区别会在下一篇文章详细讲解，敬请期待！**

<br>

### 7、小结

<br>

&emsp;&emsp;**使用二分查询解决这个算法难点在于思想的归纳和边界值的确定**，这个能力并不是一蹴而就的，是需要积累，因此，如果第一遍看完文章不能完全理解是比较正常的，不要急于否定自己，多看几次，梳理出自己疑惑的点，再去进行实践，这样慢慢的就可以将它们转换成自己的东西。
<br>

## 算法思想在实际的应用
<br>

&emsp;&emsp;·1、暴力破解：这个思想最简单，也是在平常的业务中是被应用到最多的，但是并不是一个好的选择，如果使用暴力破解，一定要考虑问题的输入规模拓张的问题，否则效率将极低。
<br>

&emsp;&emsp;·2、二分查找法：主要运用在快速定位数组元素，提高效率。



## 写在最后

<br>

&emsp;&emsp;纸上得来终觉浅，绝知此事要躬行。看完文章理解了不代表你真的掌握 了，只要亲自动手编写出来，才算你转换成自己的东西了，赶紧打开开发工具，行动起来吧，实践中如遇到不懂的问题可以联系我！
