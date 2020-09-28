# 출처 : feature engineer
#### 경로 : https://www.kaggle.com/shinyahn/feature-engineer


### 요일별 구매건수
#### "주말방문비율" 보다 활용성이 더 높을 것으로 예상함.
```
def f2(x):
    k = x.dayofweek 
    if k == 0 :
        return('월_구매건수')
    elif k== 1 :
        return('화_구매건수')
    elif k== 2 :
        return('수_구매건수')
    elif k== 3 :
        return('목_구매건수')
    elif k== 4 :
        return('금_구매건수')
    elif k== 5 :
        return('토_구매건수')
    else :
        return('일_구매건수')    
    
tr['weekday'] = pd.to_datetime(tr.sales_date).apply(f2)
f = pd.pivot_table(tr, index='cust_id', columns='weekday', values='amount', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f)
f.head()
```


### 요일별 구매금액
```
def f3(x):
    k = x.dayofweek 
    if k == 0 :
        return('월_구매액')
    elif k== 1 :
        return('화_구매액')
    elif k== 2 :
        return('수_구매액')
    elif k== 3 :
        return('목_구매액')
    elif k== 4 :
        return('금_구매액')
    elif k== 5 :
        return('토_구매액')
    else :
        return('일_구매액')    
    
tr['weekday_1'] = pd.to_datetime(tr.sales_date).apply(f3)
f = pd.pivot_table(tr, index='cust_id', columns='weekday_1', values='amount', 
                   aggfunc=np.sum, fill_value=0).reset_index()
features.append(f)
f.head()
```


### 최대 구매요일
```
def f6(x):
    k = x.dayofweek 
    if k == 0 :
        return('월')
    elif k== 1 :
        return('화')
    elif k== 2 :
        return('수')
    elif k== 3 :
        return('목')
    elif k== 4 :
        return('금')
    elif k== 5 :
        return('토')
    else :
        return('일') 


f = tr.groupby(by = 'cust_id')['sales_date'].agg([('주구매요일', lambda x : pd.to_datetime(x).apply(f6).value_counts().index[0])]).reset_index()
f = pd.get_dummies(f, columns=['주구매요일']) 
features.append(f)
f.head()
```


### 월별 구매건수
```
def f4(x):
    k = x.month
    if k==1 :
        return('1월_구매건수')
    elif k==2 :
        return('2월_구매건수')
    elif k==3 :
        return('3월_구매건수')
    elif k==4 :
        return('4월_구매건수')
    elif k==5 :
        return('5월_구매건수')
    elif k==6 :
        return('6월_구매건수')
    elif k==7 :
        return('7월_구매건수')
    elif k==8 :
        return('8월_구매건수')
    elif k==9 :
        return('9월_구매건수')
    elif k==10 :
        return('10월_구매건수')
    elif k==11 :
        return('11월_구매건수')
    else :
        return('12월_구매건수')    
    
tr['month'] = pd.to_datetime(tr.sales_date).apply(f4)
f = pd.pivot_table(tr, index='cust_id', columns='month', values='amount', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f)
f.head()
```

### 월별 구매금액
```
def f5(x):
    k = x.month
    if k==1 :
        return('1월_구매액')
    elif k==2 :
        return('2월_구매액')
    elif k==3 :
        return('3월_구매액')
    elif k==4 :
        return('4월_구매액')
    elif k==5 :
        return('5월_구매액')
    elif k==6 :
        return('6월_구매액')
    elif k==7 :
        return('7월_구매액')
    elif k==8 :
        return('8월_구매액')
    elif k==9 :
        return('9월_구매액')
    elif k==10 :
        return('10월_구매액')
    elif k==11 :
        return('11월_구매액')
    else :
        return('12월_구매액')    
    
tr['month_1'] = pd.to_datetime(tr.sales_date).apply(f5)
f = pd.pivot_table(tr, index='cust_id', columns='month_1', values='amount', 
                   aggfunc=np.sum, fill_value=0).reset_index()
features.append(f)
f.head()
```

### 주 구매월
```
def f7(x):
    k = x.month
    if k==1 :
        return('1월')
    elif k==2 :
        return('2월')
    elif k==3 :
        return('3월')
    elif k==4 :
        return('4월')
    elif k==5 :
        return('5월')
    elif k==6 :
        return('6월')
    elif k==7 :
        return('7월')
    elif k==8 :
        return('8월')
    elif k==9 :
        return('9월')
    elif k==10 :
        return('10월')
    elif k==11 :
        return('11월')
    else :
        return('12월')    
    
f = tr.groupby(by = 'cust_id')['sales_date'].agg([('주구매월', lambda x : pd.to_datetime(x).apply(f7).value_counts().index[0])]).reset_index()
f = pd.get_dummies(f, columns=['주구매월']) 
features.append(f)
f.head()
```


### 월중 구매액
```
def f8(x):
    k = x.day
    if 1<= k <= 10 :
        return('월초_구매액')
    elif 10 < k <= 20 :
        return('월중순_구매액')
    else :
        return('월말_구매액')    
    
tr['day'] = pd.to_datetime(tr.sales_date).apply(f8)
f = pd.pivot_table(tr, index='cust_id', columns='day', values='amount', 
                   aggfunc=np.sum, fill_value=0).reset_index()
features.append(f)
f.head()
```

### 월중 구매건수
```
def f9(x):
    k = x.day
    if 1<= k <= 10 :
        return('월초_구매건수')
    elif 10 < k <= 20 :
        return('월중순_구매건수')
    else :
        return('월말_구매건수')    
    
tr['day_1'] = pd.to_datetime(tr.sales_date).apply(f9)
f = pd.pivot_table(tr, index='cust_id', columns='day_1', values='amount', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f)
f.head()
```

### 월중 최대구매일
```
def f10(x):
    k = x.day
    if 1<= k <= 10 :
        return('월초')
    elif 10 < k <= 20 :
        return('월중순')
    else :
        return('월말')    
    
f = tr.groupby(by = 'cust_id')['sales_date'].agg([('주구매일', lambda x : pd.to_datetime(x).apply(f10).value_counts().index[0])]).reset_index()
f = pd.get_dummies(f, columns=['주구매일']) 
features.append(f)
f.head()
```


### 특이정보 : 성별을 아는 경우 만들 수 있는 파생변수 : 통계적 개념으로 무슨 의미인지 곰곰히 생각해 볼 것 !!!!!!!!!!
기존 데이터의 여:남 비율이 6.5:3.5이므로 
주구매코너별로 동등하게 샀다면 기대되는 gender의 기댓값은 0.3이다. 
따라서 gender의 값이 0.3이 넘으면 남자가 그 코너를 주로 이용할 가능성이 높고, 
작다면 여자가 그 코너를 자주 이용할 가능성이 높다. 
이를 활용하여, 남여 구분을 확실히 해주는 feature로 '주구매_성별'을 생성하여 남자가 더 자주 이용하는 코너에는 1, 여자가 자주 이용하는 코너에는 -1을 부여
```
f = tr.groupby('cust_id')['gds_grp_mclas_nm'].agg([('주구매코너', lambda x: x.value_counts().index[0])]).reset_index()
f['주구매코너_성별']=np.where(f['주구매코너'].isin(male_preferred_corner),1,0)
f['주구매코너_성별']=np.where(f['주구매코너'].isin(female_preferred_corner),-1,f['주구매코너_성별'])
f.drop(labels = ["주구매코너"],axis = 1,inplace=True)
features.append(f)
f.head()
```
```
f = tr.groupby('cust_id')['store_nm'].agg([('주구매지점', lambda x: x.value_counts().index[0])]).reset_index()
f['주구매지점_성별']=np.where(f['주구매지점'].isin(male_preferred_store),1,0)
f['주구매지점_성별']=np.where(f['주구매지점'].isin(female_preferred_store),-1,f['주구매지점_성별'])
f.drop(labels = ["주구매지점"],axis = 1,inplace=True)
features.append(f)
f.head()
```








