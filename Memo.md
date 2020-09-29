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
구매추세 패턴
가격선호도 (예: 고가상품구매율)
시즌 선호도
휴면(또는 이탈) 여부
Top-10 베스트 셀러(gds_grp_mclas_nm)에 대한 구매 금액/건수/여부
상품별 구매순서
주구매 요일



