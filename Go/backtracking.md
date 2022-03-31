# 1,Combination Sum -- [leetcode39](https://leetcode-cn.com/problems/combination-sum/)
```go {.line-numbers}
func combinationSum(candidates []int, target int) [][]int {

    res := make([][]int,0)
    ress := make([]int,0)
    sum := 0
    var backtracking func(int)
    backtracking = func(start int){
        if sum >= target{
            if sum == target{
                res = append(res,append([]int{},ress...))
            }
            return
        }
        for i:=start;i<len(candidates);i++{
            sum += candidates[i]
            ress = append(ress,candidates[i])
            backtracking(i)
            sum -= candidates[i]
            ress = ress[:len(ress)-1]
        }
    }
    backtracking(0)
    return res
}
```

# 2，Permutations -- [leetcode46](https://leetcode-cn.com/problems/permutations/)
```go {.line-numbers}
func permute(nums []int) [][]int {
	res := make([][]int, 0)

	var DFS func([]int, int)
	DFS = func(nums []int, index int) {
		if index == len(nums) {
			res = append(res, append([]int{},nums...))
			return
		}
		for i := index; i < len(nums); i++ {
			nums[index], nums[i] = nums[i], nums[index]
			DFS(nums, index+1)
			nums[index], nums[i] = nums[i], nums[index]
		}
	}
	DFS(nums, 0)
	return res
}

//solution 2
func permute(nums []int) [][]int {
	res := make([][]int, 0)
	flag := make([]bool, len(nums))

	var DFS func(int, []int)
	DFS = func(index int, ress []int) {
		if index == len(nums) {
			res = append(res, append([]int{}, ress...))
			return
		}
		for i := 0; i < len(nums); i++ {
			if flag[i] {
				continue
			}
			ress = append(ress, nums[i])
			flag[i] = true
			DFS(index+1, ress)
			flag[i] = false
			ress = ress[:len(ress)-1]
		}
	}
	DFS(0, []int{})
	return res
}
```
# 3,Permutations II -- [leetcode47](https://leetcode-cn.com/problems/permutations-ii/)
```go {.line-numbers}
func permuteUnique(nums []int) [][]int {

	sort.Ints(nums)
	res := make([][]int, 0)
	flag := make([]bool, len(nums))

	var DFS func(int, []int)
	DFS = func(index int, ress []int) {
		if index == len(nums) {
			res = append(res, append([]int{}, ress...))
			return
		}
		for i := 0; i < len(nums); i++ {
            if flag[i] {
                continue
            }
			if i > 0 && nums[i] == nums[i-1] && flag[i-1] == false {
				continue
			}
			ress = append(ress, nums[i])
			flag[i] = true
			DFS(index+1, ress)
			flag[i] = false
			ress = ress[:len(ress)-1]
		}
	}
	DFS(0, []int{})
	return res
}
```
# 4,Path sum II -- [leetcode113](https://leetcode-cn.com/problems/path-sum-ii/)

```go {.line-numbers}
func pathSum(root *TreeNode, targetSum int) [][]int {
	    if root == nil {
	        return nil
	    }
	    res := make([][]int, 0)
	    ress := make([]int, 0)
	    var DFS func(*TreeNode, int)
	    DFS = func(tn *TreeNode, subTarget int) {
	        if tn == nil {
	            return
	        }
	        ress = append(ress, tn.Val)
	        defer func() {
	            ress = ress[:len(ress)-1]
	        }()
	        if tn.Left == nil && tn.Right == nil && subTarget-tn.Val == 0 {
	            res = append(res, append([]int{}, ress...))
	            return
	        }
	        DFS(tn.Left, subTarget-tn.Val)
	        DFS(tn.Right, subTarget-tn.Val)
	    }
	    DFS(root, targetSum)
	    return res
	}
```
# 5,Path sum III -- [leetcode437](https://leetcode-cn.com/problems/path-sum-ii/)
```go {.line-numbers}
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
//prefix sum + preorder traversal + backtracking
func pathSum(root *TreeNode, targetSum int) int {
	m := make(map[int]int)
	m[0] = 1
	res := 0
	var DFS func(*TreeNode,int)
	DFS = func(tn *TreeNode,cur int) {
		if tn == nil {
			return
		}
		cur += tn.Val
		if _,ok := m[cur - targetSum];ok{
			res += m[cur - targetSum]
		}
		m[cur]++
		DFS(tn.Left,cur)
		DFS(tn.Right,cur)
		m[cur]--
	}
	DFS(root,0)
	return res
}
```
# 6,Combination Sum II -- [leetcode](https://leetcode-cn.com/problems/combination-sum-ii/)
```go {.line-numbers}
func combinationSum2(candidates []int, target int) [][]int {
	sort.Ints(candidates)
	lg, sum := len(candidates), 0
	res，ress,flag := make([][]int, 0),make([]int, 0),make([]bool, lg)

	var DFS func(int)
	DFS = func(index int) {
		if sum >= target {
			if sum == target {
				res = append(res, append([]int{}, ress...))
			}
			return
		}
		for i := index; i < lg; i++ {
			if i > 0 && candidates[i] == candidates[i-1] && !flag[i-1] {
				continue
			}
			sum += candidates[i]
			ress = append(ress, candidates[i])
			flag[i] = true
			DFS(i + 1)
			sum -= candidates[i]
			flag[i] = false
			ress = ress[:len(ress)-1]
		}
	}
	DFS(0)
	return res
}
```
# 7,Combination Sum III -- [leetcode216](https://leetcode-cn.com/problems/combination-sum-iii/)
```go {.line-numbers}
func combinationSum3(k int, n int) [][]int {
	res, ress, sum := make([][]int, 0), make([]int, 0), 0
	var DFS func(int)
	DFS = func(index int) {
		if sum == n && len(ress) == k {
			res = append(res, append([]int{}, ress...))
			return
		}
		if sum > n || len(ress) > k {
			return
		}
		for i := index; i <= 9; i++ {
			sum, ress = sum+i, append(ress, i)
			DFS(i + 1)
			sum, ress = sum-i, ress[:len(ress)-1]
		}
	}
	DFS(1)
	return res
}
```
# 8,Subsets -- [leetcode78](https://leetcode-cn.com/problems/subsets/)
```go {.line-numbers}
//DFS
func subsets(nums []int) [][]int {

	res, ress, lg := make([][]int, 0), make([]int, 0), len(nums)
	res = append(res, []int{})
	var DFS func(int)
	DFS = func(index int) {
		for i := index; i < lg; i++ {
			ress = append(ress, nums[i])
			res = append(res, append([]int{}, ress...))
			DFS(i + 1)
			ress = ress[:len(ress)-1]
		}
	}
	DFS(0)
	return res
}
//solution 2
func subsets(nums []int) [][]int {

	res, ress := make([][]int, 0), make([]int, 0)
	for mask := 0; mask < 1<<len(nums); mask++ {
		for i,Val := range nums{
			if (mask>>i)&1 == 1{
				ress = append(ress, Val)
			}
		}
		res = append(res, append([]int{}, ress...))
		ress = []int{}
	}
	return res
}
```
# 9,Subsets II -- [leetcode90](https://leetcode-cn.com/problems/subsets-ii/)
```go {.line-numbers}
func subsetsWithDup(nums []int) [][]int {
	sort.Ints(nums)

	res, ress, lg := make([][]int, 0), make([]int, 0), len(nums)
	res = append(res, []int{})
	flag := make([]bool, lg)

	var DFS func(int)
	DFS = func(index int) {

		for i := index; i < lg; i++ {
			if i > 0 && !flag[i-1] && nums[i] == nums[i-1] {
				continue
			}
			ress = append(ress, nums[i])
			flag[i] = true
			res = append(res, append([]int{}, ress...))
			DFS(i + 1)
			ress = ress[:len(ress)-1]
			flag[i] = false
		}
	}
	DFS(0)
	return res
}
```
# 10,Palindrome Partitioning -- [leetcode131](https://leetcode-cn.com/problems/palindrome-partitioning/)
```go {.line-numbers}
func partition(s string) [][]string {
	lg := len(s)
	dp := make([][]bool, lg)
	res, ress := make([][]string, 0), make([]string, 0)
	for i := range dp {
		dp[i] = make([]bool, lg)
		for j := range dp[i] {
			dp[i][j] = true
		}
	}
	for i := lg - 1; i >= 0; i-- {
		for j := i + 1; j < lg; j++ {
			dp[i][j] = s[i]== s[j] && dp[i+1][j-1] 
		}
	}

	var DFS func(int)
	DFS = func(index int) {
		if index == lg {
			res = append(res, append([]string{}, ress...))
			return
		}
		for i := index; i < lg; i++ {
			if dp[index][i] {
				ress = append(ress, s[index:i+1])
				DFS(i + 1)
				ress = ress[:len(ress)-1]
			}
		}
	}
	DFS(0)
	return res
}
```
# 11,Letter Combinations of a Phone Number -- [leetcode17](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)
```go {.line-numbers}
func letterCombinations(digits string) []string {
	if len(digits) == 0{
        return []string{}
    }
	var m = map[byte][]byte{
		'2': {'a', 'b', 'c'},
		'3': {'d', 'e', 'f'},
		'4': {'h', 'i', 'g'},
		'5': {'k', 'l', 'j'},
		'6': {'m', 'n', 'o'},
		'7': {'p', 'q', 'r', 's'},
		'8': {'t', 'u', 'v'},
		'9': {'w', 'x', 'y', 'z'},
	}
	res, ress := make([]string, 0), make([]byte, 0)
	var DFS func(int)
	DFS = func(index int) {
		if index == len(digits) {
			res = append(res, string(ress))
			return
		}
		for i := 0; i < len(m[digits[index]]); i++ {
			ress = append(ress,m[digits[index]][i])
			DFS(index + 1)
			ress = ress[:len(ress)-1]
		}
	}
	DFS(0)
	return res
}
``` 
# 12,Letter Case Permutation -- [leetcode784](https://leetcode-cn.com/problems/letter-case-permutation/submissions/)
```go {.line-numbers}
func letterCasePermutation(s string) []string {

    res:= make([]string,0)
    var DFS func(int,[]byte)
    DFS = func(index int,str []byte){
        res = append(res,string(str))
        for i:=index;i<len(s);i++{
            tmp := str[i]
            if str[i] >= 'A' && str[i] <= 'Z'{
                str[i] = str[i] + 'a' - 'A'
            }else if str[i] >= 'a' && str[i] <= 'z'{
                str[i] = str[i] + 'A' - 'a'
            } else{
                continue
            }
            DFS(i+1,str)
            str[i] = tmp
        }
    }
    DFS(0,[]byte(s))
    return res
}
```
# 13,Generate Parentheses -- [leetcode22](https://leetcode-cn.com/problems/generate-parentheses/)
```go {.line-numbers}
func generateParenthesis(n int) []string {
	res, ress := make([]string, 0), make([]byte, 0)

	var DFS func(int, int)
	DFS = func(left, right int) {
		if left < right {
			return
		}
		if left == n && right == n {
			res = append(res, string(ress))
			return
		}
		if left < n {
			ress = append(ress, '(')
			DFS(left+1, right)
			ress = ress[:len(ress)-1]
		}
		if right < n {
			ress = append(ress, ')')
			DFS(left, right+1)
			ress = ress[:len(ress)-1]
		}
	}
	DFS(0, 0)
	return res
}
```