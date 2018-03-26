---
layout: page
mathjax: true
permalink: /2017/projects/p18/midterm/
---

## PISA数据集数据可视化-中期报告

### 小组成员

- 何果财
- 秦晓东

### 当前进展

对下载的数据集进行分析处理，对数据集提出简单问题，进行数据分析和可视化

### 数据集下载

数据集共包含485490学生的考察数据，主要包括学生成绩、家庭和学校情况三方面的数据。数据量比较大，但是下载下来的数据已经结构化处理，方便使用R语言进行数据分析和建模。

<img src="https://raw.githubusercontent.com/hegc/md_ims/master/dm3.PNG" />

其中，文件名以2012.rda结束的文件是数据文件，文件名以2012dict.rda结束的文件是字段名和字段的含义

### 明确问题范围

经过研究，我们将可视化问题的范围定义到两个问题上：

- 与学校相关的问题，研究内容包括教师、计算机、图书馆、入学时间、授课方式等方面
- 对成绩的变化进行探索，包括性别、国家地区、学习时间、学科、书籍等影响因素

### 现阶段完成的可视化

1.探索学生成绩与家中书籍的数量关系？

<img src="https://raw.githubusercontent.com/hegc/md_ims/master/dm4.png" />

2.学校和家庭对学生数学成绩的影响？

<img src="https://raw.githubusercontent.com/hegc/md_ims/master/dm5.PNG" />

### 下一步工作

根据现阶段的可视化结果，可以看到一些因素对学生成绩的影响，符合我们的预期。接下来，要利用更多的信息进行可视化，探索数据中蕴含的关系。