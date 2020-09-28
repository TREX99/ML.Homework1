# 출처 : DNN(ensemble)
#### 경로 : https://www.kaggle.com/yousang118/dnn-ensemble


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
```

### 거래기간 : 최종거래일 - 최초거래일
```
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
```


### 한 번이라도 이용한 코너 수 : 구매 장르 다양성
```
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
#### "평균구매액" 보다 의미 있을 것으로 판단 
```
f = tr.groupby(['cust_id','dym'])['amount'].agg([('total', 'sum')]).reset_index()
f = f.groupby('cust_id')['total'].agg([('일 평균 구매액', 'mean')]).astype(int).reset_index()
features.append(f)
f
```









