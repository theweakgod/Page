# 动态规划

## 动态规划
### 简介
动态规划(dynamic programming)与分治方法相似,都是通过组合子问题的解来求解原问题.但动态规划用来求解子问题重叠的情况.动态规划方法通常用来求解最优化问题(optimization problem).

#### 钢条切割
给定一段长度为n英寸的钢条和一个价格表Pi,求切割钢条方案,使得销售收益Rn最大.注意,如果长条为n英寸的钢条的价格Pn足够大,最优解可能就是完全不需要切割.
<table>
<tr>
	<th>
	长度i
	</th>
	<th>
	1 2 3 4 5 6 7 8 9 10
	</th>
</tr>
<tr>
	<td>
		价格Pi
	</td>
	<td>
	1 5 8 9 10 17 17 20 24 30
	</td>
</tr>
</table>

动态规划,自顶向下,加入备忘机制,伪代码:
``` javascript
let r[0...n] be a new array
for i = 0 to n
	r[i]=-$\Infty$
return MEMOIZED-CUT-ROD-AUX(p,n,r)

MEMOIZED-CUT-ROD-AUX(p,n,r){
	if(r[n]>=0)
		return r[n]
	if(n==0)
		q=0
	else(q==-$\Infty$)
		for i=1 to n
			q=max(q,p[i]+MEMOIZED-CUT-ROD-AUX(p,n-i,r))
	r[n]=q
	return q
}

```
简单计算一下,该函数传递进去,先计算`for i=1 to n`,那么调用的就是参数为n-1的函数,反复调用至第1层,则r[1...n-1]都大于0了.成功被赋值.第一次循环就是查看将长度为n的长条分成1根1根的价格是多少.之后的每一次都会取最大值.该过程就把求长度为n的长条的最大价值是多少分成了长度为n-1的长条的最大价值是多少的递归解决方案,但该方案是有记录的,也就是说,当n=3的问题求解之后,求解n=4的问题就迎刃而解.

``` Java
BOTTOM-UP-CUT-ROD(p,n)
  let r[0,n] be a new array
  r[0]=0
  for j=1 to n
    q=$\Infty$
    for i = 1 to j
      q= max(q,p[i]+r[j-i])
    r[j]=q
return r[n]
```
这个方法是自底向上的方法,就是从1->n,先求解出小于x的最优解:例如求5的最优解,则在算法中,已经知道了4(含)以下的最优解是多少,那么只需要排列组合1-4,3-2就能求出5的最优解是多少了
动态规划问题较难,可能难以理解.只需要慢慢掌握即可.
