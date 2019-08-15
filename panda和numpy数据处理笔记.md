## Pandas 数据清洗

### 数据基本属性操作

#### 修改列名或索引名`rename`

```python
# 修改列名
data = data.rename(columns={0:'id', 2:'labels'})
# 修改索引
data = data.rename(index={0:'row'})
# 修改列名和索引
data = data.rename(index={0:'row'},columns={0:'id', 2:'labels'})
data.rename(index={0:'row'},columns={0:'id', 2:'labels'}, inplace=True)

# 设置某列(Year)为索引
data = data.set_index('Year', drop = True)
# 删除某列(Year)
del data['Year']
```

#### 查看数据行和列

```python
# 列数
data.shape[1]
# 行数
data.shape[0]
# 打印全部的列名称
data.columns
```

#### 选取数据`iloc`和`loc`

```python
# iloc[行, 列]
# 取所有行，列取前7
data.iloc[:, 0:7]
# 选取除了最后3列之外的全部列
data.iloc[:, :-3]
# 每隔2列取
data.iloc[:, 0::2]
```

```python
# loc 按条件选取
# 选择列名为特定值，列名：Team
data.loc[data.Team.isin(['England', 'Italy', 'Russia']), ['Team','Shooting Accuracy']]
```

| id   | Team    | Shooting Accuracy |
| :--- | :------ | ----------------- |
| 3    | England | 50.0%             |
| 7    | Italy   | 43.0%             |
| 12   | Russia  | 22.5%             |

#### 查看某一列统计信息

```python
# 数据列名：item
# 数量最多的商品
data.item.value_counts().head(1)
# 此列有多少种类
data.item.nunique()
# 一共有多少订单
data.item.value_counts().count()
```

#### 按照先`Red Cards`再`Yellow Cards`进行排序

```python
# ascending = False 降序. ascending = True 升序.
data.sort_values(['Red Cards', 'Yellow Cards'], ascending = False)
```

| id   | Team                | Yellow Cards | Red Cards |
| :--- | :------------------ | :----------- | :-------: |
| 6    | Greece              | 9            |     1     |
| 9    | Poland              | 7            |     1     |
| 11   | Republic of Ireland | 6            |     1     |
| 7    | Italy               | 16           |     0     |
| 10   | Portugal            | 12           |     0     |

#### 按条件查询数据

```python
# data 中某一列为Goals
data[data.Goals > 6]
# 选取以字母G开头的球队数据, 列名：Team
data[data.Team.str.startswith('G')]
```

### 数据分割和处理

#### `lambda` 数据分割 

原始数据：`a,b,c`

```python
data['columns'] = data['columns'].apply(lambda row: str(row).replace(',', ' '))
```

执行后：`a b c`

#### 数据拼接`concat`, `merge`

```python
# 将data1和data2两个数据框按照行的维度进行合并, 索引值不变
data = pd.concat([data1, data2])
```

| id   | subject_id | first_name | last_name |
| :--- | :--------- | :--------- | --------- |
| 0    | 1          | Alex       | Anderson  |
| 1    | 2          | Amy        | Ackerman  |
| 2    | 3          | Allen      | Ali       |
| 0    | 4          | Billy      | Bonder    |
| 1    | 5          | Brian      | Black     |
| 2    | 6          | Bran       | Balwner   |

```python
# 将data1和data2两个数据框按照列的维度进行合并
all_data_col = pd.concat([data1, data2], axis = 1)
```

**merge**

```python
# data1和data2 根据id合并
pd.merge(data1, data2, on='id')
# how = 'inner':根据id的交集合并
# how = 'left':根据data1的id合并
# how = 'right':根据data2的id合并
# how = 'outer':data1 和 data2 合并之后的所有匹配结果
pd.merge(data1, data2, on='id', how='inner')
```



#### 将某列转换为浮点数

```python
# 列名：item
dollarizer = lambda x: float(x[1:-1])
data.item = data.item.apply(dollarizer)
```





### 参考链接

[这十套练习，教你如何使用Pandas做数据分析](<https://www.kesci.com/home/project/5a8afe517f2d695222327e14>)

