---
title: "Facebook粉絲團分析（分析專頁：朱立倫）"
output: github_document
---

分析朱立倫粉絲團貼文，資料分析區間為2016/01/01至2016/04/09

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

##讀取朱立倫粉絲團資料
```{r cars}
if (!require('Rfacebook')){
    install.packages("Rfacebook")
    library(Rfacebook)
}

token<-'1710211855888688|plQxOIzb1uZM4eHQ_3HUKSOs88E'
totalPage<-NULL
lastDate<-Sys.Date()
DateVectorStr<-as.character(seq(as.Date("2016-01-01"),lastDate,by="5 days"))
for(i in 1:(length(DateVectorStr)-1)){
    tempPage<-getPage("llchu", token,
                      since = DateVectorStr[i],until = DateVectorStr[i+1])
    totalPage<-rbind(totalPage,tempPage)
}
nrow(totalPage)
```
2016/01/01 至 2016/04/09，朱立倫粉絲團一共有 129 篇文章



## 每日發文數分析
分析朱立倫粉絲團每天的發文數，由於日期格式不一，故先將其轉換為台灣時區之後再做統計
```{r cars}
totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                 format = "%Y-%m-%dT%H:%M:%S+0000", 
                                 tz = "GMT") 
totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                            tz = "Asia/Taipei") 
totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
PostCount<-aggregate(id~dateTPE,totalPage,length)
library(knitr)
kable(head(PostCount[order(PostCount$id,decreasing = T),]))
```
根據資料指出，2016-01-12(禮拜二)的發文數最多，一共有七篇

第一篇為"【台灣最美的風景】"

第二篇為"感謝大家給我們鼓勵"

第三篇為"一『旗』去投票！"

第四篇為"在社群上不敢表達想法的你"

第五篇為"一旗換頭貼！"

第六篇為"教孩子希望"

第七篇為"連著幾天，小倩都會陪著立委候選人去市場拜票"

2016-01-13、2016-01-14和2016-01-15居次

1/13有:

"再過3天，中台灣的鄉親可以用選票扭轉遺憾"+"在社群上感到特別孤單的你"+"每一雙手的打拚"+"今天是經國先生逝世28週年"+"【守護，就是力量】"

1/14有:

"他們會捍衛養豬戶的權益"+"或許你不熱衷政治"+"翻轉南台灣"+"聽聞小桃阿嬤過世的消息"+"【支持，就是力量】"

1/15有:

"去年底，來台旅遊的國際觀光客突破一千萬人"+"捍衛國旗，支持中華民國，力挺周子瑜！"+"【你，就是力量】"+"雨中，我們看見希望！"+"對於一個16歲的年輕人"




## 每日Likes數分析
分析朱立倫粉絲團每日的點讚數並統計
```{r cars}
LikesCount<-aggregate(likes_count~datetime,totalPage,mean)
library(knitr)
kable(head(LikesCount[order(LikesCount$likes_count,decreasing = T),]))
```
根據資料指出，2016-01-16"對不起，朱立倫讓各位失望了！"點讚數最多

其次為2016-01-15的"對於一個16歲的年輕人"

第三為2016-01-09的"中華民國，出發！"

第四為2016-01-12的"一旗換頭貼！"

第五為2016-01-06的"南台大地震"

第六為2016-01-11的"這位是我的阿嬤"




## 每日Comments數分析
分析朱立倫粉絲團每日的討論數並統計
```{r cars}
CommentsCount<-aggregate(comments_count~datetime,totalPage,mean)
library(knitr)
kable(head(CommentsCount[order(CommentsCount$comments_count,decreasing = T),]))
```
第一多為2016-01-15的"對於一個16歲的年輕人"

第二多為2016-01-16的"對不起，朱立倫讓各位失望了！"

第三多為2016-01-16的"看到昨晚的畫面"

第四多為2016-01-18的"敗選請辭這是我的承諾"

第五多為2016-01-15的"捍衛國旗"

第六多為2016-01-09的"中華民國，出發！"




## 每日Shares數分析
分析朱立倫粉絲團每日的分享數並做統計
```{r cars}
ShareCount<-aggregate(shares_count~datetime,totalPage,mean)
library(knitr)
kable(head(ShareCount[order(ShareCount$shares_count,decreasing = T),]))
```
第一多為2016-01-15的"對於一個16歲的年輕人"

第二多為2016-01-01的"二零一六已來到"

第三多為2016-01-16的"對不起，朱立倫讓各位失望了！"

第四多為2016-01-15的"捍衛國旗"

第五多為2016-01-12的"在社群上不敢表達想法的你"

第六多為2016-01-06的"【1/9 一“旗”來遊行】 青天白日滿地紅，是先人的努力"





