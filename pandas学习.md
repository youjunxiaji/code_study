## pandas简介及快速入门

### Anaconda 简介

Anaconda是一个很大的工具(1G)，但是为了解决这个问题，发现了[miniconda](https://docs.conda.io/en/latest/miniconda.html)，安装包才几十兆。

### pip安装第三方库

将库的版本升级到最新版：`pip install 库名 -U`

### pandas快速入门

#### 查看数据

```python
df.head() #查看前5条
df.tail() #查看尾部5条
df.sample() #随机查看5条
```

#### 验证数据

```python
df.shape #查看行数和列数
df.info() #查看索引、数据类型和内存信息
df.descirbe() #查看汇总统计
df.dtypes #查看字段类型
df.axes #查看数据行和列名
```

#### 建立索引

```python
df.set_index("name",inplace=True) #建立索引并生效
```

* `name`必须是已经存在于表格中的标题，否则会报错
* `inplace=True`使修改生效

#### 数据选取

1. 选择多列
   ```python
   df[["A","B"]] #查看A,B两列
   df.loc[:,["A","B"]] #和上面一行的效果一样
   ```
2. 指定行和列
   ```python
   df.loc["Ben","Q1":"Q4"] #只看Ben四个季度的成绩
   df.loc["Eorge":"Alexander","team":"Q4"] #指定行区间
   ```
3. 组合条件
    ```python
    df[(df["Q1"]>90)&(df["team"]=="C")] 
    ```
    **注意**：在pandas中，and即是&

#### 排序

```python

```