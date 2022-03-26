# Longest Palindromic Substring -- [leetcode 5](https://leetcode-cn.com/problems/longest-palindromic-substring/)
### dynamic program
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