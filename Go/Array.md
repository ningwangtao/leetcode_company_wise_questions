# <font color=#E9967A>1,Subarray Sum Equals K</font>--[leetcode560](https://leetcode-cn.com/problems/subarray-sum-equals-k/)
## traverse
```go
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
```go

```