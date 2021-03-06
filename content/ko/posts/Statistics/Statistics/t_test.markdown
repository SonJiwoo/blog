---
date: "2021-02-22T10:08:56+09:00"
description: t-test
draft: false
title: t 검정
weight: 3
---

# t-검정
t-검정은 모집단의 분산이나 표준편차를 알지 못할 때, 모집단을 대표하는 표본으로부터 추정된 분산이나 표준편차를 가지고 검정하는 방법이다.
![t-test](images/posts/statistics/t_test/t_test.PNG)

### 1. 단일표본 t-검정
모집단의 평균이 특정검정값과 같은지 확인하는 통계빵법이다.
![one-sample t-test](images/posts/statistics/t_test/one_sample_t_test_1.PNG)
![one-sample t-test](images/posts/statistics/t_test/one_sample_t_test_2.PNG)
![one-sample t-test](images/posts/statistics/t_test/one_sample_t_test_3.PNG)

---

### 2. 독립표본 t-검정
독립된 두 집단 간 비교하는 방법으로, 서로 다른 두 모집단으로부터 데이터가 추출되었을 때 시행한다. 


```r
# independent t-test
x1 = 3.6667
x2 = 6.0667
s1 = 2.60951
s2 = 2.37447
n1 = n2 = 15
sp = sqrt((s1^2*(n1-1) + s2^2*(n2-1)) / ((n1-1) + (n2-1)))
t = (x1-x2) / (sp*sqrt(1/n1 + 1/n2))
```
![independent t-test](images/posts/statistics/t_test/independent_t_test_1.PNG)
![independent t-test](images/posts/statistics/t_test/independent_t_test_2.PNG)
![independent t-test](images/posts/statistics/t_test/independent_t_test_3.PNG)
![independent t-test](images/posts/statistics/t_test/independent_t_test_4.PNG)
![independent t-test](images/posts/statistics/t_test/independent_t_test_5.PNG)
![independent t-test](images/posts/statistics/t_test/independent_t_test_6.PNG)

---

### 3. 대응표본 t-검정
한 집단 내 비교하는 방법으로, 하나의 모집단으로부터 데이터를 반복 추출하였을 때 시행한다.

```r
# paired t-test
xa <- c(1,10,2,6,5,2,3,4,2,1,2,2,3,4,8)
xb <- c(2,10,5,7,6,5,6,9,6,7,8,5,4,2,9)
n <- length(xa)
d_bar = (sum(xa)-sum(xb))/n
d_sd = sqrt(sum((xa-xb-d_bar)^2)/(n-1))
t = d_bar / (d_sd/sqrt(n))
```
![paired t-test](images/posts/statistics/t_test/paired_t_test_1.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_2.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_3.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_4.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_5.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_6.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_7.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_8.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_9.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_10.PNG)
![paired t-test](images/posts/statistics/t_test/paired_t_test_11.PNG)

### 참고
[1] 연세대학교 심리통계 2019-1 수업자료 
[2] https://wikidocs.net/34009
