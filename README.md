# Onion Price Prediction by DATAMINING TEAM5 

# Purpose
CA저장고에 양파를 저장하거나 판매할 시기를 정하는 데 도움이 되도록 양파의 가격을 예측

# Method
단기(1개월)와 장기(6개월)로 나눠서 데이터를 최대한 활용함

# Model
XGboost

### File Structure of Codes

```bash
├── 전처리 합치기
│   ├── 경유전처리
│   │   └── preprocessing_oil.ipynb
│   ├── 기상전처리
│   │   └── preprocessing_weather_after2013.ipynb
│   │   └── preprocessing_weather_after2020.ipynb
│   ├── 양파가격전처리
│   │   └── preprocessing_onion_price.ipynb
│   ├── 재배량전처리
│   │   └── preprocessing_quantity_area_after2013.ipynb
│   │   └── preprocessing_quantity_area_after2020.ipynb
│   ├── 지수전처리
│   │   └── preprocessing_지수.ipynb
│   ├── 토양습도전처리
│   │   ├── preprocessing_습도_토양수분.ipynb
│   ├── combining_onion_total_file_after2013.ipynb
│   ├── combining_onion_total_file_after2020.ipynb
├── 2020년이후_1개월단위예측
│   ├── 한달후예측.ipynb
├── 2013년이후_6개월단위예측
│   └── 6개월후예측.ipynb
├── eda.ipynb
```


# How to Use
1. 전처리 합치기 폴더를 이용해 원본 파일 데이터 추세를 시각화해서 확인하고, 모델링에 쓸 수 있도록 가공하고, 모델링 폴더에 저장한다.
    
    a) 2013년이후_6개월단위예측 폴더에 onion_after2013_combined.xlsx이 저장된다.
    
    b) 2020년이후_1개월단위예측 폴더에 onion_after2020_combined.xlsx이 저장된다.
    
2. 1-a, 1-b의 데이터 파일을 eda.ipynb를 실행해 변수의 특징 및 상관관계를 살펴본다.
3. 1-a 파일로 6개월후예측.ipynb를 실행하면 예측 결과가 저장된다.
4. 1-b 파일로 한달후예측.ipynb를 실행하면 예측 결과가 저장된다.


# Results
## 1개월 후 예측 시각화 & Feature importance
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/741f2588-3ab7-466f-b823-cb8c79eb1e20)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/b84b0aec-9882-4116-a5f8-56f24143216a)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/b903a052-fd44-40fd-bd7f-11efa495eea3)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/844ddac9-df65-4987-8f8f-1277ac2d0933)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/446447e3-f030-44df-b36c-54bfd7c2a8a2)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/004f0ea3-045f-4eb0-b10b-0b9515d50536)



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
