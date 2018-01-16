# attribute2relation

标签（空格分隔）： 知识图谱

---

**2018.1.6**
实验数据:
    ENTITY: 1263081
    RELATION: 1089
    TRAIN_TRIPLES: 1226601
    VALID_TRIPLES: 408867
    TEST_TRIPLES: 408867
    CANDIDATE_TRIPLES: 408867
candidate的生成说明：从test set中的尾实体，找到与其“名字相同”的实体，作为义项。这个是有问题的，这样保证了尾实体（标准答案）一定在candidate中，但是实际场景中并不一定。应该从尾实体的“mention”出发去寻找candidate（future work）
有一部分的candidate不在训练集合中，无法做测试。去除这一部分。得到的candidate的数量分布

| candidate数量        |  测试样例数量  | 占比| 
| --------   | ----- | --- |
| 1 | 355346 | 0.869 |
| 2 | 31483 | 0.077 |
| 3 | 9349 | 0.023 |
| 4 | 4834 | 0.012 |
| 5 | 2897 | 0.007 |
| 6 | 1385 | 0.003 |
| 7 | 1024 | 0.003 |
| 8 | 845 | 0.002 |
| 9 | 476 | 0.001 |
| 13 | 228 | 0.001 |
| 10 | 206 | 0.001 |
| 14 | 185 | 0.0 |
| 12 | 180 | 0.0 |
| 11 | 175 | 0.0 |
| 16 | 56 | 0.0 |
| 15 | 51 | 0.0 |
| 17 | 40 | 0.0 |
| 53 | 35 | 0.0 |
| 20 | 17 | 0.0 |
| 21 | 14 | 0.0 |
| 22 | 13 | 0.0 |
| 18 | 10 | 0.0 |
| 19 | 9 | 0.0 |
| 25 | 8 | 0.0 |
| 24 | 1 | 0.0 |


TranE在所有测试样例（408867）的准确率为96%，但是这个相当于86%是没有歧义。
去掉candidate为1的测试样例，剩下的5w需要消除歧义的样例中准确率为（76%）

| margin| 表示维度  | batch_size |  准确率 (candidate>1)| 
| --------   | ----- | --- |---|
| 1 | 100 | 10240 |76 %|
| 5 | 100 | 10240 |81 %|
| 5 | 120 | 4096  |81 %|
| 5 | 200 | 10240 |80 %|
| 5 | 200 | 4096  |82 %|
| 10| 200 | 10240 |81 %|    
| 1 | 200 | 10240 |76 %|


**2018.1.8**
从尾实体生成candida的准确率是96%，从mention生成candidate，的准确率为82%，其中8.4%的标准答案不在candidate中。
去掉只有一个candidate的数据，在多个entity上匹配的准确率为53%其中有2.3%不在candidate中。


**2018.1.11**
| candidate数量|测试样例数量|测试样例占比| 尾实体在candidate|准确率上限| TransE Base|neg 1|
| --------   | ----- | --- |---|----|---|---|
| 0 | 28466 | 0.07 | 0 | 0.0 |0.0 | 0.0 |
| 1 | 298028 | 0.729 | 293911 | 0.986 | 0.986 | 0.986 |
| 2 | 46051 | 0.113 | 45250 | 0.983 |0.823 | 0.888 |
| 3 | 17267 | 0.042 | 16947 | 0.981 | 0.684 | 0.856 |
| 4 | 7647 | 0.019 | 7414 | 0.97 | 0.593 | 0.744 |
| 5 | 3481 | 0.009 | 3401 | 0.977 |0.559 | 0.783 |
| 6 | 2525 | 0.006 | 2454 | 0.972 | 0.541 | 0.695 |
| 7 | 1913 | 0.005 | 1726 | 0.902 | 0.478 |0.712 |
| 8 | 846 | 0.002 | 818 | 0.967 | 0.485 |0.758 |
| 9 | 606 | 0.001 | 538 | 0.888 |0.409 | 0.653 |
| 10 | 312 | 0.001 | 306 | 0.981 | 0.378 |0.75 |
| 11 | 276 | 0.001 | 265 | 0.96 | 0.428 |0.641 |
| 12 | 177 | 0.0 | 171 | 0.966 |0.282 |0.695 |
| 13 | 179 | 0.0 | 179 | 1.0 | 0.497 |0.715 |
| 14 | 230 | 0.001 | 226 | 0.983 | 0.448 | 0.778 |
| 15 | 81 | 0.0 | 78 | 0.963 |0.333 | 0.741 |
| 16 | 32 | 0.0 | 32 | 1.0 |0.375 |0.812 |
| 17 | 102 | 0.0 | 102 | 1.0 | 0.52 | 0.667 |
| 18 | 13 | 0.0 | 13 | 1.0 | 0.077 | 0.308 |
| 19 | 30 | 0.0 | 27 | 0.9 |0.2 | 0.7 |
| 20 | 21 | 0.0 | 6 | 0.286 |0.19 |0.286 |
| 21 | 23 | 0.0 | 23 | 1.0 | 0.087 |0.913 |
| 22 | 5 | 0.0 | 5 | 1.0 | 0.4 | 0.4 |
| 23 | 3 | 0.0 | 3 | 1.0 | 0.667 |0.333 |
| 24 | 13 | 0.0 | 13 | 1.0 |0.231 |0.538 |
| 25 | 6 | 0.0 | 6 | 1.0 |0.167 | 0.333 |
| 26 | 10 | 0.0 | 10 | 1.0 | 0.2 | 0.9 |
| 28 | 5 | 0.0 | 5 | 1.0 | 0.0 |0.0 |
| 37 | 33 | 0.0 | 33 | 1.0 | 0.333 | 0.545 |
| 47 | 403 | 0.001 | 403 | 1.0 | 0.739 | 0.968 |
| 54 | 44 | 0.0 | 0 | 0.0 |0.0 | 0.0 |
| 55 | 39 | 0.0 | 39 | 1.0 |0.179 | 0.641 |
![image_1c3kakgd8134ddtgd2b2ccs4b2s.png-189.5kB][1]

**2018.1.13**

![dis.png-36.7kB][2]
根据统计出的na的距离以及hit的距离设定阈值为20，没有获得太大的收益
![na.png-186.2kB][3]


**2018.1.14**
加入对比实验，description + relation集合，总体不是很稳定
![Screenshot from 2018-01-14 14-11-37.png-195.7kB][4]

**2018.1.15**
错误率最高的100种关系：
| 关系名称|测试样例数量|测试样例占比| 错误率|
| --------   | ----- | --- |----|
| 别称 | 1183 | 0.12 | 0.65 |
| 代表作品 | 1086 | 0.11 | 0.11 |
| 别名 | 965 | 0.1 | 0.69 |
| 作者 | 595 | 0.06 | 0.02 |
| 主演 | 526 | 0.05 | 0.02 |
| 职业 | 338 | 0.03 | 0.06 |
| 登场作品 | 231 | 0.02 | 0.06 |
| 所属专辑 | 217 | 0.02 | 0.12 |
| 导演 | 177 | 0.02 | 0.03 |
| 类型 | 177 | 0.02 | 0.03 |
| 中文名 | 176 | 0.02 | 0.03 |
| 出生地 | 128 | 0.01 | 0.02 |
| 星座 | 127 | 0.01 | 0.13 |
| 编剧 | 126 | 0.01 | 0.03 |
| 歌曲语言 | 125 | 0.01 | 0.3 |
| 主要成就 | 112 | 0.01 | 0.02 |
| 主要食材 | 111 | 0.01 | 0.02 |
| 地理位置 | 110 | 0.01 | 0.02 |
| 著名景点 | 106 | 0.01 | 0.04 |
| 文学体裁 | 105 | 0.01 | 0.04 |
| 简称 | 81 | 0.01 | 0.5 |
| 组 | 80 | 0.01 | 0.04 |
| 主要原料 | 74 | 0.01 | 0.01 |
| 出处 | 74 | 0.01 | 0.03 |
| 创作年代 | 69 | 0.01 | 0.04 |
| 行政区类别 | 64 | 0.01 | 0.07 |
| 填词 | 57 | 0.01 | 0.02 |
| 译者 | 57 | 0.01 | 0.03 |
| 调料 | 56 | 0.01 | 0.02 |
| 中文名称 | 53 | 0.01 | 0.04 |
| 辅料 | 52 | 0.01 | 0.02 |
| 类别 | 49 | 0.0 | 0.0 |
| 系 | 48 | 0.0 | 0.05 |
| 谱曲 | 46 | 0.0 | 0.02 |
| 所属地区 | 44 | 0.0 | 0.01 |
| 种 | 41 | 0.0 | 0.01 |
| 配音 | 40 | 0.0 | 0.01 |
| 主持人 | 39 | 0.0 | 0.06 |
| 释义 | 39 | 0.0 | 0.04 |
| 近义词 | 38 | 0.0 | 0.06 |
| 制片人 | 34 | 0.0 | 0.03 |
| 歌曲原唱 | 34 | 0.0 | 0.01 |
| 主料 | 33 | 0.0 | 0.02 |
| 配料 | 31 | 0.0 | 0.02 |
| 饰演 | 31 | 0.0 | 0.04 |
| 编曲 | 31 | 0.0 | 0.03 |
| 定义 | 29 | 0.0 | 0.03 |
| 知名校友 | 29 | 0.0 | 0.03 |
| 语言 | 29 | 0.0 | 0.06 |
| 位于 | 28 | 0.0 | 0.02 |
| 经营范围 | 28 | 0.0 | 0.02 |
| 反义词 | 27 | 0.0 | 0.07 |
| 专辑语言 | 26 | 0.0 | 0.21 |
| 解释 | 26 | 0.0 | 0.03 |
| 政府驻地 | 25 | 0.0 | 0.07 |
| 下辖地区 | 25 | 0.0 | 0.03 |
| 其他名称 | 24 | 0.0 | 0.06 |
| 分类 | 24 | 0.0 | 0.01 |
| 书名 | 23 | 0.0 | 0.02 |
| 地点 | 23 | 0.0 | 0.02 |
| 生肖 | 23 | 0.0 | 0.34 |
| 其它译名 | 23 | 0.0 | 0.05 |
| 亚属 | 22 | 0.0 | 0.01 |
| 位置 | 21 | 0.0 | 0.02 |
| 纲 | 20 | 0.0 | 0.0 |
| 现任校长 | 20 | 0.0 | 0.1 |
| 亚组 | 19 | 0.0 | 0.04 |
| 含义 | 18 | 0.0 | 0.03 |
| 特点 | 18 | 0.0 | 0.02 |
| 地址 | 18 | 0.0 | 0.02 |
| 对白语言 | 17 | 0.0 | 0.02 |
| 外文名 | 17 | 0.0 | 0.03 |
| 作品出处 | 17 | 0.0 | 0.01 |
| 又名 | 16 | 0.0 | 0.07 |
| 使用者 | 16 | 0.0 | 0.05 |
| 音乐风格 | 16 | 0.0 | 0.01 |
| 性质 | 16 | 0.0 | 0.01 |
| 作用 | 14 | 0.0 | 0.03 |
| 材料 | 14 | 0.0 | 0.02 |
| 父亲 | 14 | 0.0 | 0.04 |
| 主要角色 | 14 | 0.0 | 0.05 |
| 代表作 | 14 | 0.0 | 0.24 |
| 主要作品 | 13 | 0.0 | 0.06 |
| 组成 | 13 | 0.0 | 0.02 |
| 原料 | 13 | 0.0 | 0.01 |
| 作品名称 | 13 | 0.0 | 0.02 |
| 属性 | 13 | 0.0 | 0.02 |
| 用途 | 12 | 0.0 | 0.03 |
| 丈夫 | 12 | 0.0 | 0.07 |
| 身份 | 12 | 0.0 | 0.04 |
| 儿子 | 12 | 0.0 | 0.07 |
| 包括 | 12 | 0.0 | 0.02 |
| 内容 | 12 | 0.0 | 0.05 |
| 分布区域 | 12 | 0.0 | 0.01 |
| 属 | 11 | 0.0 | 0.0 |
| 妻子 | 11 | 0.0 | 0.09 |
| 简介 | 11 | 0.0 | 0.02 |
| 祖籍 | 11 | 0.0 | 0.13 |
| 原作 | 11 | 0.0 | 0.03 |
| 场上位置 | 11 | 0.0 | 0.08 |

**TODO**

- [x] 判断NA问题
- [ ] 加入语义信息进行消歧提高准确率
- [ ] 解决Zero shot问题
- [x] 从mention中生成更多的candidate，通过别名
- [ ] 从mention中生成更多的candidate，通过编辑距离
- [ ] 用mutiTask做Global Linking
- [ ] 增量式迭代训练


  [1]: http://static.zybuluo.com/zhangsh950618/xqba2shxghicldniyr468gbk/image_1c3kakgd8134ddtgd2b2ccs4b2s.png
  [2]: http://static.zybuluo.com/zhangsh950618/maeix3xcxblhmwqcms0ycy5m/dis.png
  [3]: http://static.zybuluo.com/zhangsh950618/wm8k08ksloiouk07aq4m51a6/na.png
  [4]: http://static.zybuluo.com/zhangsh950618/40berd6n771aimn0v2z233ib/Screenshot%20from%202018-01-14%2014-11-37.png