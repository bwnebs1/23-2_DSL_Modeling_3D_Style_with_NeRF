# 🎨 일상에 예술을 더하다, 3D Exhibition

🥇**DSL 23-2 모델링 프로젝트 우수팀 선정**🥇

프로젝트 결과물로 구성한 메타버스 전시회에 놀러오세요!  
↪️[🖼️ 메타버스 전시회](https://www.spatial.io/s/DSL-3D-Art-Exhibition-651d688e9f9634efe508c13a)

## 🏛️ Team Information

연세대학교 데이터사이언스 학회 Data Science Lab 9기 & 10기,  
**Team CV_B**,

| [<img src="https://github.com/bwnebs1.png" width="100px">](https://github.com/bwnebs1) | [<img src="https://github.com/ydduri.png" width="100px">](https://github.com/ydduri) | [<img src="https://github.com/youravgENTP.png" width="100px">](https://github.com/youravgENTP) | [<img src="https://github.com/smlim02.png" width="100px">](https://github.com/smlim02) |  
| :---: | :---: | :---: | :---: |  
| [김서진](https://github.com/bwnebs1) | [박서연](https://github.com/ydduri) | [윤형진](https://github.com/youravgENTP) | [임선민](https://github.com/smlim02) |

:film_strip: 발표 영상 &rarr; 추후 추가 예정  
[:books: 발표 자료](https://github.com/ydduri/DSL_23-2_3D_Exhibition/blob/main/CV_B_3D_Exhibition_%EB%B0%9C%ED%91%9C%EC%9E%90%EB%A3%8C.pdf)(Pull requests 후 링크 수정 예정)  

## :loud_sound: Project Introduction

**Data Science Lab 23-2 모델링 프로젝트**에서 선보인 **<ins>CV_B 특별전</ins>** 에 오신 여러분을 환영합니다!  

이번 전시가 특별한 이유는  
첫 번째, 일상적인 풍경을 화가들의 화풍으로 재해석했다는 것,  
두 번째, 이를 3D로 구현하여 입체감을 불어넣었다는 것입니다.  

전시는 작품 생성의 파이프라인을 따라 다음의 두 코너로 구성되어 있습니다.  
1. AdaIN을 통한 고전과의 만남
2. NeRF를 통해 살아난 입체

아래에 관람객 여러분의 이해를 도울 해설을 준비했으니, 함께 둘러보시겠습니다!

## ✒️ 1. Overall Pipeline

<p>
  <img src="https://github.com/ydduri/BOJ_mycode/assets/63230753/60370b8c-1f28-4e53-b0fc-3001491ee991" width="800">
</p>

* Style Transfer 모델 **AdaIN(Adaptive Instance Normalization)**
* 3D view를 생성하는 모델 **NeRF(Neural Radiance Fields)**

를 활용하여, 기존의 NeRF 데이터셋에 화풍을 입히고 이를 3D로 구현하는 하나의 파이프라인을 완성하였습니다.  

## 🗂️ 2.Dataset

1. [**NeRF LLFF(Local Light Field Fusion)**](https://drive.google.com/drive/folders/128yBriW1IG_3NJ5Rp7APSTZsJqdJdfc1)
   * 일정 거리에서 front-facing
   * 고해상도
   * 8개의 scene

2. [**Best Artworks of All Time**](https://www.kaggle.com/datasets/ikarus777/best-artworks-of-all-time)

## :desktop_computer: 3. Model

### 1) AdaIN

<p>
  <img src="https://github.com/ydduri/BOJ_mycode/assets/63230753/d5f4b928-3d4f-40b5-982c-b3536e554679" width="800">
</p>

style transfer 모델인 **<ins>AdaIN</ins>** 은 Adaptive Instance Normalization의 약자로,  
adaptive라는 이름처럼 미리 학습하지 않은 이미지에 대해서도 transfer가 가능합니다.   

기존 CIN(Conditional Instance Normalization)의 learned parameter $\beta, \gamma$를  
스타일 이미지의 평균과 분산이라는 통계량으로 대체하여  
1. 학습시켜야 할 파라미터를 줄이면서
2. 임의의 스타일에 대해서도 적용 가능한 모델을 구축했습니다.
<br>

🎯**Q**. style transfer를 위해 **<ins>AdaIN</ins>** 을 선택한 이유?<br><br>
&rarr; 이번 프로젝트를 통한 최종적인 목표는 style transfer와 NeRF 모델을 합쳐 **end-to-end**로  
스타일링된 이미지의 3D view를 생성할 수 있는 모델을 구축하는 것입니다.  
  1. NeRF의 구동 시간이 오래 걸리는 만큼 style transfer 과정을 최대한 간소화  
     (임의의 이미지에 대해서도 구동 가능하면서 GAN처럼 heavy하지 않은 AdaIN 선택)
  2. Style-GAN 등 style transfer GAN 내부에서 핵심적인 스타일링 임무 담당하는 것도 사실은 AdaIN

### 2) NeRF

<p>
  <img src="https://github.com/ydduri/BOJ_mycode/assets/63230753/6f59e6ad-5289-4146-82cb-ef971ff1d322" width="800">
</p>

**<ins>NeRF</ins>** 는 9개의 FC layer로 구성된(MLP) 3D view synthesis 모델입니다.  
view synthesis란 몇 개의 시점에서 촬영된 불연속적 이미지로부터 알지 못하는 시점에서의 모습을 추측하여  
이미지가 연속적으로 구성될 수 있도록 하는 기술입니다.  

일부 시점에서의 2D 이미지만 주어져도, 나머지 시점에서의 이미지들을 생성해낼 수 있기 때문에  
이들을 모두 합치면 물체를 입체적으로 보는 것과 같은 효과를 얻을 수 있는 것입니다.  

* input: 물체의 위치 정보 (x, y, z), 방향 정보 ($\theta, \phi$)  
  In our model...,
  * 위치 정보 (x, y, z): stylized 이미지의 값
  * 방향 정보 ($\theta, \phi$): 기존 NeRF 데이터셋(LLFF)의 값
* output: (새롭게 생성하고픈 view에서의) 물체의 RGB값, density값(투명도의 역수)

1. 새롭게 생성하고픈 view로부터 물체를 향해 ray 발사
2. ray 상의 여러 포인트 sampling &rarr; 각 포인트에서의 output 예측
   * 처음 8개 FC layer: 위치 정보 (x, y, z)만 통과시켜 density 예측
   * 마지막 1개 FC layer: 방향 정보 ($\theta, \phi$)를 합쳐 RGB 예측

**(1) Positional Encoding**  
5차원의 저차원 input &rarr; 고차원으로 매핑, high-frequency 정보 보존

**(2) Volume Rendering**  
모델의 output인 한 ray상의 여러 sample 포인트에서의 RGB, density값을 하나의 pixel로 병합

## 🖼️ Result
### 1. Final Output
(결과 이미지 등)  

## 2. Limitations and Future works
(의의, 한계, 발전방향 등등)

# 🏃 How to Run?
(optional, 가능하다면 코드를 다시 실행해볼 수 있도록 코드 블럭을 활용하여 완성해주시면 좋을 것 같습니다.)
(ex)

```ruby
python run.py --config configs.txt
```

# File description
- main (실제 구동하는 파일)
  - ```main.py```  
- model (모델 내부 구조 파일)
  - ```encoder.py```
  - ```decoder.py```
- data (사용한 데이터 or 데이터 생성 파일)
(예시입니다! 각 팀의 프로젝트 파일 구조에 따라 자유롭게 완성해주세요)
