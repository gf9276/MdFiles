# 1. 动态规划

## 1.1. 删除一次得到子数组最大和

一开始没有想到 dp，浪费了点时间，其实很好想，找到转移方程就行了

删除上一个？还是继承以前的？

```java
class Solution {
    public int maximumSum(int[] arr) {
        // 用dp吧
        int[] dp1 = new int[arr.length];// 以i为结尾的子数组，删除吊一个数字，最大的和是？
        int[] dp2 = new int[arr.length];// 以i为结尾的子数组，最大的和是？

        dp1[0] = arr[0];
        dp2[0] = arr[0];

        int output = arr[0];

        // 那问题就转成了，使用以前的dp还是删除上一个数字
        for (int i = 1; i < arr.length; i++) {
            dp2[i] = Math.max(dp2[i - 1] + arr[i], arr[i]); // 要么从头开始，要么继承过去
            dp1[i] = Math.max(dp1[i - 1], dp2[i - 1] - arr[i - 1]) + arr[i]; // 仍以前的，直接继承dp1[i-1]即可，仍上一个，那就用dp2[i-1]
            output = Math.max(output, dp1[i]);
        }

        return output;
    }
}
```

## 1.2. 最长有效括号

栈和 dp 的双应用

思路是这样的

以当前括号为结尾的长度，如果当前括号是 左括号，那就是 0，如果是右括号，有效长度为 当前括号到和他匹配的左括号的长度 + dp[他匹配的左括号-1]

非常 easy 啊

```java
class Solution {
    public int longestValidParentheses(String s) {
        // 是这样的
        // 栈里不可能有 ) 会被秒
        Deque<Character> stack = new LinkedList<>();
        Deque<Integer> stackIdx = new LinkedList<>();

        int[] dp = new int[s.length() + 1];
        dp[0] = 0; // 以 i 为结尾的最大长度

        int ans = 0;

        for (int i = 0; i < s.length(); i++)
        {
            if (stack.isEmpty() && s.charAt(i) == ')')
            {
                // 那你完了，全断了
                continue;
            }
            if(!stack.isEmpty() && stack.peekFirst() == '(' && s.charAt(i) == ')')
            {
                // 对上了
                stack.pollFirst();
                int lastIdx = stackIdx.pollFirst();
                dp[i + 1] = dp[lastIdx] + i - lastIdx + 1;
            }
            else
            {
                // 加呗
                stack.addFirst('(');
                stackIdx.addFirst(i); // 这个位置刷新一下
            }
        }

        for(int i : dp)
        {
            ans = Math.max(ans, i);
        }
        return ans;
    }
}
```
