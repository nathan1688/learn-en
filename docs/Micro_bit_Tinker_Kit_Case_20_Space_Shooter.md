![](https://i.imgur.com/beCyIpm.png)

당신은 복잡한 화려하고 현대적인 게임에 질렸나요? 4K 해상도가 아닌 5x5 해상도로 게임을 즐기고 십나요? Space Shooter로 레트로 아케이드 게임을 즐기십시오! 이 자습서는 JavaScript로 작성되었습니다.

## Step 0： 사전 빌드 개요
---


이 프로젝트에서 우리는 발사체를 떨어 뜨리거나 피하려고 시도해야하는 간단한 우주 사수 게임을 만들 것입니다.



## 재료:    
---   

- 1 x [BBC micro:bit](http://www.icbanq.com/shop/templete_list.asp?t_idx=163)  
- 1 x Micro USB cable  
- 1 x [Breakout board](http://www.icbanq.com/shop/templete_list.asp?t_idx=163)   
- 1 x [ADKeypad](http://www.icbanq.com/shop/templete_list.asp?t_idx=163)  

![](https://i.imgur.com/JQVhM8j.jpg)  


## 목표 
---

- ADKeyboard 사용법을 배우십시오.
- 기본적인 게임 프로그래밍을 배웁니다.
- 자바 스크립트 프로그래밍에 대해 자세히 알아보십시오.

## 만드는 방법   

### 1 단계 - 구성 요소

![](https://i.imgur.com/vZCaRgF.jpg)

먼저 ADKeypad를 연결하십시오. 색상이 일치하는지 확인하고 나중에 연결될 핀을 연결하십시오.


### 2 단계 - 코딩

![](https://i.imgur.com/JhTbZcr.png)

이 프로젝트에서는 자바 스크립트를 사용하여 코드를 작성하기 때문에 먼저 페이지 상단의 자바 스크립트 모드로 전환하십시오.

![](https://i.imgur.com/o8jzFBl.png)

코드의 시작 부분에서 우리는 사용할 변수를 초기화해야합니다 : 현재 플레이어의 점수는 playerscore, 현재 화면에 외계인이 있는지 여부를 저장하는 것으로 noalien, 현재 최고 점수는 hi, 계속 유지하려면 gamestart 게임이 현재 어떤 상태인지 추적하고, 플레이어가 외계인과 충돌 한 위치를 저장하는 배열로 destroyedpos, 플레이어가 촬영 한 샷의 위치를 저장하는 샷, 외계인의 위치를 저장하는 외계인, 유지하려는 위치 플레이어의 현재 위치에 대한 트랙, 상단 행의 각 위치에 현재 외계인이있는 경우 저장할 토로 및 몇 가지 임시 변수가 있습니다. 나중에이 모든 변수를 사용해야 할 것입니다.
![](https://i.imgur.com/XAIZqT3.png)

이 함수, unrendership은 우주선을 나타내는 LED를 끕니다. 우리는 이것을 사용하여 우주선을 움직일 것입니다.

![](https://i.imgur.com/jOLiBBR.png)

이 함수, rendering은 우주선을 나타내는 LED를 켭니다. unrendership을 사용한 후에, 플레이어의 위치를 업데이트 한 다음 rendership을 사용하여 우주선의 위치를 변경할 수 있습니다.


![](https://i.imgur.com/xLcEWdH.png)

이 함수 unrendershots는 플레이어의 발사 된 샷을 나타내는 LED를 끕니다. 마찬가지로이 함수를 사용하여 샷의 위치를 ​​업데이트합니다.

![](https://i.imgur.com/3teYV2e.png)

이 함수, rendershots은  플레이어의 발사 된 샷을 나타내는 LED를 켭니다. unrendership 및 rendership과 마찬가지로 unrendershots를 사용하면 샷의 위치를 업데이트 한 다음 렌더링 된 샷을 사용하여 모든 발사 된 샷의 위치를 변경할 수 있습니다.
![](https://i.imgur.com/T38Jq0z.png)

이 함수, 체크 콜리 전 (checkcollision)은 플레이어와 외계인, 외계인과 플레이어가 발사 한 총의 두 가지 충돌 유형을 확인합니다.
첫째로, 우리 우주선의 모양 때문에 플레이어와 외계인이 충돌하기 때문에 가장 낮은 행 또는 두 번째로 낮은 행에 있고 우주선의 중심의 같은 열에 외계인이 있는지도 확인해야합니다 마치 우주선의 가장 아래 줄과 왼쪽 또는 오른쪽에 외계인이있는 것처럼. Javascript에서 &&는 "and"및 ||를 나타냅니다. "또는"를 나타냅니다. 실제로이 위치에 외계인이 있다면, 우리는이 우주인이 우주선과 충돌 한 위치로 destroyedpos를 설정하고 게임이 끝났음을 나타 내기 위해 gamestart의 값을 2로 변경합니다.
다음으로, 플레이어와 플레이어가 발사 한 샷 간의 충돌을 위해, 각각 하나의 LED 만 차지하기 때문에 위치가 정확히 동일한 지 확인해야합니다. 그러나 여러 샷이나 외계인이있을 수 있으므로 모든 외계인에 대해 모든 샷을 반복하고 동일한 위치에 있는지 확인해야합니다. LED가 켜져 있으면 LED가 꺼집니다. 외계인이 탑승 한 경우, 해당 컬럼의 상단 행에 더 이상 외계인이 없다는 것을 나타 내기 위해 해당 컬럼에 대한 toprow의 값을 0으로 설정합니다. 그런 다음 샷의 높이를 -1로 설정하고 외계인의 높이를 5로 설정하여 두 개를 화면 밖으로 옮깁니다. 나중에 두 개를 제거합니다. 마지막으로 외계인을 쏘면 플레이어의 점수가 1 씩 증가합니다.

![](https://i.imgur.com/1ljM6dE.png)

이제는 필요한 모든 함수를 작성 했으므로 마침내 함께 연결할 수 있습니다! 블록 모드로 코딩 할 때의 기능에 대해 이미 잘 알고있을 것입니다. Javascript에서는 basic.forever (() => {여기에서 영원히 실행되는 코드}를 입력하여 사용합니다. 첫 번째로 gamestart의 값이 0이면 디스플레이에 현재 최고 점수를 표시합니다.
다음으로, gamestart의 값이 2이면 플레이어의 게임이 방금 종료되었음을 의미합니다.

![](https://i.imgur.com/3AW9ob2.png)

게임을 시작하기 위해 플레이어가 ADKeypad에서 "A"버튼을 누르고 gamestart의 값이 0인지 감지합니다. 그렇다면 gamestart의 값을 1로 설정하여 게임이 시작되었음을 나타내며 플레이어의 위치를 재설정합니다 센터에서 플레이어의 점수를 0으로 설정하고 화면의 모든 LED를 끄고 이전에 만든 함수을 사용하여 ship을 rendering합니다.

![](https://i.imgur.com/KIaOgvL.png)

마지막으로, gamestart의 값이 1이면 플레이어가 현재 게임을하고 있습니다! 각 루프가 시작될 때 플레이어가 1 발씩 위로 움직여야하기 때문에 각 발사의 LED가 꺼집니다. 우리는 Math.random () 함수를 사용하여이 프레임에 외계인을 생성해야 하는지를 무작위로 결정합니다. 게임을 더 어렵게 만들려면 숫자를 낮추거나 더 쉽게 만들려면 숫자를 늘리십시오. 여기서 우리가 사용하는 값은 15입니다. 즉, 모든 루프가 생성되는 1/15의 기회가 있음을 의미합니다. 그러나, 우리가 생성하고자하는 열의 맨 위 줄이 점령되어 있지 않거나 중복되는 외계인이 있는지 확인해야합니다. 다음으로, 우리는 외계인을 순환하고 영원히 반복되는 5 번마다 한 위치 씩 아래쪽으로 움직입니다. 외계인이 맨 위 행에 들어가면 해당 열의 toprow 값을 1로 설정하고 외계 행인이 맨 위 행을 벗어나면 0으로 설정합니다.

![](https://i.imgur.com/NToYcYx.png)

그런 다음 각 외계인이 화면 외부에 있는지 확인한 다음 외계인 배열에서 제거합니다. 마지막으로, 화면에 외계인이 없다면 우리는 1/15의 확률로 산란한 같은 외계인을 낳습니다.

![](https://i.imgur.com/cSmZUSw.png)

외계인의 위치를 업데이트 한 후 충돌을 확인한 다음 샷의 위치를 업데이트합니다. 플레이어가 ADKeyboard에서 D 버튼을 누르면 샷을 생성합니다. 그 후, 우리는 스크린 밖에있는 모든 샷을 제거합니다.

![](https://i.imgur.com/kUT2zhV.png)

마지막으로 플레이어가 ADKeyboard에서 C 또는 E 키를 눌렀는지 확인하고 적절하게 배 위치를 업데이트합니다. 그 후 플레이어가 발사 한 샷을 rendering 한 다음 루프 당 0.08 초의 일시 중지를 설정하여 게임이 재생 가능한 속도로 진행되도록합니다.

전체 프로그램의 경우 아래 링크에서 직접 다운로드 할 수 있습니다.

[https://makecode.microbit.org/_euRV3uHYJAfx](https://makecode.microbit.org/_euRV3uHYJAfx)

Or download from the page below.

<div style="position:relative;height:0;padding-bottom:70%;overflow:hidden;"><iframe style="position:absolute;top:0;left:0;width:100%;height:100%;" src="https://makecode.microbit.org/#pub:88941-65098-41980-28805" frameborder="0" sandbox="allow-popups allow-forms allow-scripts allow-same-origin"></iframe></div>


### Step 3: Using It

![](https://i.imgur.com/AkL0Vuj.jpg)
게임을하는 것은 매우 간단합니다. ADKeyboard의 A 버튼을 사용하여 게임을 시작하고 C 및 E 버튼을 사용하여 이동하고 D 키를 눌러 외계인을 쏴보십시오!

### Step 4 – Success!

올레! 새로운 우주 비행사 함께 레트로 아케이드 게임을 즐길 시간. 친구와 함께 경쟁을 해보는건 어떨까요?
