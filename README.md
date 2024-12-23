# 카툰렌더링에 대한 조사
![0.gif](/Images/0.gif)

## 목차
[1. 렌더링 파이프라인](#1-렌더링-파이프라인)</br>
[2. 사전 준비](#2-사전-준비)</br>
[3. 아웃라인 쉐이더](#3-아웃라인-쉐이더)</br>


## 1. 렌더링 파이프라인
카툰 렌더링을 만들기 위해서는 기본적으로 렌더링 파이프라인에 대한 이해가 필요하다.

렌더링 파이프라인은 기본적으로 다음과 같은 순서로 이루워진다.
- Input Assembler
- Vertex Shader
- Rasterization
- Fragment Shader
### Input Assembler
Input Assembler 단계는 CPU가 렌더링을 수행할 도형의 정점 정보를 vertex buffer에 담아 GPU에 전달하는 과정이다.</br>
### Vertex Shader
vertex buffer에 담겨있던 정점의 3D좌표를 2D로 변환하는 과정이다.
이 과정을 통해 카메라에 그려질 vertex를 가려낸다.
### Rasterization
vertex shader에서 변환된 2D좌표를 화면에 표시될 픽셀로 변환하는 과정이다.
보간법을 사용하여 각 픽셀을 결정하며 이 과정은 프로그래밍 불가능하다.
### Fragment Shader
래스터화된 도형의 픽셀에 색을 입히는 과정이다.
매핑, 라이팅등의 처리를 할 수 있다.


카툰 렌더링을 하기 위해서 VertexShader, Fragment Shader를 작성해야한다.


## 2. 사전 준비

준비된 모델은 니케 홈페이지의 MMD모델을 유니티에 임포트하였다.
모든 매터리얼은 Unlit 쉐이더를 적용하여 적절한 텍스쳐를 입혀주었다.
![1.png](/Images/1.png)

## 3. 아웃라인 쉐이더
카툰 렌더링은 3D오브젝트를 CellAnimation 기법의 2D처럼 보이게 하는 것이 목적이다. 따라서 뚜렷한 외곽선을 보여줄 필요가 있었다.

캐릭터의 외곽선을 표현하기에 가장 단순한 방법은 조금 더 큰 크기의 검정색 캐릭터를 원본 캐릭터와 겹쳐놓는 방법이다.

캐릭터의 피봇을 기준으로 확대를 하게되면 외곽선이 아니라 단순히 모델이 겹쳐있는 것으로 보일 것이다.
![2.png](/Images/2.png)
[이런 느낌이다.]

따라서 캐릭터의 표면(노말)을 기준으로

<작성중>
