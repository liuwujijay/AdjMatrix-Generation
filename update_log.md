# 大王叫我来巡山的开发日记~~

## 2017.04.15
### 1. 增加
    --- ⭐️️️️ ️️⭐️ ⭐️ 搭建Hierarchy GAN的学习平台，目标是使得重构回去的矩阵与原始矩阵之间的L1-norm最小
                --1. 重构矩阵 re_adj = w1*adj1+w2*adj2+w3*adj3+... [w1 - 第layer层的权重 | adj1 - 第layer层重构出的adj;]
                --2. 优化方式 learning_rate_decay / AdamOptimizer

## 2017.04.14
### 1. 增加
    --- 改 update_log.txt 为 md文件~
    --- ⭐️️️️ ️️⭐️ ⭐️ 在 Condition_model.py 中 增加 reconstructMat() 方法， 用以重建邻接矩阵
    --- 在Condition_model.py中增加reconstruction部分。具体体现在模型建立时新增 reconstruction()函数，以及新建三个tf变量用于存放重建时需要给定的generator的输入，从磁盘上读入的OutDegree以及对应的inputMat. 重建出的所有邻接矩阵采用DiGraph的形式存放在 reconstruction/<filename>/Hierarchy/*.nxgraph 中
### 2. Fix Bugs:
    --- Contiditional_model.py 将TF中的变量存储改成变量名+inputMatSize, 使得再利用TF的时候不会出现 变量冲突问题~
    --- 更名 *_Topology_MAIN.py 中的 node2Part字典为 Part2node, 为与之前的adj, adj_outneighbor, outDegree字典对应, node2Part格式:
            Part2node = {
                part_num:   {adj_idx:node_idx,...}, # 节点在当前adj中的对应位置 : 节点在原图中的编号
                part_num2:  {...},
                ...
            }


## 2017.04.13 --- Major Improvement!! 增加 Hierarchy Model 方法
### 1. 增加
    --- GAN_TOPOLOGY_MAIN.py 增加SaveModel的方法~~ 以保证训练完成之后将模型保存到本地
    --- 增加 external_tools 文件夹，存放一些外部工具
    --- 创建 Hierarchy_Topology_MAIN.py 用于根据Louvain的层次化社团划分结果产生相应的训练数据
    --- ⭐️️️️ ️️⭐️ ⭐️ 将 原程序变成 Condition 部分 和 Hierarchy 部分
    --- ⭐️️️️ ️️⭐️ ⭐️ 新增加 Hierarchy_model 用作带层次的GAN /Hierarchy_GAN_Topology 用作该带层次的GAN的主函数


## 2017.04.12
### 增加
    --- 1. 将原main函数改成 GAN_TOPOLOGY_MAIN.py
    --- 2. 新增Topology分析函数，现在可以对一个大网络进行分解~ TOPOLOGY_MAIN.py


## 2017.04.11
### 1. 编译metis工具~ 并将编译好的图分割工具放在这里以备后面进行图划分操作

### 2. Fix Bugs:
    --- 将__load_Adj 函数中的冗余操作去掉，全部换成直接对np的操作。（之前是先生成tf,再在返回的时候采用.eval转换成np）

## 2017.04.10
### 1. 增加
    --link_possibility 0.5 : 用于指示当前在构建邻接矩阵的时候，如果该邻接矩阵相应位置的值>0.5，则表明这两个节点之间有边相连
    --trainable_data_size 20000 : 用于指定训练数据的个数

### 2. Fix Bugs:
    --- 引入 math.ceil() 对 Generator / Discriminator 中卷积之后的 adj-mat 大小统一进行向上取整，保证结果一致性
    --- 保存 graph 的 pos 文件时 不采用 GEN生成的 graph, 因为最开始 Gen 生成的 graph 一定没有规律
    --- 将路径中的'/' 改成 os.path.join, 保证不同平台运行的顺畅