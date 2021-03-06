---
collapsible: false
date: "2021-02-21T10:08:56+09:00"
description: null
draft: false
title: purrr
weight: 2
---

# purrr 패키지 훑어보기
purrr는 R에서 깔끔하게 반복 작업 처리하는 패키지입니다. Purrr 을 이용하면 반복작업을 Apply family 에 비해 더욱 직관적이고 쉽게 할 수 있습니다. purrr는 고양이 울음소리와 R의 합성어로, 로고는 아래와 같습니다.

```r
library(tidyverse)
```
![purrr logo](images/posts/r/purrr/purrr.jpg)

## 목차
1. map, map2
2. pmap, invoke_map
3. rerun
4. every, some, none
5. reduce, accumulate

---

#### map, map2
{{< expand "map, map2 예시" >}}

```r
num <- c(1,2,4,5,7)
num2 <- c(3,5,6,8,9)

#list
map(num, function(x){x^2})
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 4
## 
## [[3]]
## [1] 16
## 
## [[4]]
## [1] 25
## 
## [[5]]
## [1] 49
```

```r
map2(num, num2, sum)
```

```
## [[1]]
## [1] 4
## 
## [[2]]
## [1] 7
## 
## [[3]]
## [1] 10
## 
## [[4]]
## [1] 13
## 
## [[5]]
## [1] 16
```

```r
#numeric vector
map_dbl(num, function(x){x^2})
```

```
## [1]  1  4 16 25 49
```

```r
map2_dbl(num, num2, sum)
```

```
## [1]  4  7 10 13 16
```
{{< /expand >}}

{{<expand "map 실전활용- iris data">}}

```r
n_iris <- iris %>%
  group_by(Species) %>%
  nest()

mod_fun <- function(df){
  lm(Sepal.Length ~ ., data = df)
  }
  
m_iris <- n_iris %>%
  mutate(model = map(data, mod_fun))

b_fun <- function(mod){
  coefficients(mod)[[1]]
}

m_iris %>% transmute(Species, beta = map_dbl(model, b_fun))
```

```
## # A tibble: 3 x 2
## # Groups:   Species [3]
##   Species     beta
##   <fct>      <dbl>
## 1 setosa     2.35 
## 2 versicolor 1.90 
## 3 virginica  0.700
```
{{</expand>}}
![map](images/posts/r/purrr/map_example.PNG)

---

#### pmap, invoke_map
{{< expand "pmap, invoke_map 예시" >}}

```r
x <- list(3, 6, 9)
y <- list(10, 21, 30)
z <- list(100, 200, 300)

# pmap은 3개 이상의 리스트일 때 사용한다.
pmap(list(x, y, z), sum)
```

```
## [[1]]
## [1] 113
## 
## [[2]]
## [1] 227
## 
## [[3]]
## [1] 339
```

```r
# invoke_map은 각각의 리스트에 다른 함수를 적용시키고 싶을 때 활용한다.
invoke_map(list(runif, rnorm), list(list(n = 10), list(n = 5)))
```

```
## [[1]]
##  [1] 0.07250356 0.88643385 0.53457501 0.45493290 0.95970419 0.57638199
##  [7] 0.62800763 0.63266467 0.64451000 0.77471082
## 
## [[2]]
## [1]  0.1348207 -0.5144428 -0.8763132  0.8955774  0.3386345
```
{{< /expand >}}

---

#### rerun
rerun은 샘플 데이터를 **리스트** 형식으로 형성하는 데에 효율적인 방법이다.
{{< expand "rerun 예시" >}}

```r
# 예시
set.seed(2021)
a <- 10 %>% 
  rerun(rnorm(5))
a
```

```
## [[1]]
## [1] -0.1224600  0.5524566  0.3486495  0.3596322  0.8980537
## 
## [[2]]
## [1] -1.92256952  0.26174436  0.91556637  0.01377194  1.72996316
## 
## [[3]]
## [1] -1.0822049 -0.2728252  0.1819954  1.5085418  1.6044701
## 
## [[4]]
## [1] -1.841476  1.623310  0.131389  1.481122  1.513318
## 
## [[5]]
## [1] -0.9424433 -0.1856850 -1.1011246  1.2081153 -1.6249385
## 
## [[6]]
## [1]  0.10537833 -1.45544335 -0.35401614 -0.09370004  1.10066863
## 
## [[7]]
## [1] -1.9638251 -1.4479444  1.0194434 -1.4214171 -0.6045321
## 
## [[8]]
## [1] -1.58347390 -1.28593235 -1.45468488 -0.08707112  0.50473644
## 
## [[9]]
## [1]  0.11638871  1.76021373 -0.34511646  2.12000016 -0.03437749
## 
## [[10]]
## [1] -0.7921541  1.4755152 -0.7255572  0.3123790  0.6919641
```

```r
# 위 함수는 아래의 함수와 같은 결과를 산출함을 알 수 있다.
set.seed(2021)
b <- list()
for(i in 1:10){
  b[[i]] <- rnorm(5)
  print(b[i])
}
```

```
## [[1]]
## [1] -0.1224600  0.5524566  0.3486495  0.3596322  0.8980537
## 
## [[1]]
## [1] -1.92256952  0.26174436  0.91556637  0.01377194  1.72996316
## 
## [[1]]
## [1] -1.0822049 -0.2728252  0.1819954  1.5085418  1.6044701
## 
## [[1]]
## [1] -1.841476  1.623310  0.131389  1.481122  1.513318
## 
## [[1]]
## [1] -0.9424433 -0.1856850 -1.1011246  1.2081153 -1.6249385
## 
## [[1]]
## [1]  0.10537833 -1.45544335 -0.35401614 -0.09370004  1.10066863
## 
## [[1]]
## [1] -1.9638251 -1.4479444  1.0194434 -1.4214171 -0.6045321
## 
## [[1]]
## [1] -1.58347390 -1.28593235 -1.45468488 -0.08707112  0.50473644
## 
## [[1]]
## [1]  0.11638871  1.76021373 -0.34511646  2.12000016 -0.03437749
## 
## [[1]]
## [1] -0.7921541  1.4755152 -0.7255572  0.3123790  0.6919641
```

```r
for(i in 1:10){
  print(a[[i]] == b[[i]])
}
```

```
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
## [1] TRUE TRUE TRUE TRUE TRUE
```

```r
# 참고로, base에 있는 replicate와 어떻게 다른지 한번 살펴보자!
replicate(10, rnorm(5))
```

```
##             [,1]       [,2]        [,3]       [,4]        [,5]       [,6]
## [1,] -0.50029080  0.1037663  0.01604353 -0.9836134 -0.34823176 -0.2369450
## [2,] -2.25586935  0.4272891 -0.18536431  0.5650808 -0.04298997 -0.9991415
## [3,]  0.04374133 -0.1704815  0.39193326  1.6167519 -1.39755396 -1.3925426
## [4,] -0.36881809 -1.5491403 -0.75671092 -0.2519641  1.49021633  0.9820053
## [5,] -0.96022240 -1.5055999  0.23141761 -1.0558786 -1.03938712  0.3609409
##            [,7]       [,8]        [,9]      [,10]
## [1,] -0.3375092 -1.2400271  0.81061837 -0.1220018
## [2,] -0.6433876  0.5339593 -0.29366457 -0.6467737
## [3,] -2.1668853 -1.5882648 -0.05345832 -0.8678583
## [4,]  0.6332890 -0.9909645  0.73518450 -0.5087003
## [5,] -0.1449141  0.4832608  0.01498499 -2.0775844
```

```r
typeof(a)
```

```
## [1] "list"
```

```r
typeof(replicate(10, rnorm(5)))
```

```
## [1] "double"
```
{{< /expand >}}

#### every, some, none
**리스트**형식을 summarise하는 데에 효율적인 방법이다.
{{<expand "every, some, none 예시">}}

```r
y <- list(0:10, 5.5)
y
```

```
## [[1]]
##  [1]  0  1  2  3  4  5  6  7  8  9 10
## 
## [[2]]
## [1] 5.5
```

```r
y %>% every(is.numeric)
```

```
## [1] TRUE
```

```r
y %>% every(is.integer)
```

```
## [1] FALSE
```

```r
y %>% some(is.integer)
```

```
## [1] TRUE
```

```r
y %>% none(is.character)
```

```
## [1] TRUE
```
{{</expand>}}

#### reduce, accumulate
함수를 재귀적으로(recursively) 적용시키는 효율적인 방법이다.
{{<expand "reduce, accumulate 예시">}}

```r
#1. reduce:
reduce(1:10, sum)
```

```
## [1] 55
```

```r
#2. accumulate
accumulate(1:10, sum)
```

```
##  [1]  1  3  6 10 15 21 28 36 45 55
```

```r
#reduce 응용 버전
paste2 <- function(x, y, sep = ".") paste(x, y, sep = sep)
letters[1:4] %>% reduce(paste2)
```

```
## [1] "a.b.c.d"
```
{{</expand>}}
