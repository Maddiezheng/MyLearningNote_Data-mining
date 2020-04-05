# MyLearningNote_Data-mining

About me
=====
:neckbeard:: Maddie Zheng

:round_pushpin:: China

:house:: Taipei,Taiwan

:school:: Dept. of Big data management,Soochow University





About the course
====
Course Information
-----------------------
* Course Title: 資料探勘導論,INTRODUCTION TO DATA MINING
* Instructor：鍾震耀 CHUNG, CHEN-YAO
* Semester: 2019-2
* Credits: 3

Objectives
-----

資料探勘和資料驅動分析已成為商業領域的熱門話題。這門課程專為那些可能想要在這方面進行研究的專業學生設計。課程將介紹資料驅動分析的基本理論的基礎和應用的發展。除了學習課堂講座中的方法之外，還提供一個時段幫助學生使用R進行分析。目的是讓學生具備基本的能力，在商業分析領域進行學術和實務研究。

Data mining and data driven analytics have become a hot issue in the business world. This course is designed for the students who may want to conduct business research in this area. The course will introduce the basic theories of data driven analytics and the latest development in the academia. Besides learning the methods in the class lecture, a session is also offered to help students using R to conduct the analysis. The goal is to equip students with fundamental capability to conduct academic and practical research in business analytics.


My review of the course
========




Contents
=========
Week1:Introduction

Week2:從資料探勘角度漫談ERP與BI商業決策環境資訊應用；介紹關聯規則


:city_sunset



Week2
=======

什麼是關聯規則   What's the Associative Analysis?
---
>* 關聯分析的概念是由Agrawal et. al. （1993） 所提出，隨後，Agrawal & Srikant （1994） 進一步提出 Apriori演算法，以做為關聯法則之工具。
>* 關聯分析主要透過「支持度」（Support）與「信賴度」（Confidence）來對商品項目之間的關聯性，進行篩選。
>* 支持度（Support）意指即某項目集在資料庫中出現的次數比例。例如：某資料庫中有100筆交易紀錄，其中有20筆交易有購買啤酒，則啤酒的支持度為20%。
>* 信賴度（Confidence）意指兩個項目集之間的條件機率，也就是在A出現的情況下，B出現的機率值。

在電腦科學以及資料探勘領域中，Apriori 演算法是「關聯規則學習」或是「關聯分析（Associative Analysis）」的經典演算法之一，目的是在一個資料集當中，找出不同項與項之間可能存在的關係。而在行銷資料科學領域，它有個很特別的名字，被稱為「購物籃分析 (Market Basket analysis)」，也跟啤酒與尿布的故事有關。

在進行關聯分析時，我們通常會先設定最小支持度（Min Support）與最小信賴度（Min Confidence。`如果所設定的最小支持度與最小信賴度太低，則關聯出來的結果會產生太多規則，造成決策上的干擾。` 反之，太高的最小支持度與最小信賴度則可能會面臨規則太少，難以判斷的窘境。

Week3
=====
> Apriori_fundamental.R

```R
   #範例一
# table 14.1, Transactional data, 商業智慧與大數據分析(滄海書局) Page.457
# Items商品項目 : 飯糰、豆漿、尿布、啤酒、麥片、綠茶(有六種不同商品項目)
# dataset資料集合: total data = 5(有五筆交易資料)
# 最小支持度(minimun support, minsup) = 60%
# 最小信賴度(minimun confidence, minconf) = 60%
#|------------------|------------------------|
#| Transaction ID   |     Items              |
#|------------------|------------------------|
#|       T1         | {飯糰,豆漿,尿布}       |
#|       T2         | {飯糰,尿布,啤酒,麥片}  |
#|       T3         | {豆漿,尿布,啤酒,綠茶}  |
#|       T4         | {飯糰,豆漿,尿布,啤酒}  |
#|       T5         | {飯糰,豆漿,尿布,綠茶}  |
#|------------------|------------------------|
```


* 步驟一(Step 1.) *
安裝(install)與載入(load)"arules" 套件
```R
#install.packages("arules") #如果您電腦已經安裝過此行指令可以不用再執行
library(arules)            #載入arules套件   a:關聯規則
```
* 步驟二(Step 2.) *
準備資料prepare data

透過list()函數是將T1,T2,...,T5的交易內容串接建立成為list資料型態的變數並以xlist命名此變數

c()函數就是combine的意思，可以將前後資料串接起來成為向量vector資料型態

所以最後xlist中有五個文字向量，語法如下：
```R
xlist = list(
  c("飯糰","豆漿","尿布"),
  c("飯糰","尿布","啤酒","麥片"),
  c("豆漿","尿布","啤酒","綠茶"),
  c("飯糰","豆漿","尿布","啤酒"),
  c("飯糰","豆漿","尿布","綠茶")
)
```
其實上面指令也可以寫成一行，可以不用多行撰寫方式，但是一行指令太長會比較不容易閱讀
```R
xlist_test = list(c("飯糰","豆漿","尿布"),c("飯糰","尿布","啤酒","麥片"),c("豆漿","尿布","啤酒","綠茶"),c("飯糰","豆漿","尿布","啤酒"),c("飯糰","豆漿","尿布","綠茶"))
```

可以查看一下目前xlist變數資料型態
```R
class(xlist)
```

可以查看一下目前xlist變數資料內容，list型態在R中使用兩層中括號
```R
xlist
xlist[[1]]     #"飯糰","豆漿","尿布"
xlist[[2]]     #"飯糰","尿布","啤酒","麥片"
xlist[[3]]     #"豆漿","尿布","啤酒","綠茶"
xlist[[4]]     #"飯糰","豆漿","尿布","啤酒"
xlist[[5]]     #"飯糰","豆漿","尿布","綠茶"
xlist[[1]][1]  #"飯糰"
xlist[[1]][2]  #"豆漿"
xlist[[1]][3]  #"尿布"
xlist[[1]][4]  #NA表示沒資料，超出範圍所以沒資料
```

但是沒有交易編號，所以用names函數設定，set transaction names

paste()可以連前後數字或文字字串

c(1:5)就是建立一個數字向量內容為1,2,3,4,5

sep參數就是設定前("T")後(c(1:5))資料之間粘貼時候是用什麼隔離的方式
```R
names(xlist) = paste("T",c(1:5), sep = "") 
xlist
```

force data into transactions

因為在R中關聯規則使用的apriori()函數的資料格式為"`transactions`"，

因此採用`as()函數`強迫將list資料型態的xlist變數轉為"transactions"

資料格式#儲存在另一變數table14_1中
```R
table14_1 = as(xlist, "transactions") #transactions的雙引號一定要加 
```

查看目前table14_1的內容
```R
table14_1   #只會看到摘要資訊，例如幾筆資料，幾個欄位
```

使用`inspect()函數`可以看到真正詳細資料
```R
inspect(table14_1)
```
"transactions"資料格式中，item先出現，transactionID後出現，欄位顛倒擺

使用str()函數順便查看table14_1變數目前變數結構
```R
str(table14_1)
```


* 步驟三(Step 3.) *開始進行資料分析(analyze data)

先使用一些視覺化工具看資料輪廓generate level plots to visually inspect binary incidence matrices
```R
image(table14_1)    
```
透過資料密度圖查看資料密度，問一下自己這圖有很稀疏呢?還是很緊密呢?
```R
summary(table14_1)  
```
#一般統計資料，記得看density value是多少?

#ANS:0.6333333  與圖一致嗎?(可以手動驗算一下)


* 步驟四(Step 4.) 

可尋找到apriori理論方法中所提到的1-itemset(C1)
```R
itemFrequency(table14_1, type = "relative") #以支持度(support)呈現
itemFrequency(table14_1, type = "absolute") #以支持數量(support count)呈現
```

這一段可以儲存這些1-itemset的支持度或者支持數量資料
```R
itemFreq = itemFrequency(table14_1)
```
每個項集transaction包含item的個數
```R
Size = size(table14_1)
```
每個item出現的次數
```R
itemCount = (itemFreq/sum(itemFreq)*sum(Size))
```

* 步驟五(Step 5.) 

以長條圖(bar plot)繪製itemFrequency資料
```R
itemFrequencyPlot(table14_1, col = "lightgreen")         #有顏色
itemFrequencyPlot(table14_1, support = 0.4, col = "red") #加support條件
itemFrequencyPlot(table14_1, topN = 4, col = "lightblue")#加topN條件
```

* 步驟六(Step 6.) 

開始進行找出關聯規則. Mining Association Rules

以min_supp = 0.6, min_conf = 0.6為條件計算找出規則(target = "rules")

    規則中的前項與後項中至少都一個品項(都不能是空集合{})

    即規則長度為2(minlen=2)以上的才留下
    
```R
rules_1 = apriori(table14_1, parameter = list(support = 0.6, 
                                              confidence = 0.6,
                                              minlen=2, 
                                              target = "rules"))
                                              
#ANS:writing ... [11 rule(s)] done [0.00s].  找到11條規則
#使用inspect()函數查看一下有哪些關聯規則產生
inspect(rules_1)

#如果剛剛上述的所有參數都沒寫，R就採用內定的預設值，
#試一下!找找看有哪些認識的預設值~~
rules_2 = apriori(table14_1)  

##rules_2 = apriori(table14_1,parameter = list(minlen=2,   #刪掉空集合，因為必須兩個以上的集合
##                                            target = "rules"))   


#ANS:writing ... [36 rule(s)] done [0.00s].  找到36條規則
#使用inspect()函數查看一下有哪些關聯規則產生
inspect(rules_2)
```

* 步驟七(Step7.) 

顯示所產生的關聯規則的模型結果，看規則詳細內容使用inspect()函數
```R
inspect(table14_1) # display transactions  again

inspect(rules_1)   # display association rules in rules_1
#各位可以調整參數重新產生rules_1，
#lift值為提升度，count值為支持數量
#一直調整修正到大數據分析師與決策者BOSS找出有趣的強規則答案


inspect(rules_2)   # display association rules in rules_2
#通常實務上不建議跑沒任何參數條件的apriori方法
```




