#### 고객번호 : features = pd.DataFrame({'cust_id': tr.cust_id.unique()})
#### 총구매액 : tr.groupby('cust_id')['amount'].agg([('총구매액',np.sum)])
#### 구매건수 : tr.groupby('cust_id')['amount'].agg([('구매건수', np.size)])
#### 평균구매액 : tr.groupby('cust_id')['amount'].agg([('평균구매액', lambda x: np.round(np.mean(x)))])
#### 최대구매액 : tr.groupby('cust_id')['amount'].agg([('최대구매액', np.max)])
#### 구매상품종류 : f = tr.groupby('cust_id')['goods_id'].agg([('구매상품종류1', lambda x: x.nunique())])
#### 구매상품종류 : f = tr.groupby('cust_id')['gds_grp_nm'].agg([('구매상품종류2', lambda x: x.nunique())])
#### 구매상품종류 : f = tr.groupby('cust_id')['gds_grp_mclas_nm'].agg([('구매상품종류3', lambda x: x.nunique())])
#### 내점일수 : lambda x: x.str[:10].nunique())
#### 구매주기 : lambda x: int((x.astype('datetime64').max() - x.astype('datetime64').min()).days / x.str[:10].nunique()))
#### 주말방문비율 : lambda x: np.mean(pd.to_datetime(x).dt.dayofweek>4))
#### 봄-구매비율 : lambda x: np.mean(pd.to_datetime(x).dt.month.isin([3,4,5])))
#### 여름-구매비율 : lambda x: np.mean(pd.to_datetime(x).dt.month.isin([6,7,8])))
#### 가을-구매비율 : lambda x: np.mean(pd.to_datetime(x).dt.month.isin([9,10,11])))
#### 겨울-구매비율 : lambda x: np.mean(pd.to_datetime(x).dt.month.isin([1,2,12])))
#### 환불금액 : f = tr[tr.amount < 0].groupby('cust_id')['amount'].agg([('환불금액', lambda x: x.sum() * -1)
#### 환불건수 : f = tr[tr.amount < 0].groupby('cust_id')['amount'].agg([('환불건수', np.size)
#### 내점 당 구매액 = tr.groupby('cust_id')['amount'].sum() / visits
#### 내점 당 구매건수 = tr.groupby('cust_id')['amount'].size() / visits
#### 최근 3개월 구매금액, 건수
#### 최근 6개월 구매금액, 건수
#### 최근 9개월 구매금액, 건수
#### 최근 12개월 구매금액, 건수
#### 주구매상품 : lambda x: x.value_counts().index[0])
#### 주구매지점 : lambda x: x.value_counts().index[0])

<br><br><br><br>
### 구매추세 패턴
```
```
### 가격선호도 (예: 고가상품구매율)
```
```
### 시즌 선호도
```
```
### 휴면(또는 이탈) 여부
```
```
### Top-10 베스트 셀러(gds_grp_mclas_nm)에 대한 구매 금액/건수/여부
```
```
### 상품별 구매순서
```
```
### 주구매 요일
```
```

### 평균구매상품종류
```
df =tr.groupby(['custid','goodcd'])['tot_amt'].agg([('good_count', 'count')]).reset_index()
f = df.groupby(['custid'])['good_count'].agg([('good_count_mean', 'mean')]).reset_index()
features.append(f)
```

### 일평균구매액
```
test2 = tr.groupby(['sales_date','custid'])['tot_amt'].agg([('day_amt', 'sum')]).reset_index()
test2 = test2.groupby(['custid'])['day_amt'].agg([('일평균구매액', 'mean')]).reset_index()
features.append(test2)
```

### 일평균구매건
```
df = tr.groupby(['sales_date','custid'])['custid'].agg([('day_visit', 'count')]).reset_index()
f = df.groupby(['custid'])['day_visit'].agg([('일평균구매건', 'mean')]).reset_index()
features.append(f)
```

### 최소구매액
```
f = tr.groupby('cust_id')['amount'].agg([('최소구매액', 'min')]).reset_index()
features.append(f)
f
```

### 구매액 중간값 (median)
```
f = tr.groupby('cust_id')['amount'].agg([('구매액 중간값', 'median')]).reset_index()
features.append(f)
f
```

### 주거래 지점별 거래수
```
tr['ones']= np.ones(len(tr))
store = pd.pivot_table(tr, values='ones', index='cust_id', columns='store_nm',aggfunc=sum,fill_value=0)
# store['주거래 지점'] = store.idxmax(axis=1)
f = store.reset_index()
features.append(f)
```

### 월평균 구매액
```
tr['ym'] = tr.tran_date.str[:4] + tr.tran_date.str[5:7]
dm_piv = pd.pivot_table(tr, values='amount', index='cust_id', columns='ym', aggfunc=np.mean, fill_value=0)
dm_piv = dm_piv.reset_index()
f = dm_piv
features.append(f)
```

### 1년 중 거래하는 개월 수 : 얼마나 자주 거래하는지 근사적 확인. 예)12인 경우 매월마다 거래하는 고객
```
tr['ym'] = tr.tran_date.str[:4] + tr.tran_date.str[5:7]
dm_piv = pd.pivot_table(tr, values='amount', index='cust_id', columns='ym', aggfunc=np.mean, fill_value=0)
f= pd.DataFrame(dm_piv)
f=f[f>0]
f['1년 중 거래하는 개월 수'] = f.nunique(axis=1)
f = f['1년 중 거래하는 개월 수'] 
f = f.reset_index()
features.append(f)
```

### 월별 구매 건수
```
tr['ym'] = tr.tran_date.str[:4] + tr.tran_date.str[5:7]
dm_piv = pd.pivot_table(tr, values='ones', index='cust_id', columns='ym', aggfunc=sum, fill_value=0)
dm_piv = dm_piv.reset_index()
f = dm_piv
features.append(f)
```

### 최대 구매 건수 월
```
tr['ym'] = tr.tran_date.str[:4] + tr.tran_date.str[5:7]
dm_piv = pd.pivot_table(tr, values='ones', index='cust_id', columns='ym', aggfunc=sum, fill_value=0)
f= pd.DataFrame(dm_piv)
f['최대 거래 빈도 월'] = f.idxmax(axis=1)
f = f['최대 거래 빈도 월']
f= pd.to_numeric(f, downcast='float')
f = f.reset_index()
features.append(f)
```

### 최초 구매일
```
tr['dym'] = tr.tran_date.str[:4] + tr.tran_date.str[5:7] + tr.tran_date.str[8:10]
f = tr.groupby('cust_id')['dym'].agg([('최초 구매일', 'min')]).reset_index()
거래기간 : 최종거래일 - 최초거래일
import datetime
f1 = tr.groupby('cust_id')['dym'].agg([('최종 구매일', 'max')]).reset_index()
f2 = tr.groupby('cust_id')['dym'].agg([('최초 구매일', 'min')]).reset_index()
f3 = pd.merge(f1,f2,on='cust_id')
f3['max-min'] = np.zeros(len(f1)) 

for k in range(len(f3)):
    asd = datetime.datetime.strptime(f3.iloc[k,1],"%Y%m%d")-datetime.datetime.strptime(f3.iloc[k,2],"%Y%m%d")
    f3.iloc[k,3] = asd.days
    
features.append(f3)
del f3['최종 구매일']
del f3['최초 구매일']
f3
```

### 거래주기 : 총 거래 기간을 내점 일수로 나눠 거래주기를 구한다. 며칠마다 거래하는지 판단
```
f3 = pd.merge(f1,f2,on='cust_id')
f3['max-min'] = np.zeros(len(f1)) 

for k in range(len(f3)):
    asd = datetime.datetime.strptime(f3.iloc[k,1],"%Y%m%d")-datetime.datetime.strptime(f3.iloc[k,2],"%Y%m%d")
    f3.iloc[k,3] = asd.days
    
f = tr.groupby('cust_id')['dym'].agg([('내점일 수', 'nunique')]).reset_index()
features.append(f)

f4 = pd.merge(f3,f, on='cust_id')
f4['거래주기'] = f4['max-min']/f4['내점일 수']
del f4['최종 구매일']
del f4['최초 구매일']
del f4['max-min']
del f4['내점일 수']
#features.append(f4)
f4
```

### 충성도 : 한 번이라도 이용한 지점 수를 측정해 충성도 체크. 만약 1 이라면 하나의 지점만 이용하는 단골고객
```
f = tr.groupby('cust_id')['store_nm'].agg([('한번이라도 이용한 지점 수', 'nunique')]).reset_index()
features.append(f)
한 번이라도 이용한 코너 수 : 구매 장르 다양성
f = tr.groupby('cust_id')['gds_grp_nm'].agg([('한번이라도 이용한 코너 수', 'nunique')]).reset_index()
features.append(f)
f
```

### 브랜드 충성도 : 4군데 코너에서 4군데의 지점만 이용한다면 하나의 단골 브랜드만 이용함. (1에 가까울수록 높음)
```
f1 = tr.groupby('cust_id')['gds_grp_mclas_nm'].agg([('한번이라도 이용한 코너 수', 'nunique')]).reset_index()
f2 = tr.groupby('cust_id')['gds_grp_nm'].agg([('한번이라도 이용한 브랜드 수', 'nunique')]).reset_index()
f3 = pd.merge(f1,f2,on='cust_id')
f3['브랜드 충성도'] = f3['한번이라도 이용한 브랜드 수']/f3['한번이라도 이용한 코너 수']
del f3['한번이라도 이용한 브랜드 수']
del f3['한번이라도 이용한 코너 수']
features.append(f3)
f3
```

### 일 평균 구매일 : 동일날짜별로 구매액 합계 후 평균
```
"평균구매액" 보다 의미 있을 것으로 판단
f = tr.groupby(['cust_id','dym'])['amount'].agg([('total', 'sum')]).reset_index()
f = f.groupby('cust_id')['total'].agg([('일 평균 구매액', 'mean')]).astype(int).reset_index()
features.append(f)
f
```

### 구매상품 다양성
```
n = tr.gds_grp_nm.nunique()
f = tr.groupby('cust_id')['gds_grp_nm'].agg([('구매상품다양성', lambda x: len(x.unique()) / n)]).reset_index()
features.append(f); f
```

### 요일 구매패턴 : 주중 1, 주말 0
##### "주말방문비율" 보다 활용성이 더 높을 것으로 예상함.
```
def weekday(x):
    w = x.dayofweek 
    if w < 4:
        return 1 # 주중
    else:
        return 0 # 주말
f = tr.groupby(by = 'cust_id')['sales_date'].agg([('요일구매패턴', lambda x : pd.to_datetime(x).apply(weekday).value_counts().index[0])]).reset_index()
features.append(f); f
```

### 계절별 구매건수
##### "계절별 구매비율 보다 활용성이 더 높을 것으로 예상함.
```
def f1(x):
    k = x.month
    if 3 <= k <= 5 :
        return('봄-구매건수')
    elif 6 <= k <= 8 :
        return('여름-구매건수')
    elif 9 <= k <= 11 :    
        return('가을-구매건수')
    else :
        return('겨울-구매건수')    
    
tr['season'] = pd.to_datetime(tr.sales_date).apply(f1)
f = pd.pivot_table(tr, index='cust_id', columns='season', values='amount', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f); f
```


### 요일별 구매건수
##### "주말방문비율" 보다 활용성이 더 높을 것으로 예상함.
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

### 이용지점 다양성: 이용한 서로다른 매장 수
```
n = 4
f = tr.groupby('custid')['str_nm'].agg([('매장이용다양성', lambda x: len(x.unique()) / n)]).reset_index()
features.append(f); f
```

### 요일별 구매건수 - 요일을 새로운 기준으로 구분해봄
```
def f2(x):
    k = x.dayofweek
    if k <= 2 :
        return('월화수_구매건수')
    elif 3 <= k < 5 :
        return('목금_구매건수')
    elif 5 <= k < 6 :
        return('토_구매건수')
    else :
        return('일_구매건수')    
    
tr['요일2'] = pd.to_datetime(tr.sales_date).apply(f2)
f = pd.pivot_table(tr, index='custid', columns='요일2', values='tot_amt', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f); f
```

### 시간대별 구매건수: 12시 이전 / 12~2시 / 2~5시 / 5~6시 / 6시 이후
```
def f2(x):
    if 901 <= x < 1200 :
        return('12시 이전_구매건수')
    elif 1200 <= x < 1400 :
        return('12~2시_구매건수')
    elif 1400 <= x < 1600 :
        return('2~4시_구매건수')
    elif 1600 <= x < 1800 :
        return('4~6시_구매건수')
    else :
        return('6시이후_구매건수')  

tr['timeslot2'] = tr.sales_time.apply(f2)
```

### 구입 지점 빈도값 도출
```
f = pd.pivot_table(tr, index='custid', columns='str_nm', values='tot_amt', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f); f
```

### 구매 파트 변수의 각 파트의 빈도값 도출
```
f = pd.pivot_table(tr, index='custid', columns='part_nm', values='tot_amt', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f); f
```

### 구매제품 변수의 각 파트의 빈도값 도출
```
f = pd.pivot_table(tr, index='custid', columns='buyer_nm', values='tot_amt', 
                   aggfunc=np.size, fill_value=0).reset_index()
features.append(f); f
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

### 최적의 성별예측 모형에 사용하는 feature
```
from sklearn.model_selection import train_test_split
feature_name = ['VISIT_CNT','SALES_AMT','USABLE_INIT','BIRTH_SL_CODE','ACC_PNT','USED_PNT','USABLE_PNT',
                'SMS_CODE','MEMP_TP_CODE']
label_name = ['GENDER']

X_train,X_test,y_train,y_test = train_test_split(train_df[feature_name],train_df[label_name],test_size=.25,random_state=0)

prediction_data = pred_df[feature_name]

print(X_train.shape,y_train.shape,X_test.shape,y_test.shape)
```
