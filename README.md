# 카툰렌더링에 대한 조사
![0.gif](/Images/0.gif)

## 목차
[1. 렌더링 파이프라인](#1-렌더링-파이프라인)</br>
[2. 사전 준비](#2-사전-준비)</br>
[3. 아웃라인 쉐이더](#3-아웃라인-쉐이더)</br>
[4. 노멀과 라이트의 내적](#4-노멀과-라이트의-내적)</br>


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

이를 해결할 방법중 하나로 캐릭터의 표면(노말)을 기준으로 확장하고 Back face로 렌더링하는 방법이 있다.
![3.png](/Images/3.png)

하지만 이 방법은 날카로운 엣지에서는 다음과 같은 문제가 생긴다.
![4.png](/Images/4.png)
[유니티 로고 같이 생겼다]

노말 방향으로만 확장하기 때문에 엣지부분이 "아웃라인"처럼 보이지 않게 된다.

이것을 해결하는 방법으로는 SmoothNormal을 만들어주는 방법이 있다.

하드엣지로 분리된 버텍스들의 노멀을 평균을 내서 부드러운 노멀을 만들 수 있다.
만들어진 노멀은 탄젠트 채널에 저장해두고 아웃라인 쉐이더에서 사용할 수 있다.

탄젠트 채널은 노말맵을 사용할때 필요하므로 만약 노말맵이 필요한 매쉬라면 다른 UV채널을 사용해야만 한다.

```C#
    public Mesh mesh;
    void smoothNormals()
    {
        // 같은 위치의 정점들을 그룹핑
        var smoothNormals = new Dictionary<Vector3, Vector3>();
        
        for (int i = 0; i < mesh.vertexCount; i++)
        {
            var pos = mesh.vertices[i];
            if (!smoothNormals.ContainsKey(pos))
                smoothNormals[pos] = Vector3.zero;
            smoothNormals[pos] += mesh.normals[i]; // 노말 누적
        }
        
        // 평균 내서 탄젠트 채널에 저장
        var tangents = new Vector4[mesh.vertexCount];
        for (int i = 0; i < mesh.vertexCount; i++)
        {
            var avg = smoothNormals[mesh.vertices[i]].normalized;
            tangents[i] = new Vector4(avg.x, avg.y, avg.z, 0);
        }
        mesh.tangents = tangents;
    }
```
이 코드는 임포트시에 사용하면 될 것으로 추측된다.(이 문장은 이후의 실험 후에 수정하겠음)

![5.png](/Images/5.png)

계산된 Smooth Normal을 기존의 Normal과 대체하면 

![6.png](/Images/6.png)
이런 깔끔한 아웃라인을 만들 수 있다!

## 4. 노멀과 라이트의 내적
<작성중>