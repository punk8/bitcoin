# bitcoin

## 系统架构图
![image](https://github.com/131250106/bitcoin/blob/master/img/design.png)

## 系统模块设计图
![image](https://github.com/131250106/bitcoin/blob/master/img/module.png)

## 创世节点启动流程图
![image](https://github.com/131250106/bitcoin/blob/master/img/initialnode.png)

## 其它节点启动时序图
![image](https://github.com/131250106/bitcoin/blob/master/img/time.png)

## POW共识算法设计与实现
1. 所有节点启动完毕后，新开一个线程，执行挖矿逻辑
2. 某个节点一旦挖到某个block后，从transaction池中拉取TX，验证并放入新的block中
3. 该节点将new block 广播出去后，继续挖下一个区块
4. 某一节点一旦收到某个new block，立即发送一个信号量，停止当前挖矿行为
5. 验证该收到的new block

	5.1 如果验证通过，则将该new block添加到自己的链上
	
	5.2 验证不通过：
	
		5.2.1 从自己的routing table中的所有邻居节点拉取区块链
		
		5.2.2 逐个与自身区块链进行对比
		
			5.2.2.1 若区块链比自己的长，则用该区块链覆盖掉自己的链
			
			5.2.2.2 若区块链与自己的一样长，寻找fork分支点，进行fork操作
			
6. 继续挖下一个区块

## 不同算力攻击成功概率（这里模拟双花攻击）
1. 攻击节点一旦挖到新的区块时，除了从transaction池中拉取TX外，立即新建一个TX，花费自己所有的钱，给-1号地址（模拟取现过程），并创建两个new block，
block1 包含该TX，block2不包含这个TX
2. 攻击节点给正常节点发送block1，给攻击节点发送block2
3. 正常节点收到该block时，会校验block1的合法性，发现正常，确认该TX，此时取现过程被确认，攻击者取到第一笔现金
4. 攻击节点收到该block时，会校验block2的合法性，正常，接收该block

	5.1 若攻击节点再次挖到新的区块，除了从transaction池中拉取TX外，立即新建一个TX，再次花费自己所有的钱，给-1号地址（模拟取现过程），重复1步骤
	
		5.1.1 攻击节点收到块，验证，不冲突，加入区块链
	
		5.1.2 正常节点收到块，验证，冲突，拉取其它节点的区块链，若链比自己长，则覆盖（最终上次的取现交易被覆盖掉，成功双花）
	
	5.2 若正常节点再次挖到新的区块，则进行正常逻辑，区块链正常运行

6. 一段时间后，观察攻击节点给-1号地址的TX的所有金额，即全部取现金额，若金额大于本身比特币金额，则表示攻击成功。
7. 记录不同算力下攻击成功概率

### 理论上分析：
攻击成功的概率 = 系统算力占比

### 模拟实验结果：
| 攻击者算力占比 | 20% | 40% | 60% | 80% | 
|----------------|-----|-----|-----|-----|
| 攻击成功概率   |     |     |     |     |



## 不同带宽下分叉概率
//TODO

## BGD劫持 or eclipse攻击
//TODO

## POS共识算法设计与实现
//TODO

## PBFT共识算法设计与实现
//TODO

