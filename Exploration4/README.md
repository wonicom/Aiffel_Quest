# AIFFEL Campus Online 7th Code Peer Review Templete

- 코더 : 김찬중
- 네비게이터 : 박준, 전휘호
- 리뷰어 : 강다은



🔑 **PRT(Peer Review Template)**

- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?** 
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
        - 해당 조건을 만족하는 부분의 코드 및 결과물을 근거로 첨부
      
         <br/>

        >노드에서 학습한 다양한 학습 모델 및 학습 실험 전략들을 활용하여, Kaggle 문제를 풀이하기 위한 코드를 완성하였습니다. 문제 풀이("price" 예측값) 결과를 제출하고 루브릭 기준에 해당하는 11만점 이하의 성능을 달성하였습니다. 또한 제출 결과 및 점수를 그림 파일로 첨부하였습니다.
        ```
        y_pred = AveragingBlending(models, X, y, test_data)
        ```
        
        ```
        submission_path = data_dir + "/sample_submission.csv"
        submission = pd.read_csv(submission_path)
        submission["price"] = predictions
        submission_csv_path = "{}/submission_{}_RMSLE_{}.csv".format(data_dir, "ensemble", 1)
        submission.to_csv(submission_csv_path, index=False)
        ```
        <br/>
        
    
- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?** 
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.
      <br/>
      
        >여러가지 연산을 수행하는 함수의 경우, 단계별로 주석을 작성하여 이해하기 쉬운 코드를 작성하였습니다
        ```
        def my_GridSearch(model, train, y, param_grid, verbose=2, n_jobs=-1):
            # 1. GridSearchCV 모델로 `model`을 초기화합니다.
            grid_model = GridSearchCV(
                            model,
                            param_grid=param_grid,
                            scoring="neg_mean_squared_error",
                            cv=5,
                            verbose=verbose,
                            n_jobs=n_jobs,
                            )
        
            # 2. 모델을 fitting 합니다.
            grid_model.fit(train, y)

            # 3. params, score에 각 조합에 대한 결과를 저장합니다.
            params = grid_model.cv_results_["params"]
            score = grid_model.cv_results_["mean_test_score"]

            # 4. 데이터 프레임을 생성하고, RMSLE 값을 추가한 후 점수가 높은 순서로 정렬한 `results`를 반환합니다.
            results = pd.DataFrame(params)
            results["score"] = score
            results["RMSLE"] = np.sqrt(-1 * results["score"])
            results = results.sort_values(by="RMSLE")

            return results
        ```
        <br/>

- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?** 
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인 또는
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.
     
        <br/>
        
      >기존의 예시 코드(baseline) 에서 제안한 학습 모델들의 성능을 평가하였고, 더욱 향상된 학습 모델을 구현하기 위해 새로운 시도를 하였습니다. <br/> 특히 grid search 및 averaging blending 을 사용하여, 다양한 parameter values 를 가진 여러 개의 model instances 를 생성하고 각각의 예측 결과를 조합함으로써 최상의 학습 성능을 유도한 것이 매우 인상깊습니다.
      
        ```
        models = [
            {"model": xgb, "name": "XGBoost"},
            {"model": lgbm, "name": "LightGBM"},
            {"model": lgbm2, "name": "LightGBM2"},
            {"model": lgbm3, "name": "LightGBM3"},
            {"model": lgbm4, "name": "LightGBM4"},
            {"model": lgbm5, "name": "LightGBM5"},
        ]
        ```
        <br/>
      
        
- [x]  **4. 회고를 잘 작성했나요?** 
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 상세히 기록되어 있는지 확인
        - 딥러닝 모델의 경우,
        인풋이 들어가 최종적으로 아웃풋이 나오기까지의 전체 흐름을 도식화하여 
        모델 아키텍쳐에 대한 이해를 돕고 있는지 확인

        <br/>
     
      >회고록을 통해 과제 요약, 해결 방법과 어려웠던 부분을 명확하게 언급하였습니다
      ```
      위 프로젝트는 2019년 케글에서 주최한 대회에서 나온 문제를 실습해본 것으로, 주거 공간의 면적, 위치, 경관, 건물의 연식 등 여러 가지 복잡한 요인들을 조합하여 집의 실제 가격을 예상하는 문제입니다. 목표 점수는 11만 Score 이하로 받는 것이었고, 여러차례의 시도 끝에 11만 Score 이하의 점수를 얻을 수 있었습니다.  
      위 프로젝트를 통해 전처리의 중요성에 대해 배웠고, XGBRegressor와 LGBMRegressor의 사용법에 대해 알게되었습니다. 
      또한, 같은 형태의 모델일지라도 하이퍼 파라미터 값을 어떻게 주느냐에 따라서 성능이 크게 달라질 수 있다는 사실을 알았습니다.  
      그리고 이 과정에서 최적의 하이퍼 파라미터들을 찾기위해 수 많은 시행착오가 필요하다는 것을 배웠습니다. 
      중간에 이미지를 첨부한 부분은 모델의 학습 시간이 너무 오래걸려서 프로젝트 진행시간 동안 다시 돌리기에는 부적절하다 판단하여 그 기록만을 이미지로 첨부하오니 이점 양해 바랍니다. 
      마지막에 첨부한 이미지는 케글에 등록하여 점수를 받은 내용으로 가장 최근에 받은 점수를 참고해주시면 감사하겠습니다.
        ```
        <br/>
        

- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 모듈화(함수화) 했는지
        - 잘 작성되었다고 생각되는 부분을 근거로 첨부합니다.
      
      <br/>
      
      > 자주 사용하는 기능 (grid search 등) 에 대하여 메소드를 정의하여 코드 중복을 최소화하였고, 코드 전반적으로 목적을 알기 쉬운 변수명/함수명을 사용하였고, 적절한 띄어쓰기 등을 사용하여 가독성을 높이고 있습니다.
      ```
      def my_GridSearch(model, train, y, param_grid, verbose=2, n_jobs=-1):
      ```
      ```
      for c in skew_columns:
          train_data[c] = np.log1p(train_data[c].values)
          test_data[c] = np.log1p(test_data[c].values)
      ```

<br/>
<br/>

# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```

🎀💝 **꼼꼼하고 성실하신 모습으로 동기를 부여해주시는 찬중님**  ⭐️✨  
좋은 코드를 볼 수 있는 기회를 주셔서 감사합니다.  
grid search, averaging handling 등 다양한 학습 전략 기법을 배웠지만 어떻게 활용하면 좋을지 막막했었는데, 덕분에 좋은 motivation 을 얻을 수 있었습니다 😀
