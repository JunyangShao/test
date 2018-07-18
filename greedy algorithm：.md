## greedy algorithm：

1.calculate the beam effect matrix(**ray effect matrix actually**) $dose_{beam_i}[numofvoxels]$.

2.use the matrix to pick out some needed beams:

​	1)every beam is set to the same density(dose), for example, 1.

​		*The matRad optimization is to determine a beam dose, but we set this as a constant, and our optimization is to find out a best set of beams and its angles*

​	2)to sum up the beam effect in every voxels to be the effect of the $voxel$:

​		Special data structures:

​		①$B[numofbeams]$

​		beam set, with member:

​		$dose[i][j]=dose_{beam_i}[numofvoxels]$

​		$score[i][j]=score_{beam_iT[j]}$

​		$penalty[i][j]=penalty_{beam_iNT[j]}$

​		②$T[now][j]=\sum_{i=1}^{numofbeams}dose_{beam_i}[j]$

​		$T[goal][j]=ToBeSet$

​		$T[need][j]=T[goal][j]-T[now][j]$

​		③$NT[now][j]=\sum_{i=1}^{numofbeams}dose_{beam_i}[j]$

​		$NT[threshold][j]=ToBeSet$

​		$NT[left][j]=NT[threshold][j]-NT[now][j]$

​		two separate data structures.

​		④$plan$

​		not $pln$ in matRad, it determines the $T[goal]$ and $NT[threshold]$

​	3)use a ratio : $C=\frac{effectONtumor}{effectONorgan}$ to describe how good a solution is(bigger is better).

​	4)beam set $B$ comprise:

​		①for each tumor voxel we create a constant number of beams to go through it(like a sphere).

​		②for every two tumor voxel we create a beam that go through it.

​	5)beam $score$ and $penalty$:

​		determined by the $plan$:

​			①$score[i][j]=score_{beam_iT[j]}=min(dose[i][j]-T[now][j],T[goal][j]-T[now][j])$

​			② $penalty[i][j]=penalty_{beam_iNT[j]}=NT[threshold][j]-NT[now][j]-dose[i][j]$

​	7)pick out beams:

​		for each beam:

​		①rank beams in $B$ by $score(sumupallvoxels)$, get $BT$

​		②pick beams from $BT$:

```
j=0;
for(i=1:numofbeams)
{
    if(BT[i].penalty> vec(0))
    {
        D[j]=BT[i];
        j++;
        for(each beam t in BT)
        {
        BT[t].score-=B[i].score;
        BT[t].penalty-=B[i].penalty;
        }
    }
}
if(exist beam t that BT[t].score>vec(0)) no solution,return;
```



​		

​		

​		

​		



