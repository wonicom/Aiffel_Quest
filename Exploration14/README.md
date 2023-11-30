# AIFFEL Campus Online 7th Code Peer Review Templete

- 코더 : 김찬중
- 네비게이터 : 김윤서, 방은송
- 리뷰어 : 박준



🔑 **PRT(Peer Review Template)**

- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
        - 해당 조건을 만족하는 부분의 코드 및 결과물을 근거로 첨부
          
300 epoch 학습을 진행한 후 최종 테스트 결과에서 진행한 epoch 수에 걸맞은 정도의 품질을 확인하였다. 굉장히 많은 학습을 잘 진행했고 결과도 좋은것 같습니다.

![](https://github.com/currybab/first-repository/assets/7679722/07472365-3170-4c55-9678-d0e9ea51e4f7)

![](https://github.com/currybab/first-repository/assets/7679722/cf053428-9fef-4f8d-b202-34446021170c)

![](https://github.com/currybab/first-repository/assets/7679722/f7c540ff-13ad-40f4-90bd-47608c306bf6)



data 전처리 및 augementation을 성공적으로 진행하였다.

![](https://github.com/currybab/first-repository/assets/7679722/c5fa384b-922c-47fc-8f2b-740e065b978a)

![](https://github.com/currybab/first-repository/assets/7679722/d16c3566-b3c5-41bf-889d-16b8dc02e5dd)


UNetGenerator와 Discrimnator를 활용해서 모델을 잘 구성해주었습니다.

![](https://github.com/currybab/first-repository/assets/7679722/7c3dc383-3e5e-44ff-9ca5-c99b134de9da)

![](https://github.com/currybab/first-repository/assets/7679722/0f8abeaf-2109-4a2c-8b36-2da813bad2bb)


    
- [X]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.

![](https://github.com/currybab/first-repository/assets/7679722/d1839449-7ef3-40fa-bfed-725cdfcd540d)

위와 같이 로스를 정의하는 부분을 구체적으로 설명해주었습니다.
        
- [X]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인 또는
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.

또한 그래프를 그릴때 데이터를 제대로 저장하지 못한 문제가 있었다고 하는데 아웃풋 로그를 활용하여 text파일로 만들고 직접 파싱해서 사용한점이 아주 최고 입니다.

![](https://github.com/currybab/first-repository/assets/7679722/4fa7739d-847e-45aa-a2ae-bd02fdab9ff5)

위와같이 훈련이 느린부분을 gpu를 활용한부분이 있습니다.
     
![](https://github.com/currybab/first-repository/assets/7679722/d729e7c9-10f1-4519-b07b-32303f6b4a0d)


        
- [X]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 상세히 기록되어 있는지 확인
        - 딥러닝 모델의 경우,
        인풋이 들어가 최종적으로 아웃풋이 나오기까지의 전체 흐름을 도식화하여 
        모델 아키텍쳐에 대한 이해를 돕고 있는지 확인

![](https://github.com/currybab/first-repository/assets/7679722/97674c75-263d-42c6-8d7f-4ad39f98eb90)

배운점, 아쉬운점, 느낀점, 나아질점 모두 잘 작성해주셨습니다.


- [X]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 모듈화(함수화) 했는지
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.
     
이미 많은 코드가 첨부되어 더 추가하지는 않겠습니다. 가이드와 모듈화를 잘 하고 있습니다.
