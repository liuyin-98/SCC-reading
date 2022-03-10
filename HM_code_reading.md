# HM代码阅读

## 关键类的说明

### 各个类之间的关系图

![](img/relationship_between_classes.png)

### TComDataCU







## 帧间预测

### Void TEncSearch::xEstimateMvPredAMVP

-  ```c++
  Void TComDataCU::fillMvpCand ( const UInt partIdx, 
                                const UInt partAddr, 
                                const RefPicList eRefPicList, 
                                const Int refIdx, 
                                AMVPInfo* pInfo ) const
  {}
  ```

  该函数用于填充AMVP模式的MV候选列表（两个），填充规则如下

  1.  空域候选（5选1）

     ![inter_symmetric_split_spatial_candidates](img\inter_symmetric_split_spatial_candidates.png)

     ​	从当前PU的 **左侧** 和 **上方** 各选择一个。

     ​	**左侧**的选择顺序为 $A_0 \rightarrow A_1 \rightarrow scaled\ A_0 \rightarrow scaled\ A_1$

     ​	**上方** $B_0 \rightarrow B_1 \rightarrow B_2 ( \rightarrow scaled\ B_0 \rightarrow scaled\ B_1 \rightarrow scaled\ B_2)$

     - scaled

       尺缩比例需要根据 **当前块与其参考帧**(td) 以及 **参考块与其参考帧** (tb)的距离来确定，即 $curMV = \frac{td}{tb}colMV$

     - 终止条件

       如果第一个就可用则选择它，不用考虑剩余部分

     - 矩形划分需要注意

       ![inter_asymmetric_split_spatial_candidates](img\inter_asymmetric_split_spatial_candidates.png)

       对于(a)来说，$PU_2$的候选列表不能存在$A_1$，否则可能与不划分的情况重合

  2.  时域候选（与Merge类似，2选1）

     ![inter_temporal_candidates](img\inter_temporal_candidates.png)

     ​	顺序为 $scaled\ H \rightarrow scaled\ C_3$