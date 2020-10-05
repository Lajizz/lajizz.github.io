---
title: 回溯法
tags: Algorithms
---
## 回溯法 Back-Tracking Method
* 回溯法是一个尝试的过程，如果出错可以进行回退。它是建立在一部分正确的的结果上再进行扩大，
寻找后一个正确的解。一步一步进行，当尝试失败时会进行回退。可以很容易知道，回溯有递归性，它
是维护一个栈。而结束的条件就是当成功找到或者栈为空（及寻找失败）。
* 回溯法是经典的算法，之前学习的时候一直不太懂，这次正好借一道题来再学习一下。

### 题目描述
* 请设计一个函数，用来判断在一个矩阵中是否**存在**一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向**左、右、上、下**移动一格。如果一条路径经过了矩阵的某一格，那么该路径**不能再次进入**该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。
> [["a","b","c","e"],
> ["s","f","c","s"],
> ["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。
示例 1：
```cpp
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
示例 2：

输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
```
### 分析与代码

```cpp
// 外面的入口函数，整体考虑
bool hasPath(char *matirx, int rows, int cols, char* str){
    //特殊情况处理
    if(matirx == nullptr|| rows<1 || cols<1 || str==nullptr )
        return false;
    //建立一个bool数组用来保存检查过程中已经走过的路，注意！不是走过一次就置ture 而是对现有达到的部分正确解标为true，如题：如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子
    bool* visited = new bool[rows*cols];
    memset(visited,0,rows*cols);
    int pathLength=0;
    for(int row = 0;row < rows;row++)
    {
        for(int col = 0; col < cols; col++){
            //把每个当成起始寻找
            if(hasPathcore(matrix,rows,cols,row,col,str,pathLength,visited))
            {
                return true;
            }
        }
    }
    delete [] visited;
    return false;
}
// 每一步的操作,pathLength的引用是关键
bool hasPath(char *matrix,int rows, int cols,int row, int col,const char* str,int &pathLength,bool* visited)
{
    //找到正确解
    if(str[pathLength]=='\0')
    {
        return true;
    }
    bool hasPath = false;
    //检查条件，很多但不难
    if(row>=0 && row<rows && col>=0 && col<cols &&  //边界检查
    pathLength>=0 && visited[row*cols+col]==false && 
    str[pathLength] == matrix[row*cols+col])
    {
        pathLength++;
        hasPath = true;
    
        visited[row*cols+cols]=true;

        hasPath = hasPathcore(matrix,rows,cols,row+1,col,str,pathLength,visited) ||
                  hasPathcore(matrix,rows,cols,row-1,col,str,pathLength,visited) ||
                  hasPathcore(matrix,rows,cols,row,col+1,str,pathLength,visited) ||
                  hasPathcore(matrix,rows,cols,row,col-1,str,pathLength,visited);
        if(!hasPath)
        {
            //回退
            pathLength--;
            visited[row*cols+cols]=false;
        }


    }

    return hasPath;
}
```