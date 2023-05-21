# Onion Price Prediction by DATAMINING TEAM5 

# Purpose
CA저장고에 양파를 저장하거나 판매할 시기를 정하는 데 도움이 되도록 양파의 가격을 예측

    단기(1개월) : CA 저장고에 저장 후, 매월 가격 추세를 예상해 가격이 증가했을 때 팔 수 있도록
    
    장기(6개월) : CA 저장고에 저장하기 전, 추후 6개월의 추세를 예상해가격이 감소했을 때 살 수 있도록

# Model
XGboost : 순차적으로 트리를 만들어 이전 트리보다 나은 트리 만듦

    장점1. 랜덤 포레스트보다 빠르고 높은 예측능력 가짐
    
    장점2. 변수 종류가 많고 데이터가 클수록 상대적으로 뛰어난 예측 능력 보임


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
3. 1-a 파일로 6개월후예측.ipynb를 실행하면 예측 결과 및 feature importance를 확인할 수 있다.
4. 1-b 파일로 한달후예측.ipynb를 실행하면 예측 결과 및 feature importance를 확인할 수 있다.

# Results
## 1개월 후 예측 시각화
### 2020.01.01~2022.11.01 
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/741f2588-3ab7-466f-b823-cb8c79eb1e20)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/b84b0aec-9882-4116-a5f8-56f24143216a)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/b903a052-fd44-40fd-bd7f-11efa495eea3)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/844ddac9-df65-4987-8f8f-1277ac2d0933)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/446447e3-f030-44df-b36c-54bfd7c2a8a2)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/004f0ea3-045f-4eb0-b10b-0b9515d50536)

## 6개월 후 예측 시각화
### 2013.01.01~2022.11.01 
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/fded9c59-0354-4205-bca5-618fbb73143b)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/be48ff6d-b1e5-4024-a4ac-63757636b979)
![image](https://github.com/sunyeong007/DATAMINING_TEAM5_onion_price_prediction/assets/63898232/1da015a2-709d-40dd-af87-9c8d361e176b)





## [전처리 자세한 내용]
#### 양파 도매가격 전처리
    < 원본 데이터 >
    '상품_양파_{}년_일별.xlsx'(2013-2023년)
    '중품_양파_{}년_일별.xlsx'(2013-2023년)
    
    상품 중품 평균 계산후 새로운 column 형성→ 첫 행 null이면 다음 행 값으로 채우고, 나머지 null은 interpolate로 채움
    
    < 전처리 후 데이터 >
    'onion_price_data.xlsx' 

#### 기상환경 전처리
    < 원본 데이터 >
    'weather_after2013.xlsx'
    'weather_after2020.xlsx'
    
    지역별 평균풍속, 일조율 새로운 column 형성후 병합
    
    < 전처리 후 데이터 >
    'weather_df_after2013.xlsx'
    'weather_df_after2020.xlsx' 
    
#### 토양환경 전처리
    < 원본 데이터 >
    '양파_토양수분.xlsx'
    
    10, 20, 30CM 밑 토양수분의 평균을 얕은평균토양수분이라는 새로운 컬럼으로 추가
    양파와 상관 없는 1.5M 평균 습도(%), 4.0M 평균 습도(%) 컬럼은 제거
    null 값 확인 후 interpolate 사용해 채움    
    
    < 전처리 후 데이터 >
    'moist_data.xlsx' 

#### 재배량 전처리
    < 원본 데이터 >
    '2013_2023_onion_area_product.xlsx'
    
    일별 단위의 새 데이터프레임을 만든 후, 연도에 해당하는 값 넣음   
    
    < 전처리 후 데이터 >
    'new_quantity_area_2013_22.xlsx'
    
    < 원본 데이터 >
    '2020_2023_onion_area_product.xlsx'
    
    일별 단위의 새 데이터프레임을 만든 후, 연도에 해당하는 값 넣음
    일별 단위의 새 데이터프레임을 만든 후, 월별 생산량 데이터를 interpolate로 일별데이터로 변환
    두 데이터프레임을 연도 기준으로 합침
    
    < 전처리 후 데이터 >
    'new_quantity_area_2020_22.xlsx'

#### 경유 전처리
    < 원본 데이터 >
    '경유.xlsx'
    
    null 값이 없는 일별데이터라 그대로 사용    
    
    < 전처리 후 데이터 >
    'moist_data.xlsx' 

#### 지수 전처리
    < 원본 데이터 >
    '지수데이터.xlsx'
    
    월별 물가지수 데이터를 interpolate로 일별데이터로 바꿔서 넣음  
    
    < 전처리 후 데이터 >
    'moist_data.xlsx' 
