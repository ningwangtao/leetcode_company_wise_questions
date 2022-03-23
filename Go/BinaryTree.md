# 1,Binary Tree Preorder Traversal -- [leetcode144](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)
## stack/dfs
 ```go {.line-numbers}
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func preorderTraversal(root *TreeNode) []int {
	stack := make([]*TreeNode, 0)
	res := make([]int, 0)
	node := root

	for nil != node || len(stack) > 0 {
		if nil != node {
			//if node is not nil，push the stack
			stack = append(stack, node)
            res = append(res, node.Val)
			node = node.Left
		//until the left child is nil
		} else {
			//get the top element of stack
			node = stack[len(stack)-1]
			//pop the stack
			stack = stack[:len(stack)-1]
			//handle right child
			node = node.Right
		}
	}
	return res
}
 ```

 # 2,Sum Root to Leaf numbers -- [leetcode129](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)
 ## recursive/backtracking
 ```go {.line-numbers}
 /**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func sumNumbers(root *TreeNode) int {
	res,ress := 0,0
	var DFS func(*TreeNode)
	DFS = func(tn *TreeNode) {
		if tn == nil {
			return
		}
		ress = ress*10 + tn.Val
		if tn.Left == nil && tn.Right == nil {
			res += ress
            //backtracking
            ress = (ress - tn.Val) / 10
			return
		}
		DFS(tn.Left)
		DFS(tn.Right)
        //backtracking
        ress = (ress - tn.Val) / 10
	}
	DFS(root)
	return res
}
 ```

# 3,Maximum Depth of Binary Tree -- [leetcode104](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
## recursive

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func maxDepth(root *TreeNode) int {
    if root == nil{
        return 0
    }

    left := maxDepth(root.Left)
    right := maxDepth(root.Right)
    if left > right {
        return left+1
    }else{
        return right +1
    }
}
```
# <font color=#E9967A>4,Maximum Width fo Binary Tree</font> -- [leetcode662](https://leetcode-cn.com/problems/maximum-width-of-binary-tree/)
## queue/level order traversal

```go {.line-numbers}
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
type pairnode struct {
	node *TreeNode
	num  int
}

func widthOfBinaryTree(root *TreeNode) int {
	if root == nil {
		return 0
	}
	queue := []pairnode{pairnode{root, 1}}
	index, maxWidth := 0, -1
	for len(queue) > 0 {
		index = len(queue)
		l, r := queue[0].num, queue[len(queue)-1].num
		if maxWidth < r-l+1 {
			maxWidth = r - l + 1
		}
		for i := 0; i < index; i++ {
			if queue[0].node.Left != nil {
				tmp := pairnode{queue[0].node.Left, queue[0].num * 2}
				queue = append(queue, tmp)
			}
			if queue[0].node.Right != nil {
				queue = append(queue, pairnode{queue[0].node.Right, queue[0].num*2 + 1})
			}
			queue = queue[1:]
		}
	}
	return maxWidth
}
```

# 5,Binary Tree Level Order Traversal -- [leetcode102](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
## level order traversal
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func levelOrder(root *TreeNode) [][]int {

	if nil == root {
		return [][]int{}
	}
	index,floor := 1,0
	res := make([][]int, 0)
	ress := make([]int, 0)

    queue := []*TreeNode{root}
	for len(queue) > 0 {
        index = len(queue)
		for i := 0; i < index; i++ {
			node := queue[0]
			queue = queue[1:]
			ress = append(ress, node.Val)
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		res = append(res, ress)
		ress = []int{}
		floor++
	}
	return res
}
```
# 6,Binary Tree Level Order Traversal II -- [leetcode107](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)
## level order traversal
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func levelOrderBottom(root *TreeNode) [][]int {

    if root == nil {
        return [][]int{}
    }

    queue := []*TreeNode{root}
    index,floor := 1,0
    res := make([][]int,0)
    
    for len(queue) > 0{
        res = append(res,[]int{})
        index = len(queue)
        for i:=0;i<index;i++{
            res[floor] = append(res[floor],queue[0].Val)
            if queue[0].Left != nil{
                queue = append(queue,queue[0].Left)
            }
            if queue[0].Right != nil{
                queue = append(queue,queue[0].Right)
            }
            queue = queue[1:]
        }
        floor++
    }
    for i := 0; i < floor/2; i++ {
		res[i],res[floor-1-i] = res[floor-1-i],res[i]
	}
    return res
}
```

# 7,Binary Tree Zigzag Level Order Traversal -- [leetcode103](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
## bfs/level order traversal
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func zigzagLevelOrder(root *TreeNode) [][]int {
	if nil == root {
		return [][]int{}
	}

	floor := 0
	res := make([][]int, 0)
	//queue push
	queue := []*TreeNode{root}

	for len(queue) > 0 {
        res = append(res, []int{})
        index := len(queue)
		for i := 0; i < index; i++ {
			res[floor] = append(res[floor], queue[0].Val)
			if queue[0].Left != nil {
				queue = append(queue, queue[0].Left)
			}
			if queue[0].Right != nil {
				queue = append(queue, queue[0].Right)
			}
			//queue pop
			queue = queue[1:]
		}
		if floor%2 == 1 {
			for i, j := 0, len(res[floor])-1; i < j; i, j = i+1, j-1 {
				res[floor][i], res[floor][j] = res[floor][j], res[floor][i]
			}
		}
		floor++
	}
	return res
}
```

# <font color=#E9967A>8,Path Sum II</font> -- [leetcode113](https://leetcode-cn.com/problems/path-sum-ii/)
## backtracking
```go
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
		//backtracking func
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
# <font color=#E9967A>9,Path Sum III</font> --[leetcode437](https://leetcode-cn.com/problems/path-sum-iii/)
## DFS
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func DFS(node *TreeNode, target int) (res int) {
	if node == nil {
		return 0
	}
	if node.Val == target {
		res += 1
	}
	res += DFS(node.Left, target-node.Val)
	res += DFS(node.Right, target-node.Val)
	return res
}

func pathSum(root *TreeNode, targetSum int) int {
	if root == nil {
		return 0
	}
	res := DFS(root, targetSum)
	res += pathSum(root.Left, targetSum)
	res += pathSum(root.Right, targetSum)
	return res
}
```
## prefix sum + backtracking + preorder traversal
```go
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
# 10,Maximum Difference Between Node and Ancestor -- [leetcode1026](https://leetcode-cn.com/problems/maximum-difference-between-node-and-ancestor/)
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func DFS(root *TreeNode, min int ,max int) int{
    if root == nil{
        return max - min
    }
    if min > root.Val{
        min = root.Val
    }
    if max < root.Val{
        max = root.Val
    }
    l := DFS(root.Left,min,max)
    r := DFS(root.Right,min,max)
    return maxfunc(l,r)
}
func maxAncestorDiff(root *TreeNode) int {
    if root == nil{
        return 0
    }
    return DFS(root,root.Val,root.Val)
}
func maxfunc(a,b int) int{
    if a > b{
        return a
    }else{
        return b
    }
}
```
# 11,Binary Tree Pruning -- [leetcode814](https://leetcode-cn.com/problems/binary-tree-pruning/)
```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func execute(root *TreeNode) bool {
	if root == nil {
		return false
	}
	l := execute(root.Left)
	r := execute(root.Right)
	if !l {
		root.Left = nil
	}
	if !r {
		root.Right = nil
	}
	if !l && !r && root.Val == 0 {
		return false
	}
	return true
}

func pruneTree(root *TreeNode) *TreeNode {
	res := execute(root)
	if !res {
		return nil
	}
	return root
}
```