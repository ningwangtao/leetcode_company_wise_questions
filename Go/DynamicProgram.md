# 1,Longest Palindromic Substring -- [leetcode 5](https://leetcode-cn.com/problems/longest-palindromic-substring/)
## dynamic program
```go{.line-numbers}
func longestPalindrome(s string) string {
	length := len(s)
	dp := make([][]bool, length)
	for i := range dp {
		dp[i] = make([]bool, length)
		dp[i][i] = true
	}
	res := s[0:1]
	for end := 1; end < length; end++ {
		for start := 0; start < end; start++ {
			if s[start] != s[end] {
				continue
			} else if end == (start + 1) {
				dp[start][end] = true
			} else {
				dp[start][end] = dp[start+1][end-1]
			}
			if dp[start][end] && end-start+1 > len(res){
				res = s[start : end+1]
			}
		}
	}
	return res
}
```
# 2,Longest Common Subsequence -- [leetcode1143](https://leetcode-cn.com/problems/longest-common-subsequence/)
## dynamic program
```go{.line-numbers}
func longestCommonSubsequence(text1 string, text2 string) int {
	len1, len2 := len(text1), len(text2)
	dp := make([][]int, len1+1)
	for i := range dp {
		dp[i] = make([]int, len2+1)
	}
	for i := 0; i < len1; i++ {
		for j := 0; j < len2; j++ {
			if text1[i] == text2[j] {
				dp[i+1][j+1] = dp[i][j] + 1
			} else {
				dp[i+1][j+1] = getmax(dp[i+1][j], dp[i][j+1])
			}
		}
	}
	return dp[len1][len2]
}

func getmax(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```
# 3,Longest Increasing Subsequence -- [leetcode300](https://leetcode-cn.com/problems/longest-increasing-subsequence/)
## dynamic program
```go{.line-numbers}
func lengthOfLIS(nums []int) int {
    length := len(nums)
    dp := make([]int , length)
    max := 1
    for end:=0;end<length;end++{
        dp[end] = 1
        for start:=0;start<end;start++{
            if nums[end] > nums[start]{
                dp[end] = getmax(dp[end],dp[start] + 1)
            }
        }
        max = getmax(max,dp[end])
    }
    return max
}
func getmax(a,b int)int{
    if a>b{
        return a
    }
    return b
}
```
# 4，Triangle -- [leetcode120](https://leetcode-cn.com/problems/triangle/)
### dynamic program
```go{.line-numbers}
func minimumTotal(triangle [][]int) int {
	height := len(triangle)
	if height == 1 {
		return triangle[0][0]
	}
	for i := 1; i < height; i++ {
		triangle[i][0] += triangle[i-1][0]
		for j := 1; j < i; j++ {
			if triangle[i-1][j-1] > triangle[i-1][j] {
				triangle[i][j] += triangle[i-1][j]
			} else {
				triangle[i][j] += triangle[i-1][j-1]
			}
			triangle[i][j] += triangle[i-1][j]
		}
		triangle[i][i] += triangle[i-1][i-1]
	}
	min := 1 << 20
	for i := 0; i < height; i++ {
		if triangle[height-1][i] < min {
			min = triangle[height-1][i]
		}
	}
	return min
}
```
# 5，Maximum Product Subarray -- [leetcode152](https://leetcode-cn.com/problems/maximum-product-subarray/)

### dynamic program
```go{.line-numbers}
func maxProduct(nums []int) int {
	length := len(nums)
	fmax, fmin ,max:= nums[0], nums[0],nums[0]
	for i := 1; i < length; i++ {
		mx,mn:= fmax,fmin
		fmax = getmax(mx*nums[i],getmax(nums[i],mn*nums[i]))
		fmin = getmin(mn*nums[i],getmin(nums[i],mx*nums[i]))
		max = getmax(fmax,max)
	}
	return max
}
func getmax(a, b int) int {
	if a > b {
		return a
	}
	return b
}
func getmin(a, b int) int {
	if a > b {
		return b
	}
	return a
}
```
# 6,Minimum Path Sum -- [leetcode64](https://leetcode-cn.com/problems/minimum-path-sum/)

```go{.line-numbers}
func minPathSum(grid [][]int) int {
	row := len(grid)
	column := len(grid[0])

	for i := 0; i < row; i++ {
		for j := 0; j < column; j++ {
			if i == 0 && j == 0 {
				continue
			} else if i == 0 {
				grid[i][j] += grid[i][j-1]
			} else if j == 0 {
				grid[i][j] += grid[i-1][j]
			} else {
				grid[i][j] += getmin(grid[i][j-1], grid[i-1][j])
			}
		}
	}
	return grid[row-1][column-1]
}
func getmin(a, b int) int {
	if a > b {
		return b
	}
	return a
}
```

# 7,Unique Paths -- [leetcode62](https://leetcode-cn.com/problems/unique-paths/submissions/)

```go{.line-numbers}
func uniquePaths(m int, n int) int {
	if m == 1 || n == 1 {
		return 1
	}
	dp := make([][]int, m)
	for row := 0; row < m; row++ {
		dp[row] = make([]int, n)
		for column := 0; column < n; column++ {
			if row == 0 && column == 0 {
				continue
			} else if row == 0 {
				dp[row][column] = 1
			} else if column == 0 {
				dp[row][column] = 1
			} else {
				dp[row][column] = dp[row][column-1] + dp[row-1][column]
			}
		}
	}
	return dp[m-1][n-1]
}
```
# 8， Word Break -- [leetcode139](https://leetcode-cn.com/problems/word-break/)
###
```go{.line-numbers}
func wordBreak(s string, wordDict []string) bool {
	length := len(s)
	word := make(map[string]bool)
	for _, val := range wordDict {
		word[val] = true
	}
	dp := make([]bool, length+1)
	dp[0] = true
	for i := 1; i <= length; i++ {
		for j := 0; j < i; j++ {
			if dp[j] && word[s[j:i]] {
				dp[i] = true
				break
			}
		}
	}
	return dp[length]
}
```