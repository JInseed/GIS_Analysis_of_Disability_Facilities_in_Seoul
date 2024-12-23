# GIS를 활용한  서울시 장애인 복지 시설 입지 분석

<br>

## 진행기간
> **2023/06/09~07/03**
<br>


## 사용 데이터
**`GIS 분석에 활용된 데이터`**

- **장애인구(행정동 기준)**
    - [서울시 장애인 현황 (장애유형별/동별) 통계> 데이터셋> 공공데이터 | 서울열린데이터광장 (seoul.go.kr)](https://data.seoul.go.kr/dataList/10577/S/2/datasetView.do)
- **용도지역(주거지역 등)**
    - https://data.seoul.go.kr/dataList/OA-21136/S/1/datasetView.do#
- **법정동, 행정동 통합 위한 데이터**
    - [서울시 건축물대장 법정동 코드정보> 데이터셋> 공공데이터 | 서울열린데이터광장 (seoul.go.kr)](https://data.seoul.go.kr/dataList/OA-15410/S/1/datasetView.do)
- **사회복지시설**
    - [서울시 사회복지시설 목록> 데이터셋> 공공데이터 | 서울열린데이터광장 (seoul.go.kr)](https://data.seoul.go.kr/dataList/OA-20376/S/1/datasetView.do)
    - 가장 아래 4132행 부터 데이터 형태 다른게 있음, 자치구 정보도 없으므로 삭제 진행
    - 여기서 장애인복지관 시설 데이터 추출
- **서울시 시군구 경계**
    - https://bigdata-geo.kr/user/dataset/view.do?data_sn=159
- **서울시 법정동 경계**
    - [공간융합 빅데이터 플랫폼 (bigdata-geo.kr)](https://bigdata-geo.kr/user/dataset/view.do?data_sn=683)

<br>

**`EDA에 활용한 데이터(현황 조사 및 문제 정의에 활용)`**

- **장애인구(자치구 기준)**
    - [서울시 장애인 현황 (장애유형별) 통계> 데이터셋> 공공데이터 | 서울열린데이터광장 (seoul.go.kr)](https://data.seoul.go.kr/dataList/18/S/2/datasetView.do)
- **전체 인구수**
    - [행정구역(시군구)별, 성별 인구수 (kosis.kr)](https://kosis.kr/statHtml/statHtml.do?orgId=101&tblId=DT_1B040A3)
- **집객시설**
    - [서울시 상권분석서비스(집객시설-자치구)> 데이터셋> 공공데이터 | 서울열린데이터광장 (seoul.go.kr)](https://data.seoul.go.kr/dataList/OA-22170/S/1/datasetView.do)




<br>

## 개발 환경
> **Python**
<br>

### 역할(팀장)
> **자료 조사, 논문 리뷰, EDA, Data Prerprocessing, GIS Analysis**
<br>

## 분석 주제
> **GIS를 활용한 서울시 장애인 복지 시설 입지 분석 및 최적 입지 선정**
<br>

## 분석 배경

- 전 세계 인구가 증가함에도 불구하고 한국은 저출산 등의 이유로 인구가 감소
    - 서울의 인구는 2022년 기준 938만 명으로 감소하였다. 그러나 2007년 기준 210만 명이었던 전국의 장애인 인구가 2022년 기준 260만 명으로 증가
    - `이러한 변화는 장애인 복지 서비스에 대한 수요가 늘어나고 있음을 의미(전혜진 & 성주은, 2023)`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/ff0db62f-7150-41df-875f-eddc0e52639d" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/3b280c84-87eb-4607-9bbd-c1d268ed0869" width="95%">
    </td>
  </tr>
</table>

- 장애인 복지관은 장애인들이 일상생활을 영위하고 사회와 소통할 수 있는 중요한 거점 역할
    - 현재 서울시의 장애인 복지관 분포를 보면, 지역 간 불균형을 보임
- 장애인 실태조사 (2020년)에 따르면, 장애인 복지사업 기관 중 장애인 복지관의 이용 경험이 가장 높음
    - 장애인의 주 외출 목적 관련 설문에서도, 장애인 복지관은 지체장애인의 경우 26.5%, 지체장애인 이외 장애인의 경우 21.9%로 업무 목적 다음으로 높은 외출 목적 순위를 차지했다. 이 결과들은 장애인 복지시설의 접근성이 중요함을 나타냄
- 장애인 복지관의 주 이용자인 `장애인은 이동에 취약한 집단`으로(이유신, 2019), (김제선(2017)에 따르면, 이용자의 주거 공간에서부터 장애인 복지관까지 `이동하는 시간이 길수록 복지관 이용률이 떨어진다는 연구 결과`
- 지역 수요자 유형에 따라 필요한 사회복지시설의 유형 및 공급량이 달라져야 하지만, 지역 특성을 고려한 정확한 분석 과정 없이 시설 공급만을 목적으로 해 `지역 불균형을 초래`하고 있다는 선행 연구(최병소 & 이명훈, 2021)
    - 현재 사회복지시설은 이용자들의 수나 접근성 등의 편의보다 공급자의 입장에서 예산에 맞추거나 부지확보가 용이한 곳으로 입지(김동균, 2016)

<br>


## 문제 정의 및 분석 목표
- **지역별 복지 서비스 불균형**
    - 일부 지역은 장애인 복지관이 충분히 제공되는 반면, 다른 지역은 부족하여 서비스 이용의 격차 발생
- **접근성**
    - 장애인 복지관의 주 이용자인 장애인들은 이동에 취약한 집단
    - 장애인의 주거 공간에서 복지관까지의 거리가 멀면 이용률이 떨어질 수 있기에 복지관의 위치는 장애인의 이동 편의성을 고려하여 선정
- **시설의 효율적 배치**
    - 현재 복지관 배치가 장애인 인구 분포와 불일치하여 일부 지역에서는 복지 서비스 접근성이 낮음

> **GIS (지리정보시스템)을 활용하여 지리적, 공간적 데이터를 종합적으로 고려하여 서울시 내 장애인 복지관의 최적 입지를 분석하고 선정**
>


<br>



## 분석 요약

1. EDA(인구 및 시설 현황 파악)
2. Data Preprocessing
    1. 시설 추출 및 주소 수정
    2. 자치구 경계 데이터 통합
    3. 행정동 경계 데이터 정제
    4. 법정동 및 행정동 통합
3. GIS Analysis
    1. 현황 및 접근성 분석(주거지 및 인구 분포)
    2. 복지관 서비스 이용 권역 분석
    3. 최적 입지 선정(인구 가중 중심점 활용)

- **인구 통계 분석**: 2000년부터 2023년까지의 서울시 전체 인구와 장애인 인구의 변화를 분석하여, 장애인 인구의 증가 추세를 확인
- **시설 관련 분석**: 각 자치구별 장애인 복지관 수와 시설 당 장애인구 수용 비율을 분석하여, 지역별 불균형 문제를 파악
- **GIS 네트워크 분석**: 주거지역에 따른 장애인 복지관 서비스 이용 권역을 시각화하고, 인구가중중심점을 기반으로 최적의 입지를 선정
- **최적 입지 선정 및 시각화**: 미수혜 장애인구수가 많은 5개 자치구를 선정하여, 인구가중중심점을 기준으로 신규 장애인 복지관의 위치를 도출
- **미수혜 면적 및 장애인구수 변화 분석**: 신규 시설 확충 후의 미수혜 면적과 예상되는 미수혜 장애인구수를 분석하여, 신규 시설의 효과를 평가

<br>


## 분석 과정

### *EDA*

- `연도별 서울시 전체 인구 및 장애 인구 변화`, `연도별 전체 인구수 대비 장애인구수 비율 변화`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/c9218240-61e0-4304-ba00-0fb74468fc55" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/9343ca46-8057-4e80-96d8-43d18fcf295b" width="95%">
    </td>
  </tr>
</table>


<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/57f3a973-4a5d-4caa-a47a-833323c63ca6" width="70%">
    </td>
  </tr>
</table>


- 전체 인구 변화: 서울시의 전체 인구는 점진적으로 감소하는 추세
- 장애 인구 변화: 장애 인구는 꾸준히 증가
    - 장애인 복지 서비스에 대한 수요가 지속적으로 증가하고 있음을 의미
- **즉, 전체 인구는 줄어들고 있지만 장애 인구는 증가하고 있으므로, 장애인을 위한 복지시설의 필요성이 더욱 커지고 있음**
    - **따라서, 현재와 미래의 장애인 복지 수요를 충족시키기 위해 장애인 복지관과 같은 복지시설 확충 필요**

<br>


- `구별 장애 복지관 시설 수, 면적 당 시설 수`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/78891d12-128d-4a4e-aad0-937e09654966" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/161509ca-8782-4178-b042-a0c3a0d90f41" width="95%">
    </td>
  </tr>
</table>


- 일부 자치구는 복지관이 많지만, 다른 자치구는 상대적으로 적은 복지관 수를 보임
    - **지역간 장애인 복지 서비스 제공의 불균형을 실 데이터로도 확인**

<br>


- `시설 당 장애인구`, `시설 수 대비 장애인구`


<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/c932d2af-215c-4bc4-9d05-bda948391739" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/d18003bb-52b1-4787-a817-bcee8e2e80a5" width="95%">
    </td>
  </tr>
</table>

- 중랑구, 양천구, 강북구 등 일부 자치구는 시설 당 장애인구 수용 비율이 매우 높다. 이는 해당 자치구의 장애인 복지관이 과밀화
    - 특히 중랑구의 경우 장애인구는 많은 편이나, 복지관은 1개밖에 없어 수용 비율이 매우 높음

<br>


### *Data Preprocessing*

> **본 프로젝트는 서울특별시를 대상으로 진행**

> **서울특별시의 행정구역은 25개의 자치구와 법정동 기준 467개의 동, 행정동 기준 426개의 동으로 구성**

> **이에 법정동을 기준으로 데이터를 통일하여 처리**

<br>

- `법정동, 행정동 경계 데이터 전처리`
    - **장애인 복지관 시설 추출 및 주소 수정(지오코딩 위한 전처리)**
        - 서울 내 사회복지시설에서 장애인복지관 추출
        - 괄호 삭제, 실제 위치랑 다른 것들 수정
    - **자치구 경계 통합**
        - 제공되는 공공데이터가 구별로 제공되어 이를 병합
    - **행정동 경계 데이터 처리**
        - 위의 자치구 데이터와 다르게 행정동 데이터는 전국 읍면동 통합해서 제공됨
        - 서울 행정동 필터링 후
        - 용강동 위치가 북한 지역으로 처리 되어 있어서 수정
    - **법정동, 행정동 병합**
        - 경계 데이터는 법정동 기준, 장애인구데이터는 행정동 기준으로 법정동, 행정동 이름 모두 제공하는 데이터 활용하여 병합 해야함
        - 하나의 행정동이 여러개의 법정동으로 분리된 경우
            - 현재 제공되는 건 행정동 기준의 인구수이기 때문에 법정동으로 바꿀 시 문제가 생김. 정확히 알 수 없기에 분리된 수만큼 나누어 배정
        - 여러개의 행정동이 하나의 법정동으로 병합된 경우
            - 인구 수 합산
        - **추가**
            - 장애 인구 데이터의 보라매동은 법정동명, 행정동명에서는 보래매동으로 나타내서 맞추어 변경
            - 법정동명, 행정동명 동시에 제공하는 데이터에서 보통 상계3.4동 이런 식으로 .으로 하는데 중계2,3동만 유일하게 반점으로 해놔서 처리
            - 자치구는 다르나 동일한 이름의 법정동이 존재하여 이 또한 고려
<br>

- `GIS 분석 시 고려한 전처리`
    - **좌표계 통일**
        - 서울 지역 면적 계산에 적절한 좌표계로 UTM Zone 52N인 32652로 통일
    - **장애인구와 경계데이터 병합**
        - 장애인구데이터에는 존재하지만 경계데이터에는 존재하지 않는 12개의 법정동이 존재하여, 병합 후 장애인구 집계 시 약 4,422명 제외
    - **구별 서비스 미수혜 지역 면적 계산 및 인구 수 계산**
        - 인구 수와 결합하면서 인구데이터에는 존재하나, 주거지역 데이터에는 존재하지 않는 지역이 존재. 약 5,217,537 제곱미터
        - 하나의 주거지역은 여러개의 법정동(인구데이터)에 걸쳐질 수 있음
            - 이 경우 병합하면서 행이 늘어나서 주거지역에 대한 면적을 과집합하게 되는 경우
            - 그 반대의 경우 인구수가 과집합

### *GIs Analysis*

**`현황 및 접근성 분석(주거지 및 인구 분포)`**

- `주거지 현황 및 지역별 장애인 인구 분포`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/b17864ba-8d87-40e4-a434-29e93f3e4873" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/11c7cc84-7d01-49b5-9df2-111557567e62" width="95%">
    </td>
  </tr>
</table>

- 좌측 그림은 서울시 전체 인구 주거 지역을 나타내며, 우측 그림은 법정동 기준 장애인 인구 분포를 나타냄(색이 진할 수록 인구가 많음)
    - 하나의 구 안에서도 장애인 인구 밀집 정도가 다름
    - 지역 별로 장애 인구 분포가 고르지 않음

<br>


- `인구 가중 중심점 및 장애인 복지관 현황`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/f30309b6-4975-4d04-a0f7-01ae49b51f4d" width="90%">
    </td>
  </tr>
</table>

- 서울특별시 대상으로 장애인 복지관은 총 52개
- 위의 지역 별 장애 인구를 살펴보았을 때 하나의 구 안에서도 밀집 정도가 다르게 나타남
- **이에 법정동 장애 인구 수를 기준으로 자치구 별로 인구 가중 중심점을 구해서 확인**
    - **이를 바탕으로 현재 장애인 복지관 위치의 정당성을 확인**
    - 인구 가중 중심점을 기준으로 0.8km(보도 10분)/1.5km(보도 20분)/2.5km(보도 30분) 반경을 나누어 확인
- **인구가중중심점 밖으로 장애인 복지관이 위치한 경우가 많음**
    - 0.8km 반경 외에 있는 장애인 복지관 수: 45
    - 1.5km 반경 외에 있는 장애인 복지관 수: 33
    - 2.5km 반경 외에 있는 장애인 복지관 수: 9
    - 전체 복지관 중 약 83%가 인구 가중 중심점 기준 2.5km 반경 내에 위치
    - 2.5km 반경 외에 존재하는 경우 2.5km 반경 내에 다른 복지관이 하나 이상 존재하고 있음을 확인
    - 그럼에도 0.8km와 1.5km 반경 내에 존재하는 복지관 수가 현저히 적어, **현재 장애인들의 수요를 만족하지 못하고 있음을 추측**

<br>



**`복지관 서비스 이용 권역 분석`**

- `주거 지역과 장애인 복지관 서비스 이용 권역`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/1e290d4a-d3bc-4aca-969e-c96d46f68226" width="90%">
    </td>
  </tr>
</table>

- 서비스 권역은 소생활권 단위 기준인 2km (오병록, 2014)로 지정
- 복지관의 위치와 서비스 권역이 주거 지역에 잘 위치하는 경우도 있지만, 이 안에 위치하지 못하는 주거 지역도 존재
    - **서비스 권역에 포함되지 않은 지역은 장애인 복지관 서비스를 이용하기 어려운 주거 지역으로 추측**

<br>


- `서비스 미 수혜 지역`, `서비스 미수혜 면적 비율 및 예상되는 미수혜 장애 인구`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/4381844d-5974-498a-b288-fe59d9f97283" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/10436c58-00ca-44f1-beff-609fa6218a3c" width="95%">
    </td>
  </tr>
</table>

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/cae16e74-ca7b-4afe-8637-354d5d6920a4" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/ca41266a-9704-49c4-8f81-5f1817f546cf" width="95%">
    </td>
  </tr>
</table>


- **상단은 주거지역 및 인구 분포에서 서비스 이용권역을 제외한 면적으로, 이 지역을 서비스 미수혜 지역으로 정의**
- 하단 좌측은 자치구 별 전체 면적 대비 미수혜 면적 비율을 나타낸 그래프
    - 비율이 높은 자치구는 장애인 복지관의 접근성에 사각지대가 많이 존재한다는 것을 의미
    - 용산구, 성동구, 광진구 등이 높은 미수혜 비율을 보임. 이는 해당 자치구에서 장애인 복지관이 부족하다는 것으로 판단
- 하단 우측은 장애인구수에 미수혜 면적 비율을 곱하여 나타낸 그래프
    - **중랑구의 경우 간단한 EDA에서는 시설 확충이 필요해 보였으나, 실제로는 서비스 이용권역이 주거지역을 충분히 커버하고 있어 추가 시설이 필요하지 않았음을 알 수 있음(면적 기준)**


<br>

**`최적 입지 선정(인구 가중 중심점 활용)`**

> **서울시의 장애인 복지관을 균형 있게 제공하기 위해, 미수혜 장애인구수가 가장 많은 순으로 5개 자치구를 선정하고 최적의 입지를 분석**

<br>

1. **미수혜 장애인구수 기준 5개구 선정**
    1. 복지관 서비스의 사각지대가 큰 지역을 우선적으로 고려
2. **미수혜 장애인구가중중심점 계산**
    1. 각 자치구의 주거지역에 따른 장애인구수를 활용하여 인구가중중심점을 계산
    2. 각 자치구 내에서 장애인 복지관 서비스의 수요가 가장 높은 지역을 파악
3. **최적 입지 선정**
    1. 미수혜 면적의 장애인구가중중심점을 기반으로 각 자치구 내 최적의 장애인 복지관 입지를 선정

<br>


- `최적 입지 선정 결과`

<table width="100%">
  <tr>
    <td align="center">
      <img src="https://github.com/user-attachments/assets/d7a9d81a-5106-4ffe-871a-5b3393f33d79" width="90%">
    </td>
  </tr>
</table>

- 미수혜 장애인 인구 수가 큰 기준으로, 강북구, 강서구, 성동구, 양천구, 용산구로 최적 입지 자치구를 선정
- 기존 장애인 복지관의 위치는 앞선 시각화와 마찬가지로 ♿를 통해 나타내고 있으며, 이때의 서비스 권역은 붉은 색 원으로 표시
- 최적 입지 분석을 통해 선정된 5개의 신규 장애인 복지관은 ⭐을 통해 나타내며, 이때의 서비스 권역은 녹색 원으로 표시
<br>



- `신규 시설 확충 후 예상되는 미수혜 인구 및 면적 변화`

<table width="100%">
  <tr>
    <td align="left" width="50%">
      <img src="https://github.com/user-attachments/assets/e078051a-49f7-4fc7-88c5-5eb8713b4cf2" width="95%">
    </td>
    <td align="right" width="50%">
      <img src="https://github.com/user-attachments/assets/cd84cdd7-ee84-4f40-a44f-65a3eacc2f4a" width="95%">
    </td>
  </tr>
</table>

| 구분 | 확충 전 미수혜 면적 | 확충 후 미수혜 면적 | 신규 수혜 면적 | 확충 전 미수혜 인구 수 | 확충 후 미수혜 인구 수 | 신규 수혜 인구 |
| --- | --- | --- | --- | --- | --- | --- |
| 서초구 | 10,291,360 | 10,225,973 | 65,387 | 2,930 | 2,912 | 19 |
| **강서구** | **6,832,627** | **782,630** | **6,049,996** | **6,098** | **183** | **5,915** |
| 강남구 | 3,879,560 | 3,732,947 | 146,612 | 1,048 | 1,089 | 41 |
| 노원구 | 614,357 | 614,357 | 0 | 318 | 318 | 0 |
| 송파구 | 4,614,841 | 4,614,841 | 0 | 2,389 | 2,389 | 0 |
| 은평구 | 6,534,563 | 6,534,563 | 0 | 3,590 | 3,590 | 0 |
| 관악구 | 2,106,734 | 2,106,734 | 0 | 1,176 | 1,176 | 0 |
| 성북구 | 2,940,407 | 2,411,432 | 528,974 | 1,289 | 418 | 871 |
| 영등포구 | 926,692 | 926,692 | 0 | 447 | 447 | 0 |
| 강동구 | 5,093,811 | 5,093,811 | 0 | 3,035 | 3,035 | 0 |
| 종로구 | 4,764,764 | 4,764,764 | 0 | 1,555 | 1,555 | 0 |
| 마포구 | 2,563,072 | 2,563,072 | 0 | 700 | 700 | 0 |
| **강북구** | **4,699,603** | **995,595** | **3,704,009** | **4,756** | **845** | **3,911** |
| **용산구** | **8,170,819** | **3,120,245** | **5,050,574** | **3,641** | **699** | **2,942** |
| 도봉구 | 2,372,158 | 1,988,258 | 383,901 | 2,469 | 2,069 | 400 |
| 구로구 | 2,449,649 | 2,449,649 | 0 | 2,374 | 2,374 | 0 |
| 중랑구 | 116,715 | 116,715 | 0 | 66 | 66 | 0 |
| 서대문구 | 3,400,808 | 3,400,808 | 0 | 1,300 | 1,300 | 0 |
| **양천구** | **5,023,747** | **1,573,770** | **3,449,978** | **7,614** | **1,593** | **6,021** |
| 광진구 | 4,940,641 | 4,866,796 | 73,845 | 3,619 | 3,565 | 54 |
| **성동구** | **5,016,400** | **2,174,598** | **2,841,802** | **4,540** | **1,546** | **2,995** |
| 동작구 | 1,717,014 | 1,717,014 | 0 | 888 | 888 | 0 |
| 동대문구 | 1,051,170 | 1,050,977 | 193 | 1,251 | 1,251 | 0 |
| 금천구 | 984,460 | 984,460 | 0 | 446 | 446 | 0 |
| 중구 | 2,642,978 | 1,090,927 | 1,552,052 | 1,580 | 437 | 1,142 |
| **total** | **93,748,950** | **69,901,628** | **23,847,322** | **59,160** | **34,850** | **24,310** |


- 좌측은 시설 확충 전 미수혜 면적을 나타내고, 우측은 시설 확충 후 미수혜 면적을 나타냄
    - **시각적으로도 미수혜 면적이 눈에 띄게 감소**
- **신규 시설 확충으로 미수혜 인구가 약 60,000명에서 34,850명으로 약 41% 감소할 것으로 예상**

<br>


**`기대효과 및 의의`**

- **수요 기반의 맞춤형 시설 배치**
    - 기존의 장애인 복지관은 단순히 자치구별로 균등하게 배치되어 있었으나, 실제 수요를 반영하지 못한 경우가 많았음
    - 프로젝트를 통해 장애인 인구가 많은 지역에 복지관을 추가 배치하는 등, 수요에 기반한 맞춤형 시설 배치의 중요성을 확인
    - 예를 들어, 중랑구는 장애인 인구가 많고 복지관이 1개밖에 없어 추가 시설이 필요해 보였으나, 심층 분석 결과 주거지역과 서비스 이용권역이 적절히 커버하고 있어 추가 시설이 필요하지 않음을 알 수 있었음
        - 하지만 이는 이용 권역, 즉 면적을 중심으로 진행한 결과이기 때문에 장애인 복지관 수용 인원 등 추가적인 자료 조사 필요
- **GIS를 활용한 정밀한 입지 선정**
    - `GIS` 기술을 활용하여 `인구가중중심점`을 계산하고, 이를 바탕으로 최적의 장애인 복지관 입지를 선정
    - 이는 기존의 단순한 배치 방식보다 훨씬 더 정밀하고 효과적인 방법임을 보여줌
    - 각 자치구의 주거지역과 인구 밀집도를 고려하여 복지관의 위치를 선정함으로써, 복지 서비스의 접근성을 크게 향상
- **정량적 데이터에 기반한 의사결정**
    - 본 프로젝트를 통해 장애인 복지관의 배치와 관련된 정책 결정을 정량적 데이터에 기반하여 수행할 수 있도록 지원
    - 이는 향후 복지 정책의 계획과 실행에 있어 중요한 참고 자료가 될 수 있음
    - 데이터에 기반한 접근 방식은 자원의 효율적 배분과 불필요한 시설 확충을 방지하는 데 기여
- **미수혜 지역 감소**
    - 신규 장애인 복지관의 적절한 배치를 통해 복지 서비스 미수혜 지역이 크게 감소할 것으로 기대
    - 분석 결과에 따르면, 신규 시설 확충 시 `미수혜 장애인구수`가 약 60,000명에서 34,849명으로 `약 41% 감소`할 것으로 예상
- **접근성 향상**
    - `인구가중중심점`에 기반한 복지관 배치를 통해 장애인들이 복지관에 보다 쉽게 접근 가능
    - 이는 장애인들의 이동 편의성을 증대시키고, 복지 서비스 이용률을 높이는 데 기여
- **장애인의 삶의 질 향상**
    - 장애인 복지관의 최적 입지 선정은 장애인들이 보다 쉽게 복지 서비스를 이용할 수 있도록 하여, 그들의 삶의 질을 향상시키는 데 직접적인 기여
    - 복지관의 접근성 향상은 장애인들의 사회적 참여와 통합을 촉진하고, 궁극적으로 더 나은 사회적 환경을 조성하는 데 도움이 될 것

<br>

## 시사점 및 보완할 점

- **단순히 장애인 복지관의 수량적인 측면뿐만 아니라, 실제 수요에 맞춘 적절한 위치 선정의 중요성을 강조**
- **자료 조사 및 EDA**
    - 다양한 논문을 검토하여 선행 연구를 체계적으로 분석하고, `실제 데이터를 수집`하여 EDA를 수행함으로써 현황과 배경을 면밀히 조사하며, `분석의 필요성과 문제 정의를 명확히 수립`
    - 지역 경계 데이터를 정밀하게 전처리하여 `데이터의 정합성`을 철저히 확보
    - 중랑구의 경우 간단한 EDA에서는 시설 확충이 필요해 보였으나, 실제로는 서비스 이용권역이 주거지역을 충분히 커버하고 있어 추가 시설이 필요하지 않았음을 알 수 있음
        - 하지만 이는 이용 권역, 즉 면적을 중심으로 진행한 결과이기 때문에 장애인 복지관 수용 인원 등 추가적인 자료 조사 필요
- **기대효과 및 의의**
    - 수요 기반의 맞춤형 시설 배치
    - `GIS`를 활용한 정밀한 입지 선정
    - 정량적 데이터에 기반한 의사결정
    - `복지 서비스 미 수혜 장애 인구 감소(약 41% 감소 예상)`
    - 접근성 향상
    - 장애인의 삶의 질 향상
- **향후 분석 제언**
    - 교통 접근성 데이터 포함
        - 교통 접근성 데이터를 포함하여 장애인들이 복지관에 접근하는 데 걸리는 시간과 편의성을 분석하면 보다 정밀한 입지 분석 가능할 것
    - 동적 인구 변화 반영
        - 인구의 이동 및 변화 추이를 반영한 동적 모델을 개발하여, 미래의 인구 분포 변화를 예측하고 이에 맞춘 복지관 입지 분석을 수행하면 더 정확한 인구수 데이터로 분석할 수 있을 것
    - 종합적인 복지 서비스 분석
        - 장애인 복지관 외에도 다양한 복지 서비스를 포함한 종합적인 분석을 수행하여, 장애인들이 필요로 하는 모든 서비스를 적재적소에 배치하는 방안 모색 가능
    - 장애인 및 지역 주민의 의견 수렴
        - 장애인과 지역 주민들의 의견을 수렴하여, 실제 현장에서의 요구와 기대를 반영한 정책을 수립함으로써 실효성을 높일 수 있을 것






