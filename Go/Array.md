# <font color=#E9967A>1,Subarray Sum Equals K</font>--[leetcode560](https://leetcode-cn.com/problems/subarray-sum-equals-k/)
## traverse
```go {.line-numbers}
func subarraySum(nums []int, k int) int {
    count := 0
    for end := 0;end<len(nums);end++{
        sum := 0 
        for start:= end ; start>=0 ;start --{
            sum += nums[start]
            if sum == k{
                count++
            }
        }
    }
    return count
}
```
## prefix sum + hash map
```go {.line-numbers}
func subarraySum(nums []int, k int) int {
	hashmap := map[int]int{0:1}
    //  hashmap := make(map[int]int)
    //	hashmap[0] = 1
	count,pre := 0,0
	for i := 0; i < len(nums); i++ {
        pre += nums[i]	
		if _,ok := hashmap[pre - k];ok{
			count += hashmap[pre - k]
		}
        hashmap[pre]++
	}
	return count
}
```