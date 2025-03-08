***
Token : 말뭉치(scorpus)에서의 의미있는 단위. 상황, 언어 에 따라 단위가 달라질 수 있다.  
토큰화에서 고려해야 할 사항  
1) 구두점이나 특수 문자를 단순 제외하면 안된다.  
 - ex) Ph.D, $45.55  
2) 줄림말과 단어 내에 띄어쓰기가 있는 경우  
'접어'라는 것이 있다. 예를 들어 we're는 we are의 줄임말이다. 이 때 re를 접어라고 한다.  
이런 접어가 있는 경우, 한 단어 내에 띄어쓰기가 있어도 하나의 토큰으로 봐야할 수도 있다.  

3) 표준 토큰화(Penn Treebank Tokenization)예제  
규칙 1. '-'하이픈으로 구성된 단어는 하나로 유지한다.  
규칙 2. 접어가 함께하는 단어는 분리한다.(doesn't -> does, n't)   
 - 한국어가 영어보다 토큰화가 훨씬 어렵다.  
 - 교착어의 특성과, 띄어쓰기 때문에.  

 - 또한 단어의 표기는 같지만 품사가 다른 경우가 있다. ex) 못  
영어 품사 태깅 : NLTK, 한국어 품사 태깅 : KoNLPy.  
 --> 여러 토큰화하는 라이브러리가 있기 때문에 필요 용도에 따라 형태소 분석기를 선택하면 된다.  

***

토큰화 전후에는 항상 정제, 정규화를 한다.  
정제(cleaning) : 갖고 있는 코퍼스로부터 노이즈 데이터를 제거.  
정규화(normalization) : 표현 방법이 다른 단어들을 통합시켜서 같은 단어로 만들어 줌.  
 - 규칙에 기반한 표기가 다른 단어들의 통합. ex) US와 USA, uh-huh와 uhhuh  
 - 대, 소문자 통합.  
 - 불필요한 단어의 제거. 
  ex) 등장 빈도가 적은 단어, 길이가 짧은 단어.  
 - 정규표현식.  

***

불용어 : 자주 등장하지만 분석을 하는 데는 큰 도움이 안되는 단어들. ex) I, my, me, 조사...  
![image](https://github.com/user-attachments/assets/8c0c39c9-38fd-4317-b1b4-55fe90575064)  
또한 불용어를 사용자가 직접 지정할 수도 있다.  
![image](https://github.com/user-attachments/assets/ac186076-38e9-47e8-9979-95d8338321df)  

정규표현식 실습 : https://wikidocs.net/217240  
자연어 처리 전처리 실습 : https://wikidocs.net/64517

***
***

언어 모델 : 단어 시퀀스에 확률을 할당하는 모델.  
크게 나누자면, 통계를 이용한 방법, 인공 신경망을 이용한 방법이 있다.  
 - 단어 시퀀스의 확률(w개의 단어) : ![image](https://github.com/user-attachments/assets/8c7adee2-dea9-4811-94e9-94372090cb05)
 - 다음 단어가 등장할 확률(n-1 나열된 상태에서 n번째 단어 예측) : ![image](https://github.com/user-attachments/assets/973ad4d3-9c9d-45ca-8670-4b637f08c218)
 - 전체 단어 시퀀스 W의 확률 : ![image](https://github.com/user-attachments/assets/c0b9f420-ebf0-4093-b33d-ab3c1828a79f)


통계적 언어 모델 SLM(Stochastic Language Model)  
chain rule : ![image](https://github.com/user-attachments/assets/37817c0f-eeb8-47a2-8234-109b3a690fbf)  
각 단어는 문맥이라는 관계로 인해 이전 단어의 영향을 받아 나온 단어이다.  
따라서 문장의 확률은 각 단어들이 이전 다너가 주어졌을 때 다음 단어로 등장할 확률의 곱으로 구성된다.  
![image](https://github.com/user-attachments/assets/f278bfa4-e50a-40bb-abcf-b3c0fdd6835a)  
![image](https://github.com/user-attachments/assets/734a075c-8e5b-41e8-b9ac-d006e71d1418)  
 - 카운트 기반의 확률 계산 :
![image](https://github.com/user-attachments/assets/d2bcf899-1f47-4bf3-b400-84e61c95ffe0)
 --> 그러나 실제로는 이 세상의 모든 이런 경우의 수를 모두 데이터에 넣을 수가 없다.
   따라서 이런 희소 문제를 해결하기 위해 n-gram 언어모델이나 스무딩, 백오프와 같은 일반화 기법이 있음.

**n-gram 언어모델** : SLM 기반 모델. 다만 이전에 등장한 모든 단어가 아니라, 일부 단어만 고려하는 접근방법. 이 때 분석하는 일부 단어의 개수가 n개이다.  
ex) ![image](https://github.com/user-attachments/assets/94fa13c7-d050-4d47-8c99-5e5528a7d8a6)  
이전에 나온 연속적인 단어 나열 개수에 따라 unigrams, bigrams, trigrams, 4-grams ....  
그러나 n-gram의 언어모델 또한 전체 문장을 고려한 언어 모델보다는 정확도가 떨어질 수밖에 없다.  
  -> 여전히 희소 문제가 존재함.  
  -> n을 선택하는 것은 trade-off이다. n이 커지면 모델의 성능이 좋아질...수도 있지만 동시에 데이터의 희소성으로 인해 부정확해질 수도 있다.  
  보통은 n < 5로 권장된다.  
  
***  

언어 모델의 평가방법 : Perplexity(PPL)  
perplexity : 헷갈리는 정도. perplexity 수치가 낮을 수록 좋은 것.  
PPL : 문장의 길이로 정규화된 문장 확률의 역수.    
![image](https://github.com/user-attachments/assets/371fd259-9aa0-4501-9b01-9eae31f13c0d)  

Branching factor : 특정 시점에서 평균적으로 몇 개의 선택지를 가지고 고민하는지.  
if PPL = 10 : ![image](https://github.com/user-attachments/assets/5c0ed835-ab43-4602-a9bd-30f4fe4787c3)  
주의사항 : PPL값이 낮다는 것은 test data 상에서 높은 정확도를 보인다는 것이지, 사람이 직접 느끼기에 좋은 언어모델이라는 뜻은 아니다.  

단어의 표현 방법 : 국소표현(Local Representation), 분산 표션 (Distributed Representation)  
국소 표현 : 해당 단어 그 자체만 보고, 특정값을 매핑하여 단어를 표현하는 방법.  
분산 표현 : 그 단어를 표현하고자 주변을 참고하여 단어를 표현하는 방법.  

![image](https://github.com/user-attachments/assets/b3c78be8-1802-4452-9c78-f5dc63c9540e)  
***
**Bag of Words** : 단어의 순서는 고려하지 않고, 단어의 출현 빈도에만 집중하는 텍스트의 수치화 표현방법.  
과정 1. 각 단어에 고유한 정수 인덱스 부여.  
과정 2. 각 인덱스의 위치에 단어 토큰의 등장 횟수를 기록한 벡터를 만듬.  
 - BoW는 주로 어떤 단어가 얼마나 등장했는지를 기준으로 문서가 어떤 성격의 문서인지를 판단하는 작업에 쓰인다. ex) 분류 or 문서 간 유사도

**Document-Term Matrix(DTM)** 
문서단어행렬 : 다수의 문서에서 등장하는 각 단어들의 빈도를 행렬로 표현한 것.  
BoW를 다수의 문서에 대해 행렬로 표현한 것이다.  
![image](https://github.com/user-attachments/assets/9edfa548-04ee-41d3-96f0-0856c0c66888)  

DTM의 한계 : 
 - 희소 표현의 문제 : 저렇게 원-핫 벡터로 표현할 시 단어 집합의 크기가 벡터의 차원이 된다는 문제가 생긴다.
불필요한 데이터가 많다는 말이다. 그래서 구두점, 불용어 제거 등의 정규화를 통해 단어 집합의 크기를 줄여야 함.
 - 단순 빈도 수 기반 접근 : 단순히 많은 단어가 겹친다고 해서 유사하다고 하기 힘들다.
   ex) the 같은 불용어가 많이 겹쳐봤자 의미가 없다.
   -> 그래서 각 단어의 중요도를 계산해야 한다.

**Term Frequency-Inverse Document Frequency(TF-IDF)** : 우선 DTM을 만들고, TF-IDF 가중치 부여.  
tf(d,t) : 문서 d에서 단어 t의 등장 횟수.  
df(t) : 특정 단어 t가 등장한 문서의 수.  
idf(t) : ![image](https://github.com/user-attachments/assets/617a2ad0-2cd1-4a23-abe0-f3c39dbad5e7)  
 - 분모 0 방지, 너무 급격하게 커지는 것 방지.
최종 TD-IDF 값 : tf값 x idf값.

TD-IDF는 모든 문서에서 자주 등장하는 단어는 중요도가 낮다고 판단하며, 특정 문서에서만 자주 등장하는 단어가 중요도가 높다고 판단함.  

**코사인 유사도(Cosine Similarity)**  
방향이 일치하면 1, 점점 방향이 달라져서 반대가 되면 -1.  
![image](https://github.com/user-attachments/assets/7048fcf0-4cf6-4e47-968f-1591db1f0824)  
 - A와 B는 DTM이나 TF-IDF 행렬이 된다.
 - 코사인 유사도는 벡터의 길이가 아니라 방향(문서 패턴)에 초점을 두므로 문서의 길이가 다른 상황에서 비교적 공정한 비교를 할 수 있게 해줌.

***  



















