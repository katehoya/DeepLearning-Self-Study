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
Bag of 













