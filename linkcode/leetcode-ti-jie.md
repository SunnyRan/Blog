# LeetCode题解



1.Two Sum

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:  
Given nums = \[2, 7, 11, 15\], target = 9,  
Because nums\[0\] + nums\[1\] = 2 + 7 = 9,  
return \[0, 1\].

```text
    public static int[] twoSum(int[] nums, int target) {
        Hashtable<Integer, Integer> hs = new Hashtable<Integer, Integer>();
        int [] re = {0,0};
        for(int i = 0;i<nums.length;i++){
            Integer need = target - nums[i];
            Integer value = hs.get(need);
            if(value == null){hs.put(nums[i], i);}
            else {
                re[0] = value;
                re[1] = i;
                break;
            }
        }
        return re;
    }
```

2.Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Input: \(2 -&gt; 4 -&gt; 3\) + \(5 -&gt; 6 -&gt; 4\)  
Output: 7 -&gt; 0 -&gt; 8

```text
 public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode re = new ListNode(0);
        ListNode head = re;
        int a=0,b=0,c =0;
        while(l1!=null || l2!=null){
            a = l1==null?0:l1.val;
            if(l1 != null) l1 = l1.next;
            b = l2==null?0:l2.val;
            if(l2 != null) l2 = l2.next;
            re.next = new ListNode((a+b+c)%10);
            c = (a+b+c)/10;
            re= re.next;
        }
        if(c!=0)
        {
            re.next = new ListNode(1); 
        }
        return head.next;
    }
```

3.Longest Substring Without Repeating Characters  
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given “abcabcbb”, the answer is “abc”, which the length is 3.

Given “bbbbb”, the answer is “b”, with the length of 1.

Given “pwwkew”, the answer is “wke”, with the length of 3. Note that the answer must be a substring, “pwke” is a subsequence and not a substring.

Subscribe to see which companies asked this question

```text
    public int lengthOfLongestSubstring(String s) {
        Hashtable<Character,Integer> hs = new Hashtable<Character,Integer>();
        int star = 0,end = 0;
        int maxLength = 0;
        if(s.length() == 0) return 0;
            for(int i = star ;i < s.length();i++){              
                Integer n = hs.get(s.charAt(i));
                if(n != null && star<=n){
                    maxLength = end-star+1>maxLength?end-star+1:maxLength;
                    star = n+1;
                }else{
                    end = i;
                }
                hs.put(s.charAt(i), i); 
            }
            maxLength = end-star+1>maxLength?end-star+1:maxLength;
            return maxLength;
    }
```

