# 출처 : KBA Challenge starter code (using DNN) 19f432
#### 경로 : https://www.kaggle.com/leejunyong/kba-challenge-starter-code-using-dnn-19f432


### 구매상품 다양성
```
n = tr.gds_grp_nm.nunique()
f = tr.groupby('cust_id')['gds_grp_nm'].agg([('구매상품다양성', lambda x: len(x.unique()) / n)]).reset_index()
features.append(f); f
```

### 요일 구매패턴 : 주중 1, 주말 0
#### "주말방문비율" 보다 활용성이 더 높을 것으로 예상함.
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
#### "계절별 구매비율 보다 활용성이 더 높을 것으로 예상함.
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






















