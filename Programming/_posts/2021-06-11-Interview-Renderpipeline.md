---
layout: post
title: '면접 질문 정리 2 - 렌더 파이프라인'
tags: [Unity]
image : /assets/img/post/2021-06-11-Interview-Renderpipeline/main.jpg
description: >
  두번째로 정리해 볼 면접 질문은 "유니티의 렌더 파이프라인에 대해서" 입니다.
---

* toc
{:toc .large-only}
# 렌더 파이프라인

## 렌더 파이프라인이란,

유니티 개발자라면, '렌더파이프라인'이라는 말은 한 번쯤은 들어봤을 것입니다. 저도 듣기도 많이 들어보고 대충 렌더링하는 과정 정도로 알고 있었지만, 면접 질문에서 그렇게 대답할 수는 없었습니다.. 

'렌더 파이프라인'에 대한 [유니티 매뉴얼의 정의](https://docs.unity3d.com/Manual/render-pipelines.html){:target="_blank"}는 이렇습니다.

> 씬의 콘텐츠를 가져와 화면에 표시하는 일련의 작업을 수행하는 것. 컬링, 렌더링, 포스트 프로세싱 등을 수행한다.
{:.em .lead}

다시 한번 풀어쓰면, 

**유니티 씬에서만 존재했던 물체들(오브젝트, 콘텐츠)에 대한 데이터를 가지고, 컬링, 렌더링, 포스트 프로세싱 등의 작업을 수행해서, 우리가 보고 있는 스크린에 표시하는 역할**
{:.lead}

을 하는 것이 바로 '렌더 파이프라인'입니다. 

---

사실 이 정도는 얘기할 수 있었을 것 같은데, "렌더파이프라인에 대해 설명해보세요."라는 질문이었는지, "렌더파이프라인 과정에 대해 얘기해보세요."라는 질문이었는지 기억은 잘 나지 않지만, 당시 저는 후자로 이해했고, 사실 과정까지는 몰랐기 때문에 대답을 못 했던 것이 조금 아쉽습니다. 그냥 아는 대로 라도 이야기할 것 그랬습니다.
{:.note title="사담"}

---

유니티는 다양한 렌더 파이프라인을 제공합니다. 각각의 렌더 파이프라인은 저마다 다른 기능과 성능 특성을 가지고 있어서, 개발자는 자신의 어플리케이션, 플랫폼에 따라 적절한 렌더 파이프라인을 선택해야 합니다.

## 유니티가 제공 중인 '렌더 파이프라인'

유니티는 렌더 파이프라인은 두 종류로 나눌 수 있습니다.

'빌트인 렌더 파이프라인'은 유니티의 기본 렌더 파이프라인으로 가장 범용으로 사용되며, 커스터마이즈가 제한적입니다. 대신, 포워드 렌더링, 디퍼드 셰이딩, 레거시 디퍼드, 버텍스 릿 등 다양한 렌더링 경로를 선택할 수 있습니다. 기존의 있던 다양한 쉐이더를 그대로 사용할 수 있다는 장점이 있지만, 커스터마이징이 제한적이어서 성능의 이슈가 있을 수 있다는 단점도 있습니다.
{:.note title="Built-In Render Pipeline"}

'스크립터블 렌더 파이프라인'은 '빌트인 렌더 파이프라인'과는 다르게, 렌더링 과정을 C# 스크립트를 통해 제어하고 커스터마이징 할 수 있도록 하는 렌더 파이프라인입니다. 플랫폼에 맞게 렌더링 프로세스를 변경할 수 있어, 하드웨어의 성능을 최적화할 수 있다는 장점이 있지만, 직접 개발을 해야 하는 만큼 난이도가 있고, 그래픽스를 전문으로 다루는 프로그래머가 필요하다는 단점이 있습니다. 
{:.note title="SRP"}

 <sub>'스크립터블 렌더 파이프라인'을 만드는 방법까지 보려면 내용이 너무 방대지므로 이 정도로만 알고 있으면 될 것 같습니다. </sub>



## URP & HDRP

유니티는 자체적으로 2개의 스크립터블 렌더 파이프라인을 제공하고 있습니다.<sub>(2020.3 기준)</sub> 
**URP**라고 알려져있는 '유니버셜 렌더 파이프라인'과 **HDRP**로 부르는 '고해상도 렌더 파이프라인'입니다.

유니버셜 렌더 파이프라인은 쉽고 빠르게 커스터마이즈할 수 있는 스크립터블 렌더 파이프라인으로, 광범위한 플랫폼(모바일, PC, 콘솔, VR 등)에서 최적화된 그래픽스를 구현합니다. 기존 빌트인 렌더 파이프라인보다 확장성이 좋고, 저사양의 기기에서도 고성능을 낼 수 있습니다.
{:.note title="URP"}

고해상도 렌더 파이프라인은 유니버셜 렌더 파이프라인보다 고사양 플랫폼을 위한 고해상도 그래픽스를 구현합니다. 보다 사실적이고 뛰어난 비주얼을 제작하기 위한 고사양 하드웨어에서 그래픽스를 개선하는 데 이상적입니다.
{:.note title="HDRP"}

## 렌더 파이프라인의 과정

지금까지는 렌더 파이프라인이 어떤 것인지에 대해 이야기해봤다면, 이제는 렌더 파이프라인의 과정에 대해서 이야기해보도록 하겠습니다. 아까 렌더 파이프라인이란 **씬의 물체들을 여러 작업을 수행해서 스크린에 표시하는 과정**이라고 말했습니다. 그런 과정이 여러 단계에 걸쳐 나누어져 있는데, 그 과정에 대해 간략히 알아보도록 하겠습니다.  

1. 먼저 메쉬 정보를 읽습니다. 버텍스 위치, 노멀, 탄젠트 등 메쉬를 그리기 위해 필요한 정보를 램에 저장합니다.


2. CPU는 커맨드 버퍼를 통해 GPU에서 렌더링을 요청합니다. 커맨드 버퍼는 GPU 디바이스에 전달할 명령을 저장하는 큐(Queue)입니다. 


3. 커맨드 버퍼에는 렌더 스테이트(Render State)를 설정 요청과 메쉬를 그리라는 드로우콜(Draw Call) 요청이 들어갑니다. 

   렌더 스테이트는 메쉬가 그려지는 환경으로, 셰이더, 텍스쳐, 라이트 세팅 등을 포함합니다.


4. GPU는 커맨드 버퍼에 등록된 명령들을 하나씩 처리합니다. 먼저 버텍스 셰이더가 실행됩니다. 버텍스 셰이더의 역할은 메쉬의 정점들을 화면상의 점들로 변환하는 역할을 합니다. 

	Local 스페이스에서, World 스페이스, View 스페이스, Projection 스페이스 순으로 변환됩니다. 이 과정에서 행렬 변환이 사용됩니다. 
	버텍스의 위치뿐만 아니라, 노말, 컬러 등도 동일한 과정을 거칩니다. 


5. 변환된 메쉬를 화면에 그리기 위해서 래스터 변환(Rasterizing)을 합니다. 메쉬가 화면에 매칭되는 픽셀을 결정합니다. 

   각 픽셀에는 깊이 정보와 색상 정보가 포함됩니다. 깊이 정보를 통해, Depth Test가 이루어지고, 더 가까운 메쉬에 의해 가려지는 픽셀을 걸러냅니다.

6. 프래그먼트 셰이더를 통해 픽셀들의 최종 색을 계산합니다.

이외에 상황에 따라서, Backface Culling 이나, Alpha Blending, Stencil Test 등의 추가적으로 이루어지기도 합니다.

## 렌더 파이프라인이 바뀔 때

면접 중에 렌더 파이프라인과 관련된 질문 중의 하나는, 보통 프로젝트에 따라 URP를 쓰기도 하고, HDRP를 쓰기도 한다고 했더니 파이프라인이 바뀌면 머티리얼이나 텍스처 세팅 등이 바뀌는데 기존에 사용하던 에셋들을 새로운 파이프라인에서 사용하기 위해서 어떤 것을 했냐는 질문이었습니다.

사실, 저희는 매 프로젝트 때마다 디자인 에셋을 새로 만드는... 비효율적인 작업 파이프라인을 갖고 있습니다. 디자인팀이 별 말없이 항상 새로 만들기 때문에, 기존의 에셋을 재활용하는 방법은 별로 생각해보지 못한 것 같습니다. 하지만 낭비되는 자원들을 절약하기 위해서는 이러한 방법도 생각해봐야 할 것 같습니다.

먼저, 유니티에서는 

> 렌더 파이프라인은 서로 다른 셰이더 출력을 사용하고 서로 다른 기능을 보유하고 있기 때문에 한 렌더 파이프라인에서 다른 렌더 파이프라인으로 프로젝트를 전환하기가 어려울 수 있습니다.

라고 말합니다. 이렇듯 서로 다른 렌더 파이프라인의 가장 큰 문제는 서로 다른 셰이더를 사용한다는 점입니다.

일단, HDRP같은 경우에는 유니티에서 제공하는 HDRP 위자드를 이용하면 기존의 머티리얼에 적용되어 있던 셰이더를 HDRP용 셰이더로 바꾸는 것이 가능합니다. 
하지만, 이는 몇몇 기본 셰이더에만 적용이 되기 때문에, 추가적인 셰이더에 대해서도 URP용 셰이더와 HDRP용 셰이더를 매칭시켜주는 기능을 추가로 개발해야 할 것 같습니다. 

또한 셰이더를 바꾸더라도, 셰이더 내에 설정된 변수들의 이름이 다를 수 있으니 이름 또한 매칭시켜서 새로운 셰이더에 기존 셰이더에 저장된 변수들을 옮길 수도 있어야겠습니다.

렌더 파이프라인을 바꿀 때, 또 한 가지 문제가 되는 점은 빌트인 렌더 파이프라인과 스크립터블 렌더 파이프라인의 포스트 프로세싱 방식이 변경된다는 점입니다. 이를 처리하기 위해서는 포스트 프로세싱 정보를 가지고 있는 에셋을 서로 다른 버전으로 바꿀 수도 있어야겠습니다.

---

# 유니티와 그래픽스

정리하면서 느낀 점은 그래픽스 프로그램이 어려워졌다는 것입니다. 분명 학부 때, C++과 OpenGL을 쓰면서 컴퓨터 그래픽스 수업을 들을 때는 참 쉽고 재밌다는 생각을 많이 했는데, 유니티를 계속 쓰다 보니 아무래도 그래픽스 쪽으로는 점점 얕아져 버렸고, 어느새 기초적인 그래픽스조차 어려워 보이는 수준까지 된 것이 참 안타깝습니다. 

그래픽스 교수님께서 수업 때 항상 "툴과 라이브러리에 의존하지 말라"고 강조하셨었는데, 이제야 그게 어떤 의미인지를 조금씩 알아가고 있는 것 같습니다. 

유니티를 사용하는 것도 중요하지만, 유니티를 더 잘 사용하기 위해서 유니티에 의존하지 않는 연습도 계속해서 해야겠습니다.
