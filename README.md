## 수상기록
![KakaoTalk_20221102_160940229](https://user-images.githubusercontent.com/108644811/199635931-2a8760bb-8556-492f-ba6c-e80892b5a7cc.jpg)


## 🔰  team members
| 학과 | 이름 | 비고 |
| -------- | ---- | ---- |
| 기계공학 | 정지호 |  |
| 메카트로닉스공학과 | 지수빈 |  |
| 소비자학과 | 장동훈 |  |
| 수학과 | 조현진 |
| 수학과 |문현정| |
| 천문학과 |양지인| |
## 🕑 git 명령어 모음

| 번호 | 기능 | 명령어 |
| -- | --- | ------------ |
| 1 | 초기화 | git init |
| 2 | 상태확인 | git status |
| 3 | 파일 전체 올리기 | git add .  |
| 4 | 커밋 메세지 | git commit -m "msg" |
| 5 | 저장소 이름 지정 | git remote add orgint [주소] |
| 6 | 주소 복사법 | insert or shift+insert |
| 7 | 저장소에 밀어넣기 | git push origin master(or main) |
| 8 | 원격에서만 삭제 | git rm --cached -r [폴더명] -> commit, push |
| 9 | 원격 로컬 모두 삭제 | git rm -rf [폴더명] -> commit, push |
| 10 | 이력 확인 | git log|
|11| 특정상태 복귀 | git reset --hard <commit_id> |
|12| 직전상태 복귀 | git reset --hard HEAD^|


## 💥 이슈 라벨 의미
| keyword |	의미|
| ------- | ------------ |
|bug|	예기치 않은 문제 또는 의도하지 않은 동작(버그)을 나타냅니다.|
|documentation	|문서를 개선하거나 추가 할 필요가 있음을 나타냅니다.|
|duplicate	|해당이슈 또는 PR이 기존에 있음을 나타냅니다.|
|enhancement|	새로운 기능 요청을 나타냅니다.|
|good first issue	|처음 기여해볼 사람에게 좋은 문제를 나타냅니다.|
|help wanted	|관리자가 문제 또는 PR 요청에 대한 도움을 원함을 나타냅니다.|
|invalid|	이슈 또는 PR 요청이 더 이상 관련이 없음을 나타냅니다.|
|question	|이슈 또는 풀 요청에 추가 정보가 필요함을 나타냅니다.|
|wontfix|	문제 나 PR 요청에서 작업이 계속되지 않음을 나타냅니다.|

# 요약

### 카페 데이터

- 구암동 / 월평동 / 장대동 / 궁동 / 봉명동 / 죽동 / 어은동 (학교 인근 위주)에 있는, 네이버 리뷰 참가자 수(블로그가 아닌 **방문자들** 대상. 인증된 리뷰이므로)가 **100명 이상**인 카페들을 선정했다. 리뷰 참가자수가 너무 적으면 객관적인 추천이 힘들기 때문이다.
- 와플대학 같은 특수한 카페는 제외하였다(애초에 공부에 적합한 곳이 아니다)
- 스터디 카페는 제외하였다(스터디 카페가 아니더라도 공부하기 좋은 곳을 추천해 주고 싶은게 우리의 목적이므로)

---

### 정렬에 의한 추천

- **평균 평점**
    - 평균 평점 데이터는 네이버 평점(없으면 구글 평점)을 추출하여 구성한다.
    - 사용자가 평균 평점을 고려 시, 평점이 높은 순으로 데이터를 반환한다(sorting).
    
- **평균 가격**
    - 평균 가격은 각 카페의 1500원 이상 8000원 이하인 음료들의 평균 가격이다. 너무 가격이 낮거나 높은 것들은 이용빈도가 낮으므로 제외하였다.
    - 네이버 데이터로부터 음료 가격들을 크롤링 해온다. 그 후 각 카페의 음료 가격들의 평균을 계산한다.
    - 사용자가 평균 가격을 고려 시, 평균 가격이 낮은 순으로 데이터를 반환한다(sorting).
    
- **공부 환경**
    - '공부 환경 점수'를 만들기로 결정하였다.
    - 네이버 방문자 리뷰는 카테고리 별로 제시가 되어있다. 예를 들면 '커피가 맛있어요'라는 카테고리에 체크한 리뷰어 수 440명, '인테리어가 멋져요'라는 카테고리에 체크한 리뷰어 수 437명. 이런 식으로 정보가 주어지는 것이다.

<p align="center">
 <img src = "https://user-images.githubusercontent.com/108641325/206123084-3c0d9cfc-2eb5-4970-8f75-fb43a66a1c43.png">
</p>
    
우리는 이 정보를 토대로 공부 환경 점수를 다음과 같이 산출한다.

**=> '공부 환경 점수' = ({매장이 청결해요+집중하기 좋아요+좌석이 편해요}/각 카테고리 득표수의 총합})**

**=> '공부 환경 점수' = ({집중하기 좋아요+좌석이 편해요}/각 카테고리 득표수의 총합})** 

<**매장이 청결해요는 매장만 청결하다고 하는 경우가 있어서 기준에서 제외하기로 했다.>**

- 사용자가 공부환경점수를 고려 시, 공부환경점수가 높은 순으로 데이터를 반환한다(sorting).

---

### 감성분류 & 유사도 분석을 통한 카페 추천

1. 카페이름 + overview + concept 정보를 하나로 합쳐 각 카페마다 하나의 문서를 만들었다. overview는 감성분류 모델로 뽑아낸 카페의 긍정 리뷰 상위 5개로 이루어진 문서이고,  concept은 카페의 특색과 구조에 관한 정보로 이루어진 문서이다. 그리고 우리는 이 문서들 간의 유사도를 판별하여, 사용자가 특정 카페를 검색하였을 때 그 카페와 유사한 카페들을 추천하고자 하였다. 

**concept**

![image](https://user-images.githubusercontent.com/108641325/206121648-7cedc703-f84d-4b45-9c14-c866a0460dae.png)

2.  초기에는 카페 간의 유사도를 단순히 코사인 유사도만 사용하여  판별하려고 하였다(overview로는 랜덤하게 추출한 카페 리뷰들을 활용하려고 하였다). 하지만 그렇게 되면 리뷰에 있는 부정적인 키워드를 바탕으로 유사도를 판별하여 추천할 수 있다는 문제가 발생하였다. 
    
    **그래서 우리는 감성분류 모델을 만들고, 이를 통해 긍정적인 리뷰 상위 5개를 추출하여 overview를 만들었다.  이를 통해 우리는 단순히 코사인 유사도만 사용한 추천보다 더 질 좋은 추천을 할 수 있게 되었다.** 
    
3. 감성분류 모델을 만들기 위해, 카페에 대한 긍정, 부정 리뷰들을 수집한 후, 이를 활용해 모델을 훈련(학습)시켰다.  
    
    **이때 감성분류 모델의 성능을 높이기 위하여 수집한 리뷰 데이터들을 전처리 해주었다. 리뷰 데이터의 결측치를 제거하고, 불필요한 문자를 제거하기 위해 불용어를 정의하였다. 또한 등장 빈도가 2번 이하인 희귀 단어들의 비율이 낮은 것을 파악하여, 이들이 학습에 있어 잡음이라고 판단한 후 제거하였다.**
    
    **또한 학습하는데 걸리는 시간을 감소시키기 위해 리뷰들의 길이를 파악, 리뷰 최대 길이를 정하였다.** 
    

4. 또한, 감성분류 모델을 만들 때 **로지스틱 회귀를 사용하였다.** 로지스틱 회귀(Logistic Regression)는 회귀를 사용하여 데이터가 어떤 범주에 속할 확률을 0에서 1 사이의 값으로 예측하고 그 확률에 따라 가능성이 더 높은 범주에 속하는 것으로 분류해주는 지도 학습 알고리즘이다. **우리가 카페 리뷰를 분류할 때는 긍정 혹은 부정, 즉 0 또는 1로만 분류하기 때문에 로지스틱 회귀가 적합하다고 판단하였다.** 
    
    **선형 회귀 분석**은 우리가 가진 데이터를 가장 잘 설명할 수 있는 일차 함수 y=wx+b의 기울기(w)와 b를 얻는 것인데, 이것은 0과 1로만 리뷰를 분류할 때는 **의미가 없다.** 
    
    단, 로지스틱 회귀는 과대적합에 취약하므로, **조기종료**를 통해 **최적의 모델을 선별해준다.**
    
5. 우리는 로지스틱 회귀에 맞게 시그모이드 함수를 활성 함수로 사용하였다. 이때, 단순 **RNN(시계열 데이터와 같이 시간의 흐름에 따라 변화하는 데이터를 학습하기 위한 인공 신경망), 그러니까 순환 신경망 기법을 사용하면 시그모이드 함수의 기울기 소멸 문제가 발생하게 된다. 그래서 우리는 이 문제를 해결해 줄 수 있는 요소들을 지닌 lstm(RNN의 한 종류)을 활용하였다.** 
    
    **LSTM 핵심요소 4가지**
    
    **1) 메모리 블록(셀): 은닉 상태 장기 기억**
    
    **2) 망각 개폐구: 기억 유지 혹은 제거**
    
    **3) 입력 개폐구: 입력 연산**
    
    **4) 출력 개폐구: 출력 연산**
    
    **즉, 위  네가지 핵심요소를 통해 기울기 소멸 문제를 방지하여 우리는 모델 성능을 향상 시켰다.**
    
    또한 **RNN은 긴 시간 의존성 문제가 있는 반면,** lstm은 ****긴 의존 기간을 필요로 하는 학습을 수행할 능력을 갖고 있기 때문에 모델 학습에 있어 더 효율적이다.**** 
    

6. 각 카페간의 유사도를 판별할 때, 컴퓨터는 일반 문자를 인지하지 못하므로 우리는 각 카페에 대한 문서를 벡터화 시켰다. **이때 활용한 방법이 TF-IDF 방법이다. 단순 빈도에 따라 중요도를 판단하는 CounterVectorizer 보다는, 중요하지 않은 단어에는 가중치를 조금만 주어 문서 벡터의 방향이 좀 더 중요한 단어들에 의해 좌우되도록 TF-IDF를 선택해 준 것이다. 이로써 우리는 더 질 좋은 추천을 하고자 하였다.**

