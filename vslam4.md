# camera model

## 양안 카메라

카메라에서 하나의 픽셀을 기반으로 3D위치를 특정 지을 수 없다.
    -> **양안 카메라에서 깊이를 이용해 3D깊이를 정함**

![ex_screenshot](./img/slam4_depth.png)

핀홀 카메라가 두 개 있다고 함.
삼각형 닮음의 원리를 이용해 d를 계산
그렇지만 d를 찾는 것이 어려움

### stereo matching 
왼쪽 카메라에 찍은 부분이 오른쪽 카메라의 어디에서 찍혔는지 알고 계산
- 최근에는 딥러닝 등 다양한 연구가 진행되고 있음.
- 잘 만들어진 라이브러리는 한 쪽 카메라만 기준으로 원점을 계산

## RGB-D camera

### structure light
- 능동적으로 빛을 쏘고 받아서 3D 포지션 추정
- single camera에서 encoding 과정에서 stride id를 얻어 stereo camera와 비슷한 방식을 사용함.
- encoding, decoding과정을 거쳐서 

### time of flight
- 각 페이지마다 2개의 receptor (앞뒤로)
- 물체가 앞에 있으면 빛이 A만 들어오는데 일반적으로 그렇지 않아 그 차이를 계산하고 픽셀마다 depth를 계산

### 위 두개의 limit
- 빛을 쏘고 받아야 해서 적외선 씀 -> 쓸 수 있는 범위가 제한되어 있음.(ex 간섭)
- 투명한 물질에 반응할 수 없음.
    - 실내의 유리 등을 handle하지 못함.

## Deep Learning base approach

### depth map prediction
- 2014에 처음 나옴, depth를 잘 계산하지만 smooth하지 못했음
- 그래디언트 등의 요소를 넣어 자연스러운 3d constructor가 가능, 딥러닝 방법 적용

### learning to fly by crashing
- 부딫혀 보면서 정보를 바탕으로 거리 추정(learning 기반, 강화 학습은 아님)
- 물체의 imformation, 경험 등이 들어가 잘 작동

---
<BR>

## image data

![ex_screenshot](./img/slam4_image_data.png)

- 행렬마다 밝기값을 가지고 있음

- intensity 값은 0~255값(8bit)
- RGB image -> 1픽셀이 8*3= 24 비트의 저장공간
- RGB-D 이미지인 경우 0~65536값(16bit 미터기로 표시할 경우 최대 65m까지 가능)

### 주의할 점
- 기존의 카메라 좌표계는 오른손 좌표를 따라 X는 우측, Y는 좌측, Z는 이미지 방향인데 행렬로 표현되었을 때 
- 이미지는 img[h][w]
- 픽셀은 IMG[Y][X]로 표현됨.

- opencv는 BGR 컬러스페이스를 사용함.(imshow를 사용하면 컬러스페이스에 있어서 문제 없음,다만 기타 라이브러리를 동시에 이용할 때 사용하는 컬러스페이스가 다를 경우 문제가 생김)

- OPENCV는 intensity값을 [B,G,R] 순 작성, matplotlib같은 라이브러리는 [R, G, B]순 (자동으로 찾아주지 않아 수동으로 수정해줘야 됨.)



- 