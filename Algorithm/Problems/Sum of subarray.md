Given an integer array ‘arr[]’ of size n, find sum of all sub-arrays of given array.

[1,2,3,4,5]，对于数组中的每个元素，可以计算出有多少个 subarray 包含它，即每个元素出现过几次。因为包含一个元素的 subarray 的开始元素一定在这个元素之前（inclusive），结束元素一定在这个元素之后（inclusive），以元素3为例，包含他的 subarray 可以以1，2，3开始，可以以3，4，5结尾，每一个开始和结尾都可以构成不同的 subarray，所以包含3的 subarray 就有3x3 = 9个，所以对于n 个元素集合的第 i (1,2,...n)个元素，有 i * (n-i+1)个subarray 出现过