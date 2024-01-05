# AIFFEL Campus Online Code Peer Review Templete
- 코더 : 김찬중
- 리뷰어 : 강다은


# PRT(Peer Review Template)
- [x]  **1. 주어진 문제를 해결하는 완성된 코드가 제출되었나요?**
    - 문제에서 요구하는 최종 결과물이 첨부되었는지 확인
    - 문제를 해결하는 완성된 코드란 프로젝트 루브릭 3개 중 2개, 
    퀘스트 문제 요구조건 등을 지칭
        - 해당 조건을 만족하는 코드를 캡쳐해 근거로 첨부
      
      <br/>

      >요구사항대로 CAM, Grad-CAM 이미지를 생성하고, `visualize_cam_on_image()` 함수 구현을 통해 이미지 합성을 시도하였습니다.
      
      <br/>

      ```
      def generate_cam(model, item):
          ...
          return cam_image


      def generate_grad_cam(model, activation_layer, item):
          ...
          return grad_cam_image


      def visualize_cam_on_image(src1, src2, alpha=0.5):
          ...
          return merged_image
      ```

      <img width="307" alt="image" src="https://github.com/DiANA-KANG/Aiffel_Quest_CJK/assets/149550222/53b0b0db-255e-41d9-afc9-4812fefe23fa">
      <img width="347" alt="image" src="https://github.com/DiANA-KANG/Aiffel_Quest_CJK/assets/149550222/0dabaa7e-ac42-49ae-ac30-4a7e461b0a6a">

      <br/>
      <br/>


     
    
- [x]  **2. 전체 코드에서 가장 핵심적이거나 가장 복잡하고 이해하기 어려운 부분에 작성된 
주석 또는 doc string을 보고 해당 코드가 잘 이해되었나요?**
    - 해당 코드 블럭에 doc string/annotation이 달려 있는지 확인
    - 해당 코드가 무슨 기능을 하는지, 왜 그렇게 짜여진건지, 작동 메커니즘이 뭔지 기술.
    - 주석을 보고 코드 이해가 잘 되었는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        
      <br/>

      >CAM, Grad-CAM 이미지 생성 등 핵심 기능에 필요한 함수들이 작동하는 과정을 주석을 통해 쉽게 설명하였다.
      
      <br/>

      ```
      def generate_cam(model, item):
          item = copy.deepcopy(item)
          width = item['image'].shape[1]
          height = item['image'].shape[0]

      img_tensor, class_idx = normalize_and_resize_img(item)

      # 학습한 모델에서 원하는 Layer의 output을 얻기 위해서 모델의 input과 output을 새롭게 정의해줍니다.
      cam_model = tf.keras.models.Model([model.inputs], [model.layers[-3].output, model.output])
      conv_outputs, predictions = cam_model(tf.expand_dims(img_tensor, 0))

      conv_outputs = conv_outputs[0, :, :, :]

      # 모델의 weight activation은 마지막 layer에 있습니다.
      class_weights = model.layers[-1].get_weights()[0]

      cam_image = np.zeros(dtype=np.float32, shape=conv_outputs.shape[0:2])
      for i, w in enumerate(class_weights[:, class_idx]):
          # conv_outputs의 i번째 채널과 i번째 weight를 곱해서 누적하면 활성화된 정도가 나타날 겁니다.
          cam_image += w * conv_outputs[:, :, i]

      cam_image /= np.max(cam_image) # activation score를 normalize합니다.
      cam_image = cam_image.numpy()
      cam_image = cv2.resize(cam_image, (width, height)) # 원래 이미지의 크기로 resize합니다.
      return cam_image

      ```

      <br/>
     
     
        
- [x]  **3. 에러가 난 부분을 디버깅하여 문제를 “해결한 기록을 남겼거나” 
”새로운 시도 또는 추가 실험을 수행”해봤나요?**
    - 문제 원인 및 해결 과정을 잘 기록하였는지 확인
    - 문제에서 요구하는 조건에 더해 추가적으로 수행한 나만의 시도, 
    실험이 기록되어 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
        
      <br/>

      >기존의 bounding box 추출 함수의 성능을 높이기 위하여 코드를 일부 수정하였습니다. 그 결과 탐지된 feature 부분에 맞게 bounding box가 잘 표시되는 것을 확인하였습니다.
      
      <br/>

      ```
      def get_gbbox(cam_image, score_thresh=0.05):
      # cam_image 내의 점수가 score_thresh 이하인 픽셀을 0으로 설정합니다.
      # 이는 CAM 이미지에서 중요하지 않은 부분을 제거하는데 도움이 됩니다.
      low_indicies = cam_image <= score_thresh
      cam_image[low_indicies] = 0

      # CAM 이미지를 255로 스케일링하여 정수 형태의 이미지로 변환합니다.
      # 이는 findContours 함수가 uint8 형태의 이미지를 요구하기 때문입니다.
      cam_image = (cam_image * 255).astype(np.uint8)

      # findContours 함수를 사용하여 이미지에서 윤곽선(contours)를 찾습니다.
      # cv2.RETR_TREE는 모든 윤곽선을 찾고, 계층 정보를 구성합니다.
      # cv2.CHAIN_APPROX_SIMPLE은 윤곽선을 단순화하여 불필요한 점을 줄입니다.
      contours, _ = cv2.findContours(cam_image, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

      #     # contours 리스트에서 첫 번째 윤곽선을 가져옵니다.
      #     # 이 예제에서는 가장 큰 윤곽선을 가져오는 것을 가정합니다.
      #     # 다양한 윤곽선 중 원하는 것을 선택하는 로직을 추가할 수도 있습니다.
      #     cnt = contours[0]

      #     # minAreaRect 함수는 윤곽선을 포함하는 최소 크기의 회전된 직사각형을 반환합니다.
      #     rotated_rect = cv2.minAreaRect(cnt)

      #     # boxPoints 함수는 회전된 직사각형의 4개 모서리 포인트를 반환합니다.
      #     rect = cv2.boxPoints(rotated_rect)

      #     # rect의 포인트들을 정수형 좌표로 변환합니다.
      #     rect = np.int0(rect)

      #     return rect

      # Grad-CAM에서는 기존 노드와 달리 가장 큰 윤곽선을 선택하는 코드를 사용합니다.

      # 가장 큰 윤곽선 선택
      max_area = 0
      max_contour = None
      for contour in contours:
          area = cv2.contourArea(contour)
          if area > max_area:
              max_area = area
              max_contour = contour

      # 가장 큰 윤곽선이 없는 경우, None 반환
      if max_contour is None:
          return None

      # 선택된 윤곽선으로 회전된 직사각형 구하기
      rotated_rect = cv2.minAreaRect(max_contour)
      rect = cv2.boxPoints(rotated_rect)
      rect = np.int0(rect)

      return rect
      ```

      <img width="312" alt="image" src="https://github.com/DiANA-KANG/Aiffel_Quest_CJK/assets/149550222/c58b3265-f7c1-4698-aa55-4f5bcb17509f">
      
      <br/>
      <br/>


     
        
- [x]  **4. 회고를 잘 작성했나요?**
    - 주어진 문제를 해결하는 완성된 코드 내지 프로젝트 결과물에 대해
    배운점과 아쉬운점, 느낀점 등이 기록되어 있는지 확인
    - 전체 코드 실행 플로우를 그래프로 그려서 이해를 돕고 있는지 확인
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
    
      <br/>

      >프로젝트 수행 내용을 다시 한 번 요약하고, 수행 과정에서 배운점과 아쉬운점, 앞으로의 발전 계획에 대하여 구체적으로 언급하였다.
      
      <br/>

      ```
      ## 회고록
      - 본 프로젝트는 CAM과 Grad-CAM을 사용하여 object의 위치를 찾고 적절한 위치에 B-Box를 그리는 것을 목표로 하고 있습니다. 베이스 모델은 ResNet50을 사용하였고, 데이터 셋으로는 'stanford_dogs'을 사용하였습니다. CAM 방식과 Grad-CAM 방식의 성능 비교를 위해서 IOU 계산을 통해 성능을 비교 하였습니다.
      - 느낀점 : 에폭이 적다고 꼭 학습이 부족한 것 만은 아니며, 에폭을 높인다고 항상 좋은 결과가 나오는 것은 아니라는 것을 느꼈습니다.
      - 배운점 : CAM과 Grad-CAM을 통해물체의 위치를 찾고 그 위치의 좌표를 이용하여 Bounding Box를 그리는 방법에 대해 배웠고, 이를 IOU 계산을 통해 평가하는 방법에 대해 배웠습니다. 또한, CAM의 과정을 시각화 하면서 단계별로 진행 상황을 알 수 있었습니다.
      - 아쉬운 점 :
          - Grad-CAM에서 바운딩 박스를 정확하게 잡지 못하는 현상이 있었고, 이를 해결하기 위해 GPT를 사용하였습니다. 이를 통해 BBOX를 엉뚱하게 하는 문제는 해결 했으나 만족할 만한 성과를 얻지는 못했습니다.
      - for문을 사용하여 여러 데이터를 한번에 뽑아내어 비교 하였으나 이를 함수화하여 사용하지는 못했습니다. 다음에는 좀더 시간을 들여서 함수화까지 하도록 노력하겠습니다.
      ```

      <br/>


     
- [x]  **5. 코드가 간결하고 효율적인가요?**
    - 파이썬 스타일 가이드 (PEP8) 를 준수하였는지 확인
    - 하드코딩을 하지않고 함수화, 모듈화가 가능한 부분은 함수를 만들거나 클래스로 짰는지
    - 코드 중복을 최소화하고 범용적으로 사용할 수 있도록 함수화했는지
        - 잘 작성되었다고 생각되는 부분을 캡쳐해 근거로 첨부합니다.
      
      <br/>

      >반복적인 실험을 하드 코딩을 통해 수행하지 않고 FOR 반복문을 통해 코드 반복을 최소화하였다.
      
      <br/>

      ```
      gcam_iou_scores = []
      for i in range(num):
          gcam_iou_score = get_iou(pred_bbox_grads[i], images[i]['objects']['bbox'][0])
          gcam_iou_scores.append(gcam_iou_score)
          print(gcam_iou_score)
      ```

     <br/>
     


# 참고 링크 및 코드 개선
```
# 코드 리뷰 시 참고한 링크가 있다면 링크와 간략한 설명을 첨부합니다.
# 코드 리뷰를 통해 개선한 코드가 있다면 코드와 간략한 설명을 첨부합니다.
```

