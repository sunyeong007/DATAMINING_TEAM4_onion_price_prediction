# DATAMINING TEAM5 
# Onion Price Prediction


```bash
├── 전처리 합치기
│   ├── 경유전처리
│   ├── 기상전처리
│   ├── boxEnlarge.py
│   ├── dataset.py
│   ├── gaussian.py
│   ├── imgaug.py
│   ├── imgproc.py
├── 2020년이후_1개월단위예측
│   ├── pseudo_label
│   │   ├── make_charbox.py
│   │   └── watershed.py

├── 2013년이후_6개월단위예측
│   └── mseloss.py
├── eda.ipynb
```


# How to Use


# Index


# ReadMe 파일 - 전처리 파트

---

- 양파 도매가격 전처리

`'상품_양파_{}년_일별.xlsx'`(2013~2023년), `'중품_양파_{}년_일별.xlsx'`(2013~2023년) 데이터 `new_df **=** onion_df**.**groupby('date_time')['평균']**.**mean()**.**reset_index()` 코드사용해서 상품 중품 평균 계산후 새로운 column 형성→ 첫 행만 null값 확인후 `new_onion_df**.**loc[0,'상품중품평균가격'] **=** new_onion_df['상품중품평균가격'][1]` 다음행 가격과 동일하게 처리→`'onion_price_data.xlsx'` 데이터 추출 

- 기상환경 전처리

`'weather_after2013.xlsx'` 데이터에 `weather_df['평균풍속(m/s)'] **=** weather_df[['평균풍속(m/s)_통영', '평균풍속(m/s)_울진', '평균풍속,` `weather_df['일조율(%)'] **=** weather_df[['일조율(%)_통영', '일조율(%)_울진', '일조율(%)_여수']]**.**mean(axis**=**1)` 사용하여 지역별 평균풍속, 일조율 새로운 column 형성후 병합→`'weather_df_after2013.xlsx'` 데이터로 추출 

`'weather_after2020.xlsx'` 데이터에 `weather_df['평균풍속(m/s)'] **=** weather_df[['평균풍속(m/s)_통영', '평균풍속(m/s)_울진', '평균풍속,` `weather_df['일조율(%)'] **=** weather_df[['일조율(%)_통영', '일조율(%)_울진', '일조율(%)_여수']]**.**mean(axis**=**1)` 사용하여 지역별 평균풍속, 일조율 새로운 column 형성후 병합→`'weather_df_after2020.xlsx'` 데이터로 추출 

```
weather_df['평균풍속(m/s)']= weather_df[['평균풍속(m/s)_통영', '평균풍속(m/s)_울진', '평균풍속(m/s)_여수']].mean(axis=1)
weather_df['일조율(%)']= weather_df[['일조율(%)_통영', '일조율(%)_울진', '일조율(%)_여수']]
```

- 토양환경 전처리

`'양파_토양수분.xlsx'` 데이터에 `df['얕은평균토양수분'] **=** df[['10CM 일 토양수분(%)', '20CM 일 토양수분(%)', '30CM 일 토양수` 사용하여 새로운 칼럼 추가, `columns_to_drop **=** ['1.5M 평균 습도(%)', '4.0M 평균 습도(%)', '10CM 일 토양수분(%)', '20CM 일` 사용하여 필요 없는 column 제거 → null 값 확인후 `new_moist_df **=** moist_df**.**interpolate()` 빈 값 채움 →`'moist_data.xlsx'` 데이터 추출 

- 재배량 전처리

`'2013_2023_onion_area_product.xlsx'` 데이터에 `yearly_area_prod_df['year'] **=** pd**.**DatetimeIndex(yearly_area_prod_df['date_time'])**.**year`  사용하여 year, month  칼럼 추가 → `area_prod_df **=** pd**.**merge(new_withnull_df, yearly_area_prod_df, on**=**'year')` 두 데이터 프레임 합침 → `'new_quantity_area_2013_22.xlsx'` 로 추출

`'2013_2023_onion_area_product.xlsx'` 데이터에  `yearly_area_prod_df['year'] **=** pd**.**DatetimeIndex(yearly_area_prod_df['date_time'])**.**year`  사용하여 year, month  칼럼 추가 후 `daily_prod_df['생산량(kg)'] **=** daily_prod_df['생산량(kg)'] **/** 30` 월별 값을 일별로 /30해줌 → `daily_prod_df **=** daily_prod_df**.**rename(columns**=**{'생산량(kg)': '월단위생산량(kg)'})` 생산량 column 이름 월단위생산량으로 변환 → `area_prod_df **=** pd**.**merge(yearly_area_prod_df, daily_prod_df, on**=**'year')` year column 기준으로 두 데이터 프레임 합침 → `'new_quantity_area_2020_22.xlsx'` 데이터 추출 

- 경유 전처리

`'경유.xlsx'` 데이터 null값 확인 후 이상 없어 그대로 사용

- 각종 지수 전처리

`'지수데이터.xlsx'` 데이터 `df**.**rename` 함수 사용하여 column 이름 변환 → `new_index_df **=** index_df**.**interpolate()` 사용하여 월별데이터 일별데이터로 변환 →`'index_data.xlsx'` 데이터 추출
