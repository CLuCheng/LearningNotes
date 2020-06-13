# Leetcode数据结构与算法

### [0081]按奇偶排序数组

给定一个非负整数数组 A，返回一个数组，在该数组中， A 的所有偶数元素之后跟着所有奇数元素。

你可以返回满足此条件的任何数组作为答案。 示例：

```
输入：[3,1,2,4]
输出：[2,4,3,1]
输出 [4,2,3,1]，[2,4,1,3] 和 [4,2,1,3] 也会被接受。
```

提示：

```
1 <= A.length <= 5000
0 <= A[i] <= 5000
```

方法一：排序

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        Integer[] B = new Integer[A.length];
        for (int t = 0; t < A.length; ++t)
            B[t] = A[t];

        Arrays.sort(B, (a, b) -> Integer.compare(a%2, b%2));

        for (int t = 0; t < A.length; ++t)
            A[t] = B[t];
        return A;

        /* Alternative:
        return Arrays.stream(A)
                     .boxed()
                     .sorted((a, b) -> Integer.compare(a%2, b%2))
                     .mapToInt(i -> i)
                     .toArray();
        */
    }
}
```

方法二：两次扫描

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        int[] ans = new int[A.length];
        int t = 0;

        for (int i = 0; i < A.length; ++i)
            if (A[i] % 2 == 0)
                ans[t++] = A[i];

        for (int i = 0; i < A.length; ++i)
            if (A[i] % 2 == 1)
                ans[t++] = A[i];

        return ans;
    }
}
```

方法三：原地算法

维护两个指针 i 和 j，循环保证每刻小于 i 的变量都是偶数（也就是 A[k] % 2 == 0 当 k < i），所有大于 j 的都是奇数。

所以， 4 种情况针对 (A[i] % 2, A[j] % 2)：

如果是 (0, 1)，那么 i++ 并且 j--。
如果是 (1, 0)，那么交换两个元素，然后继续。
如果是 (0, 0)，那么说明 i 位置是正确的，只能 i++。
如果是 (1, 1)，那么说明 j 位置是正确的，只能 j--。
通过这 4 种情况，循环不变量得以维护，并且 j-i 不断变小。最终就可以得到奇偶有序的数组。

```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        int i = 0, j = A.length - 1;
        while (i < j) {
            if (A[i] % 2 > A[j] % 2) {
                int tmp = A[i];
                A[i] = A[j];
                A[j] = tmp;
            }

            if (A[i] % 2 == 0) i++;
            if (A[j] % 2 == 1) j--;
        }

        return A;
    }
}
```



### [0082]重新排列数组

给你一个数组 nums ，数组中有 2n 个元素，按 [x1,x2,...,xn,y1,y2,...,yn] 的格式排列。

请你将数组按 [x1,y1,x2,y2,...,xn,yn] 格式重新排列，返回重排后的数组。

示例 1：

```
输入：nums = [2,5,1,3,4,7], n = 3
输出：[2,3,5,4,1,7] 
解释：由于 x1=2, x2=5, x3=1, y1=3, y2=4, y3=7 ，所以答案为 [2,3,5,4,1,7]
```


示例 2：

```
输入：nums = [1,2,3,4,4,3,2,1], n = 4
输出：[1,4,2,3,3,2,4,1]
```


示例 3：

```
输入：nums = [1,1,2,2], n = 2
输出：[1,2,1,2]
```

提示：

```
1 <= n <= 500
nums.length == 2n
1 <= nums[i] <= 10^3
```

方法：

```java
class Solution {
    public int[] shuffle(int[] nums, int n) {
        int[] ret = new int[2 * n];
        for(int i = 0; i < n; i++){
            ret[2 * i] = nums[i];
            ret[2 * i+1] = nums[i + n];
        }
        return ret;
    }
}
```



### [0083]拥有最多糖果的孩子

给你一个数组 candies 和一个整数 extraCandies ，其中 candies[i] 代表第 i 个孩子拥有的糖果数目。

对每一个孩子，检查是否存在一种方案，将额外的 extraCandies 个糖果分配给孩子们之后，此孩子有 最多 的糖果。注意，允许有多个孩子同时拥有 最多 的糖果数目。

 

示例 1：

```
输入：candies = [2,3,5,1,3], extraCandies = 3
输出：[true,true,true,false,true] 
```


解释：

```
孩子 1 有 2 个糖果，如果他得到所有额外的糖果（3个），那么他总共有 5 个糖果，他将成为拥有最多糖果的孩子。
孩子 2 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
孩子 3 有 5 个糖果，他已经是拥有最多糖果的孩子。
孩子 4 有 1 个糖果，即使他得到所有额外的糖果，他也只有 4 个糖果，无法成为拥有糖果最多的孩子。
孩子 5 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
```


示例 2：

```
输入：candies = [4,2,1,1,2], extraCandies = 1
输出：[true,false,false,false,false] 
解释：只有 1 个额外糖果，所以不管额外糖果给谁，只有孩子 1 可以成为拥有糖果最多的孩子。
```


示例 3：

```
输入：candies = [12,1,12], extraCandies = 10
输出：[true,false,true]
```

方法一：

```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        List<Boolean> list = new ArrayList<Boolean>();
        int maxValue = 0;
        for(int i = 0; i < candies.length ; i++ )
            if(candies[i] > maxValue) maxValue = candies[i];
        for(int i = 0; i < candies.length ; i++ )
            list.add(candies[i] +extraCandies >= maxValue ? true : false);
        return list;
    }
}
```



### [0084]在既定时间做作业的学生人数

给你两个整数数组 startTime（开始时间）和 endTime（结束时间），并指定一个整数 queryTime 作为查询时间。

已知，第 i 名学生在 startTime[i] 时开始写作业并于 endTime[i] 时完成作业。

请返回在查询时间 queryTime 时正在做作业的学生人数。形式上，返回能够使 queryTime 处于区间 [startTime[i], endTime[i]]（含）的学生人数。

 

示例 1：

```
输入：startTime = [1,2,3], endTime = [3,2,7], queryTime = 4
输出：1
解释：一共有 3 名学生。
第一名学生在时间 1 开始写作业，并于时间 3 完成作业，在时间 4 没有处于做作业的状态。
第二名学生在时间 2 开始写作业，并于时间 2 完成作业，在时间 4 没有处于做作业的状态。
第二名学生在时间 3 开始写作业，预计于时间 7 完成作业，这是是唯一一名在时间 4 时正在做作业的学生。
```


示例 2：

```
输入：startTime = [4], endTime = [4], queryTime = 4
输出：1
解释：在查询时间只有一名学生在做作业。
```


示例 3：

```
输入：startTime = [4], endTime = [4], queryTime = 5
输出：0
```


示例 4：

```
输入：startTime = [1,1,1,1], endTime = [1,3,2,4], queryTime = 7
输出：0
```


示例 5：

```
输入：startTime = [9,8,7,6,5,4,3,2,1], endTime = [10,10,10,10,10,10,10,10,10], queryTime = 5
输出：5
```

提示：

```
startTime.length == endTime.length
1 <= startTime.length <= 100
1 <= startTime[i] <= endTime[i] <= 1000
1 <= queryTime <= 1000
```

方法一：

```java
class Solution {
    public int busyStudent(int[] startTime, int[] endTime, int queryTime) {
        int ret = 0;
        for(int i = 0; i < startTime.length ; i++ )
            if(startTime[i] <= queryTime && endTime[i] >= queryTime)
                ret++;
        return ret;
    }
}
```

### [0085]拿硬币

桌上有 n 堆力扣币，每堆的数量保存在数组 coins 中。我们每次可以选择任意一堆，拿走其中的一枚或者两枚，求拿完所有力扣币的最少次数。

示例 1：

```
输入：[4,2,1]

输出：4

解释：第一堆力扣币最少需要拿 2 次，第二堆最少需要拿 1 次，第三堆最少需要拿 1 次，总共 4 次即可拿完。
```

示例 2：

```
输入：[2,3,10]

输出：8
```

限制：

```
1 <= n <= 4
1 <= coins[i] <= 10
```

方法一：

```java
class Solution {
    public int minCount(int[] coins) {
        int ret = 0;
        for(int i = 0; i < coins.length ; i++ ){
            ret += coins[i] % 2 == 0 ? coins[i] /2 : coins[i] /2 + 1 ;
        }
        return ret;
    }
}
```

### [0086]旅行终点站

给你一份旅游线路图，该线路图中的旅行线路用数组 paths 表示，其中 paths[i] = [cityAi, cityBi] 表示该线路将会从 cityAi 直接前往 cityBi 。请你找出这次旅行的终点站，即没有任何可以通往其他城市的线路的城市。

题目数据保证线路图会形成一条不存在循环的线路，因此只会有一个旅行终点站。

示例 1：

```
输入：paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
输出："Sao Paulo" 
解释：从 "London" 出发，最后抵达终点站 "Sao Paulo" 。本次旅行的路线是 "London" -> "New York" -> "Lima" -> "Sao Paulo" 。
```


示例 2：

```
输入：paths = [["B","C"],["D","B"],["C","A"]]
输出："A"
解释：所有可能的线路是：
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
显然，旅行终点站是 "A" 。
```


示例 3：

```
输入：paths = [["A","Z"]]
输出："Z"
```

方法一：

```java
class Solution {
    public String destCity(List<List<String>> paths) {
		Map<String, Integer> map = new HashMap<String, Integer>();
		for (List<String> list : paths)
			map.put(list.get(0), 1);
		for (List<String> list : paths)
			if (map.get(list.get(1)) == null)
				return list.get(1);
		return null;
    }
}
```

### [0087]通过翻转子数组使两个数组相等

给你两个长度相同的整数数组 target 和 arr 。

每一步中，你可以选择 arr 的任意 非空子数组 并将它翻转。你可以执行此过程任意次。

如果你能让 arr 变得与 target 相同，返回 True；否则，返回 False 。

示例 1：

```
输入：target = [1,2,3,4], arr = [2,4,1,3]
输出：true
解释：你可以按照如下步骤使 arr 变成 target：
1- 翻转子数组 [2,4,1] ，arr 变成 [1,4,2,3]
2- 翻转子数组 [4,2] ，arr 变成 [1,2,4,3]
3- 翻转子数组 [4,3] ，arr 变成 [1,2,3,4]
上述方法并不是唯一的，还存在多种将 arr 变成 target 的方法。
```


示例 2：

```
输入：target = [7], arr = [7]
输出：true
解释：arr 不需要做任何翻转已经与 target 相等。
```


示例 3：

```
输入：target = [1,12], arr = [12,1]
输出：true
```


示例 4：

```
输入：target = [3,7,9], arr = [3,7,11]
输出：false
解释：arr 没有数字 9 ，所以无论如何也无法变成 target 。
```


示例 5：

```
输入：target = [1,1,1,1,1], arr = [1,1,1,1,1]
输出：true
```

提示：

```
target.length == arr.length
1 <= target.length <= 1000
1 <= target[i] <= 1000
1 <= arr[i] <= 1000
```

方法一：(实际上是比较两个数组是否相等，参考冒泡排序)

```java
class Solution {
    public boolean canBeEqual(int[] target, int[] arr) {
       if (target.length != arr.length) {
            return false;
        }
        int[] tmp1 = new int[1001];
        int[] tmp2 = new int[1001];
        int len = target.length;
        for(int i = 0; i < len ; i++)
        {   tmp1[target[i]]++;
            tmp2[arr[i]]++;
        }
        for(int i = 0; i < 1001 ; i++)
        {
            if(tmp1[i] != tmp2[i]) return false;
        }
        return true;
    }
}
```



### [0088]最大数值

编写一个方法，找出两个数字`a`和`b`中最大的那一个。不得使用if-else或其他比较运算符。

**示例：**

```
输入： a = 1, b = 2
输出： 2
```



方法一：

```java
class Solution {
    //公式：max(a,b) = (a + b + |a - b|) / 2
    int maximum(int a, int b) {
        long sum = (long)a + (long)b;
        long diff = (long)a - (long)b;
        long abs_diff = (diff ^ (diff >> 63)) - (diff >> 63);
        return  (int)((sum + abs_diff) / 2);
    }
}
```

绝对值的位运算，为了回避abs，利用位运算实现绝对值功能。

以int8_t为例：分析运算：(var ^ (var >> 7)) - (var >> 7)

var >= 0: var >> 7 => 0x00，即：(var ^ 0x00) - 0x00，异或结果为var

var < 0: var >> 7 => 0xFF，即：(var ^ 0xFF) - 0xFF，var ^ 0xFF是在对var的全部位取反，-0xFF <=> +1, 对signed int取反加一就是取其相反数。

举个栗子🌰：var = -3 <=> 0xFD，(var ^ 0xFF) - 0xFF= 0x02 - 0xff= 0x03

基于上述分析：

类型	绝对值位运算
int8_t	(var ^ (var >> 7)) - (var >> 7)
int16_t	(var ^ (var >> 15)) - (var >> 15)
int32_t	(var ^ (var >> 31)) - (var >> 31)
int64_t	(var ^ (var >> 63)) - (var >> 63)

代码中`(diff^ (diff >> 63)) - (diff>> 63)`就是在求取`long (int64_t)`的绝对值。

方法二：

```java
class Solution {
    public int maximum(int a, int b) {
        // 先考虑没有溢出时的情况，计算 b - a 的最高位，依照题目所给提示 k = 1 时 a > b，即 b - a 为负
        int k = (b - a) >>> 31;
        // 再考虑 a b 异号的情况，此时无脑选是正号的数字
        int aSign = a >>> 31, bSign = b >>> 31;
        // diff = 0 时同号，diff = 1 时异号
        int diff = aSign ^ bSign;
        // 在异号，即 diff = 1 时，使之前算出的 k 无效，只考虑两个数字的正负关系
        k = k & (diff ^ 1) | bSign & diff;
        return a * k + b * (k ^ 1);
    }

}
```

### [0089]数组拆分 I

给定长度为 2n 的数组, 你的任务是将这些数分成 n 对, 例如 (a1, b1), (a2, b2), ..., (an, bn) ，使得从1 到 n 的 min(ai, bi) 总和最大。

示例 1:

```
输入: [1,4,3,2]

输出: 4
解释: n 等于 2, 最大总和为 4 = min(1, 2) + min(3, 4).
```

提示:

```
n 是正整数,范围在 [1, 10000].
数组中的元素范围在 [-10000, 10000].
```

方法一：排序

```java
class Solution {
    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);
        int sum = 0;
        for (int i = 0; i < nums.length; i += 2) {
            sum += nums[i];
        }
        return sum;
    }
}
```

方法二：使用额外的空间

```java
public class Solution {
    public int arrayPairSum(int[] nums) {
        int[] arr = new int[20001];
        int lim = 10000;
        for (int num: nums)
            arr[num + lim]++;
        int d = 0, sum = 0;
        for (int i = -10000; i <= 10000; i++) {
            //d：如果当前集合中有剩余元素将被再次考虑，则此标志设置为 1。在从下一组中选择元素时，会考虑已考虑的相同额外元素。
            sum += (arr[i + lim] + 1 - d) / 2 * i;
            d = (2 + arr[i + lim] - d) % 2;
        }
        return sum;
    }
} 
```

https://leetcode-cn.com/problems/array-partition-i/solution/shu-zu-chai-fen-i-by-leetcode/

### [0090]独一无二的出现次数

给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数。

如果每个数的出现次数都是独一无二的，就返回 true；否则返回 false。

 示例 1：

```
输入：arr = [1,2,2,1,1,3]
输出：true
解释：在该数组中，1 出现了 3 次，2 出现了 2 次，3 只出现了 1 次。没有两个数的出现次数相同。
```

示例 2：

```
输入：arr = [1,2]
输出：false
```

示例 3：

```
输入：arr = [-3,0,1,-3,1,1,1,-3,10,0]
输出：true
```

提示：

- `1 <= arr.length <= 1000`
- `-1000 <= arr[i] <= 1000`



方法一：

```java
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        HashSet<Integer> set = new HashSet<>();  
        int[] ret = new int[2001];
        int base = 1000;

        for (int i = 0; i < arr.length; i ++)
            ret[arr[i] + base]++;
                
        for (int i = 0; i < ret.length; i ++)
        {
            if(ret[i]!=0){
                if(set.contains(ret[i])) return false;
                set.add(ret[i]);
            }
        }
        return true;
        
    }
}
```

### [0091]递增顺序查找树

给你一个树，请你 按中序遍历 重新排列树，使树中最左边的结点现在是树的根，并且每个结点没有左子结点，只有一个右子结点。

示例 ：

    输入：[5,3,6,2,4,null,8,1,null,null,null,7,9]
    		5
          / \
        3    6
       / \    \
      2   4    8
     /        / \ 
    1        7   9
    
    输出：[1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
    
     1
      \
       2
        \
         3
          \
           4
            \
             5
              \
               6
                \
                 7
                  \
                   8
                    \
                     9  

提示：

给定树中的结点数介于 1 和 100 之间。
每个结点都有一个从 0 到 1000 范围内的唯一整数值。

方法一：中序遍历 + 构造新的树

我们在树上进行中序遍历，就可以从小到大得到树上的节点。我们把这些节点的对应的值存放在数组中，它们已经有序。接着我们直接根据数组构件题目要求的树即可。

```java
class Solution {    
    public TreeNode increasingBST(TreeNode root) {
        List<Integer> vals = new ArrayList();
        inorder(root, vals);
        TreeNode ans = new TreeNode(0), cur = ans;
        for (int v: vals) {
            cur.right = new TreeNode(v);
            cur = cur.right;
        }
        return ans.right;
    }

    public void inorder(TreeNode node, List<Integer> vals) {
        if (node == null) return;
        inorder(node.left, vals);
        vals.add(node.val);
        inorder(node.right, vals);
    }
}
```

复杂度分析

时间复杂度：O(N)O(N)，其中 NN 是树上的节点个数。

空间复杂度：O(N)O(N)。

方法二：中序遍历 + 更改树的连接方式

和方法一类似，我们在树上进行中序遍历，但会将树中的节点之间重新连接而不使用额外的空间。具体地，当我们遍历到一个节点时，把它的左孩子设为空，并将其本身作为上一个遍历到的节点的右孩子。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    TreeNode cur;
    public TreeNode increasingBST(TreeNode root) {
        TreeNode ans = new TreeNode(0);
        cur = ans;
        inorder(root);
        return ans.right;
    }

    public void inorder(TreeNode node) {
        if (node == null) return;
        inorder(node.left);
        node.left = null;
        cur.right = node;
        cur = node;
        inorder(node.right);
    }
}
```

复杂度分析

时间复杂度：O(N)O(N)，其中 NN 是树上的节点个数。

空间复杂度：O(H)O(H)，其中 HH 是数的高度。

### [0092]找出给定方程的正整数解

给出一个函数  f(x, y) 和一个目标结果 z，请你计算方程 f(x,y) == z 所有可能的正整数 数对 x 和 y。

给定函数是严格单调的，也就是说：

```
f(x, y) < f(x + 1, y)
f(x, y) < f(x, y + 1)
```

函数接口定义如下：

```
interface CustomFunction {
public:
  // Returns positive integer f(x, y) for any given positive integer x and y.
  int f(int x, int y);
};
```

如果你想自定义测试，你可以输入整数 function_id 和一个目标结果 z 作为输入，其中 function_id 表示一个隐藏函数列表中的一个函数编号，题目只会告诉你列表中的 2 个函数。  

你可以将满足条件的 结果数对 按任意顺序返回。

示例 1：

```
输入：function_id = 1, z = 5
输出：[[1,4],[2,3],[3,2],[4,1]]
解释：function_id = 1 表示 f(x, y) = x + y
```

示例 2：

```
输入：function_id = 2, z = 5
输出：[[1,5],[5,1]]
解释：function_id = 2 表示 f(x, y) = x * y
```


提示：

```
1 <= function_id <= 9
1 <= z <= 100
题目保证 f(x, y) == z 的解处于 1 <= x, y <= 1000 的范围内。
在 1 <= x, y <= 1000 的前提下，题目保证 f(x, y) 是一个 32 位有符号整数。
```

方法一：暴力法

```java
class Solution {
    public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 1; i <= 1000; i++) {
            if (customfunction.f(i,1) > z) {
                break;
            }
            for (int j = 1; j <= 1000; j++) {
                if (customfunction.f(i,j) == z) {
                    List<Integer> list = new ArrayList<>();
                    list.add(i);
                    list.add(j);
                    res.add(list);
                    break;
                } else if(customfunction.f(i,j) > z){
                    break;
                }
            }
        }
        return res;  
    }
}
```

时间复杂度：O(n^2)

方法二：二分查找

```java
class Solution {
    public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
        List<List<Integer>> res = new ArrayList<>();
        for (int i = 1; i <= 1000; i++) {
            if (customfunction.f(i,1) > z) {
                break;
            }
            int left = 1;
            int right = 1000;
            while (left <= right) {
                int mid = (right + left) / 2;
                int temp = customfunction.f(i,mid);
                if (temp == z) {
                    List<Integer> list = new ArrayList<>();
                    list.add(i);
                    list.add(mid);
                    res.add(list);
                    break;
                } else if (temp > z) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
        }
        return res;
    }
}
```

时间复杂度：O(nlogn)

方法三：双指针法 (给定函数是严格单调的)

```java
class Solution {
    public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
        List<List<Integer>> res = new ArrayList<>();
        int left = 1;
        int right = 1000;
        while (left <= 1000 && right >= 1) {
            int temp = customfunction.f(left,right);
            if (temp == z) {
                List<Integer> list = new ArrayList<>();
                list.add(left);
                list.add(right);
                res.add(list);
                left++;
            } else if (temp > z) {
                right--;
            } else {
                left++;
            }
        }
        return res;
    }
}

```

时间复杂度：O(n)

### [0093]只出现一次的数字

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具**有线性时间复杂度**。 你可以**不使用额外空间**来实现吗？

```
示例 1:

输入: [2,2,1]
输出: 1

示例 2:

输入: [4,1,2,1,2]
输出: 4
```

方法一：

异或运算有以下三个性质。

- 任何数和 0 做异或运算，结果仍然是原来的数。
- 任何数和其自身做异或运算，结果是 0。
- 异或运算满足交换律和结合律。

```java
class Solution {
    public int singleNumber(int[] nums) {
        int single = 0;
        for (int num : nums) {
            single ^= num;
        }
        return single;
    }
}
```

- 时间复杂度：O(n)，其中 n 是数组长度。只需要对数组遍历一次。
- 空间复杂度：O(1)。

### [0094]特殊等价字符串组

你将得到一个字符串数组 A。

如果经过任意次数的移动，S == T，那么两个字符串 S 和 T 是特殊等价的。

一次移动包括选择两个索引 i 和 j，且 i ％ 2 == j ％ 2，交换 S[j] 和 S [i]。

现在规定，A 中的特殊等价字符串组是 A 的非空子集 S，这样不在 S 中的任何字符串与 S 中的任何字符串都不是特殊等价的。

返回 A 中特殊等价字符串组的数量。 

```
示例 1：
输入：["a","b","c","a","c","c"]
输出：3
解释：3 组 ["a","a"]，["b"]，["c","c","c"]

示例 2：
输入：["aa","bb","ab","ba"]
输出：4
解释：4 组 ["aa"]，["bb"]，["ab"]，["ba"]

示例 3：
输入：["abc","acb","bac","bca","cab","cba"]
输出：3
解释：3 组 ["abc","cba"]，["acb","bca"]，["bac","cab"]

示例 4：
输入：["abcd","cdab","adcb","cbad"]
输出：1
解释：1 组 ["abcd","cdab","adcb","cbad"]


提示：
1 <= A.length <= 1000
1 <= A[i].length <= 20
所有 A[i] 都具有相同的长度。
所有 A[i] 都只由小写字母组成。
```



https://leetcode-cn.com/problems/groups-of-special-equivalent-strings/solution/te-shu-deng-jie-zi-fu-chuan-zu-by-leetcode/

方法一：

```java
class Solution {
    public int numSpecialEquivGroups(String[] A) {
        Set<String> seen = new HashSet();
        for (String S: A) {
            int[] count = new int[52];
            for (int i = 0; i < S.length(); ++i)
                count[S.charAt(i) - 'a' + 26 * (i % 2)]++;
            seen.add(Arrays.toString(count));
        }
        return seen.size();
    }
}
```

### [0095]数字的补数

给定一个正整数，输出它的补数。补数是对该数的二进制表示取反。

 示例 1:

```
输入: 5
输出: 2
解释: 5 的二进制表示为 101（没有前导零位），其补数为 010。所以你需要输出 2 。
```

示例 2:

```
输入: 1
输出: 0
解释: 1 的二进制表示为 1（没有前导零位），其补数为 0。所以你需要输出 0 。
```


注意:

```
给定的整数保证在 32 位带符号整数的范围内。
你可以假定二进制数不包含前导零位。
本题与 1009 https://leetcode-cn.com/problems/complement-of-base-10-integer/ 相同
```

方法一：

```java
class Solution {
    public int findComplement(int num) {
        int temp = num, c = 0;
        while(temp > 0){
            temp >>= 1;
            c =  (c << 1) + 1;
        }
        return num ^ c;       
    }
}
```

### [0096]最小差值 I

给你一个整数数组 A，对于每个整数 A[i]，我们可以选择处于区间 [-K, K] 中的任意数 x ，将 x 与 A[i] 相加，结果存入 A[i] 。

在此过程之后，我们得到一些数组 B。

返回 B 的最大值和 B 的最小值之间可能存在的最小差值。

 ```
示例 1：
输入：A = [1], K = 0
输出：0
解释：B = [1]

示例 2：
输入：A = [0,10], K = 2
输出：6
解释：B = [2,8]

示例 3：
输入：A = [1,3,6], K = 3
输出：0
解释：B = [3,3,3] 或 B = [4,4,4]
 ```


提示：

```
1 <= A.length <= 10000
0 <= A[i] <= 10000
0 <= K <= 10000
```

方法一：

```java
class Solution {
    public int smallestRangeI(int[] A, int K) {
        int min = A[0], max = A[0];
        for (int x: A) {
            min = Math.min(min, x);
            max = Math.max(max, x);
        }
        return Math.max(0, max - min - 2*K);
    }
}
```

