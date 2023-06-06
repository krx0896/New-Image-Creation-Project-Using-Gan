# Project
### 스케치 이미지를 초상화 이미지로 변환하는 인공지능: cGAN 과 U-Net 아키텍처 활용

# Intro 
### 🗓️ Date 
Project term : 2023.05.10 ~ 2023.06.06 </br>
Presentation Date : 2023.06.07 </br>


## 1.  Introduction

이 프로젝트는 스케치 이미지를 활용하여 초상화 이미지로 변환하는 인공지능을 구현하는 과정을 진행하였다. 다음은 프로젝트의 주요 단계와 내용에 대한 설명이다:

1.	스케치 이미지 데이터 수집: 우선적으로 스케치 이미지 데이터를 찾아서 수집하였다. 이는 스케치 공개 데이터셋 등에서 얻을 수 있었다.


2.	모델 선택: 모델 성능 향상을 위해 GAN 기반의 다양한 모델을 탐색하던 중 U-Net 아키텍처를 사용하여 GAN 모델을 활용한 논문을 찾았다. U-Net은 다운 샘플링 단계에서 추출한 특성 맵을 업 샘플링 단계에서 병합하는 과정에서, 이전 단계의 특성 맵과 연결하여 정보의 유실을 방지하는 효과가 있다. 이 때문에 이미지 분할 작업에 효과적인 네트워크 구조이므로 선택하게 되었다.


3.	데이터 전처리: 수집한 스케치 이미지 데이터를 전처리 하였다. 레퍼런스 모델 코드의 데이터셋이 달랐기에 주로 이미지 사이즈를 조정하고, 스케치 이미지와 해당하는 초상화 이미지를 합쳐서 쌍을 만들어주었다.

4.	데이터 증강: 기존의 데이터보다 다양성을 높이기 위해 데이터 증강 기법을 적용했다. 크기 랜덤 조정, 이동, 반전 등의 변환을 적용하여 데이터셋을 다양하게 만들어 주었다.

5.	레이어 추가: 모델의 성능을 더욱 향상시키기 위해 추가적인 레이어를 모델에 추가하였다. Conv2d 레이어만 있는 모델에서 특징 추출을 잘 할 수 있게 Residual Connections 레이어를 적용하였다. 이 레이어는 이미지 분류에 많이 사용되는Resnet모델에서 사용되는 레이어로 이전 Convolution레이어에서 출력을 다음 레이어에 다하는 방식으로 모델이 더 깊어지더라도 Gradient 소실과 표현 능력 감소를 방지할 수 있다.

6.	하이퍼 파라미터 조정: 모델의 성능을 최적화하기 위해 간단한 하이퍼 파라미터를 조정하였다. Unet은 파라미터를 조정하기에 용이한 특성을 가지고 있어서 파라미터 튜닝에 대한 부담이 적었다.

7.	모델 비교: 추가한 레이어를 포함한 모델과 Conv2d 레이어만 있는 모델을 비교하였다. 결과적으로 Residual Connections 레이어가 있는 모델 학습이 더 잘 될 거라는 생각과 다르게 Conv2d 레이어만 있는 모델이 더 다양하고 그럴싸한 이미지를 만들어 내었고 Residual Connections 레이어가 있는 모델은 오히려 대부분 비슷한 이미지로 출력하였다. 이는 학습이 모델의 복잡성을 충분히 다루지 못하고 일부 정보만을 학습하는 경향이 생긴 것으로 판단하였다. 

8.	GPU 문제와 파라미터 저장: 학습을 계속 진행하면 GPU 문제가 발생하여 학습을 여러 번 나눠서 진행하였다. 중간에 파라미터를 저장하고 다시 그 파라미터를 불러와서 학습을 이어갈 수 있도록 하였다.

9.	모델 성능 평가: 학습된 모델의 성능을 평가하기 위해 얼굴 형태를 추론하는 OpenCV의 DNN모듈을 불러와서 생성한 이미지에 적용해 보았다. 결과적으로 생성된 이미지가 얼굴로 잘 인식되었음을 확인할 수 있었다.

10.	직접 스케치 이미지 테스트: 마지막으로 직접 만든 스케치 이미지를 Generator에 입력하여 초상화 이미지가 만들어지는 것을 시연하였다. 이를 통해 프로젝트의 결과물을 실제로 확인하고 시각적으로 보여줄 수 있었다.

이러한 과정을 통해 U-Net 구조의 cGAN을 활용하여 스케치 이미지를 초상화 이미지로 변환하는 인공지능을 구현하였다.


## 2.	Results 
-	cGAN , 픽셀 단위 L1, 판별자 Loss를 활용하여 모델을 학습시킴 
 
-	처음 학습된 결과 이미지를 보면 아직 특징을 추출하지 못하여 생성된 이미지가 자연스럽지 못함
 
<img src="./Image/일반 모델의 초기 학습 결과.png" width="500px">
 
<제일 처음 학습된 결과 이미지>

-	중간 과정에서 학습된 결과 이미지를 보면 생성된 이미지는 비슷하지만 스케치 형태를 잘 반영한 것을 볼 수 있음
 
<img src="./Image/일반 모델의 중기 학습 결과.png" width="500px">
 
<중간 과정에서 학습된 결과 이미지>

-	마지막에서 학습된 결과 이미지를 보면 생성된 이미지와 실제 이미지와 많이 흡사한 것을 볼 수 있음
 
<img src="./Image/일반 모델의 마지막 학습 결과.png" width="500px">
 
<마지막에서 학습된 결과 이미지>

-	Residual Connections레이어 추가한 모델의 결과 이미지는 Fig6의 Conv2d레이어 만 있는 모델보다 생성된 사진이 단조로운 것을 볼 수 있음
 
<img src="./Image/Residual Connections레이어 모델의 마지막 결과.png" width="500px">
 
<Residual Connections레이어 추가한 모델의 결과 이미지>

-	생성된 마지막 이미지를 OpenCV의 얼굴 감지 모듈로 확인 한 결과 실제 이미지와 비슷하게 얼굴이 인식됨
 
 <img src="./Image/OpenCV 얼굴인식모듈을 활용하여 시각화.png" width="500px">
 
<생성된 이미지의 얼굴이 잘 나왔는지 OpenCV 얼굴 감지 모듈로 확인>

-	직접 그린 스케치 이미지 모델에 넣고 생성한 결과 자연스럽지는 않지만 준수한 초상화 이미지를 생성함

<img src="./Image/직접 그린 스케치 이미지 Generator로 이미지 생성.png" width="300px">

<직접 그린 스케치 이미지를 모델에 넣고 이미지를 생성하여 확인>


## 3.	How to run 
이 프로젝트는 Google Colab 환경에서 구현을 하였고 Google Drive를 연동하여 보다 빠르게 데이터를 불러올 수 있다. 프로젝트 파일에 있는 데이터셋 파일 그대로 본인 Drive에 올린 다음 코드를 실행하면 된다.
모델의 학습 속도가 느리고 코드를 실행하는 과정에서 GPU가 제한될 수 있기 때문에 파일에 있는 이미 학습된 모델인 Pix2Pix_Discriminator2.pt, Pix2Pix_Generator2.pt 파일을 사용하면 (1)~(4)까지 과정을 실행한 후 (7) Evaluation부터 바로 실행시켜 보다 빠르게 성능을 평가하고 이미지를 만들 수 있다.

## 4. References
데이터셋</br>
- Flickr, Portrait bldigital dataset, https://www.kaggle.com/datasets/kairess/edges2portrait 

</br>참고  논문
- Isola, Phillip, et al. "Image-to-image translation with conditional adversarial networks." Proceedings of the IEEE conference on computer vision and pattern recognition. 2017.

</br>참고자료
- TensorFlow, “pix2pix: 조건부  GAN 을  사용한  이미지  대  이미지 변환”, https://www.tensorflow.org/tutorials/generative/pix2pix?hl=ko
- 동빈나, “Image-to-Image Translation with Conditional Adversarial Networks [꼼꼼한  딥러닝  논문 리뷰와  코드 실습]”, https://www.youtube.com/watch?v=ImiD4npRj7k
- 빵형의  개발도상국, “[GAN] 초상화를  그리는  인공지능  - Python, Deep Learning”, https://www.youtube.com/watch?v=yOLE9aCWAN4
- kairess, “edges2portrait_gan”, https://github.com/kairess/edges2portrait_gan/tree/master
