# 카툰렌더링에 대한 조사

## 목차
[1. 렌더링 파이프라인](#1-렌더링-파이프라인)</br>
[2. 아웃라인 쉐이더](#2-아웃라인-쉐이더)</br>


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

## 2. 아웃라인 쉐이더
카툰 렌더링은 3D오브젝트를 CellAnimation 기법의 2D처럼 보이게 하는 것이 목적이다. 따라서 뚜렷한 외곽선을 보여줄 필요가 있다.


외곽선을 만들기 위하여 언릿 쉐이더를 생성하고 

<작성중입니다.>