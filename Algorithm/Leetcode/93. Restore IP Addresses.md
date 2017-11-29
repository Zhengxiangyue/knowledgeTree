#### 93.Restore IP Addresses

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:
Given `"25525511135"`,

return `["255.255.11.135", "255.255.111.35"]`. (Order does not matter)



ip 地址一定是由四个部分组成，每进行一次 dfs 操作，当前分支的结果里面已经有零个一个或多个部分，需要再剩下的部分里面寻找其余部分，总共保证遍历完是四个部分。

以下几种情况得不到正确结果，可以直接回退：

1.remain 已经为0了，已经找到四个部分了，但是剩余字符串长度还不为0

2.剩余字符串已经为0了，但是还没找够4个部分（remain 大于0）



ip 地址的每个部分是0到255，每次 dfs 只用看剩余字符串的第一位，前两位，前三位是否满足一个部分