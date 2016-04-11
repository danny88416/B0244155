---
title: "NBA 2014-2015球季 各隊分析"
output: github_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

```{r echo=T}
install.packages("SportsAnalytics")
library(SportsAnalytics)
NBA1415<-fetch_NBAPlayerStatistics("14-15")
names(NBA1415)
head(NBA1415)
```

##各隊最辛苦的球員
計算依據為出戰分鐘數最多的球員

```{r echo=T}
MaxPoint=aggregate(TotalPoints~Team,NBA1415,max)
NBA1415MaxPoint=merge(NBA1415,MaxPoint)
Output=NBA1415MaxPoint[order(NBA1415MaxPoint$TotalMinutesPlayed,decreasing = T),c("Team","Name","TotalMinutesPlayed")]
library(knitr)
kable(Output,digits = 2)
```

##各隊得分王

```{r echo=T}
MaxPoint<-aggregate(TotalPoints~Team,NBA1415,max)
NBA1415MaxPoint<-merge(NBA1415,MaxPoint)
output<-NBA1415MaxPoint[order(NBA1415MaxPoint$TotalPoints,decreasing=T),c("Team","Name","TotalPoints")]
library(knitr)
kable(output, digit=2)
```

##各隊最有效率的球員

```{r echo=T}
NBA1415$Efficiency<-
    round(NBA1415$TotalPoints/NBA1415$TotalMinutesPlayed,digits=3)
EfficiencyMax<-merge(NBA1415,aggregate(Efficiency~Team,NBA1415,max))
EfficiencyMax[order(EfficiencyMax$Efficiency,decreasing=T),
           c("Team","Name","Position","Efficiency","TotalPoints","TotalMinutesPlayed")]

```

##各隊三分球出手最準的球員

```{r echo=T}

NBA1415$ThreesP<-
    round(NBA1415$ThreesMade/NBA1415$ThreesAttempted,digits=3)
ThreesPMax<-merge(NBA1415,aggregate(ThreesP~Team,NBA1415,max))
ThreesPMax[order(ThreesPMax$ThreesP,decreasing=T),
           c("Team","Name","Position","ThreesP","ThreesMade")]

```


