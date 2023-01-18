---
layout: post
title:  pandas Normalization(정규화)
date:   2022-11-07 21:37
image:  
tags:   Pandas
---

# normalization
#### - 데이터의 범위(scale)를 0~1 사이의 값으로 바꾸는 것
<br/>

##### 개념
1. <mark style='background-color: #fff5b1'> MinMaxScaler(최대-최소 정규화) </mark>
    * MinMaxScaler는 정규화 종류 중 가장 많이 사용되는 방법이다.
    * Xi값에 최소값을 빼고 그 다음에 최대값에서 최소값을 뺀 값으로 나누는 방법이다.

![]({{site.baseurl}}/images/minmaxscaler.png)




<!-- 이미지 크기 지정할 때 -->
<!-- <img src="/images/minmaxscaler.png" width="40%" height="30%"> -->
``` py
import pandas as pd
import numpy as np
from sklearn.preprocessing import MinMaxScaler

#0에서 20사이의 데이터를 5X3 사이즈로 랜덤으로 생성
df = pd.DataFrame(np.random.randint(0,20,size=(5, 3)), columns = list('ABC'))
print(df,"\n")

#MinMaxScaler객체 생성
scaler = MinMaxScaler()

#MinMaxScaler로 데이터 변환
df_scaled = scaler.fit_transform(df)

#transform()을 하면 데이터가 numpy array로 변환이 되어 dataframe으로 변환
df_df_scaled = pd.DataFrame(data=df_scaled, columns = list('ABC'))
print(df_df_scaled)
```

<br/>

##### 츨력
![]({{site.baseurl}}/images/minmaxscaler_print.png)

##### 설명
max (A) = 17, mix(A) = 1

1. A의 0번 행 같은 경우는 A(17, 9, 1, 2, 4)중에서 가장 큰 값이므로 1이 나온다 

        (17-1) / (17-1) = 1

2. A의 1번 행 같은 경우는

        (9-1) / (17-1) = 0.5

이러한 방법으로 df의 데이터들을 MinMaxScaler를 하는 것이다.

***