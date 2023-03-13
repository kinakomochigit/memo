## ターゲット変数（pm25_mid）との相関係数の絶対値上位10変数を抽出 ※1番目はpm25_midであるため2番目の変数から上位10変数を抽出
```python
corr_top10 = corr_df.loc[:, ['pm25_mid']].assign(abs_value=lambda d: np.abs(d['pm25_mid']))\
                    .sort_values('abs_value', ascending=False)\
                    .iloc[1:11, :].index.tolist()
corr_top10
```
```
['no2_mid',
 'co_mid',
 'no2_max',
 'no2_min',
 'co_max',
 'so2_max',
 'ws_mid',
 'so2_mid',
 'o3_max',
 'co_min']
```
## 「churn」と他の変数との相関のグラフ

```
plt.figure(figsize=(15,8))
df2[b].corr()['churn'].sort_values(ascending = False).plot(kind='bar')
```
