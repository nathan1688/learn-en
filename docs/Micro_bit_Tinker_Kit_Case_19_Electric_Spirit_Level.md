![](https://i.imgur.com/x8QD9mQ.jpg)

이 spirit level을 사용하여 연결된 모든 물체의 기울기를 빠르고 쉽게 표시 할 수 있습니다!
래플스 연구소 (Raffles Institution)의 Kaitlyn에 의해 작성되었습니다.

## 목표
---

- 마이크로 비트의 내장 가속도계로 기울기를 읽는 법을 배우십시오.
- 마이크로 : 비트의 5x5 LED 디스플레이로 작업하는 방법을 배우십시오!

![](https://i.imgur.com/b0zxOOu.png)


## 재료
---

- 1 x BBC micro:bit
- 1 x Micro USB cable
- 2 x AA Batteries
- 1 x Double AA Battery Pack


## 사전 코딩 : 마이크로 : 비트 연결
---

- 마이크로 USB 케이블을 사용하여 BBC 마이크로 : 비트를 컴퓨터에 연결하십시오.
- makecode.microbit.org에서 micro : bit 용 javascript 편집기에 액세스하십시오.


### 단계 0 : 코드 흐름

코드 작성을 시작하기 전에 프로그램을 통해 달성하려는 내용과 각 구성 요소가 실행될 순서를 결정해야합니다.

electric spirit level에서 각 루프의 코드에서 수행 할 단계는 다음과 같습니다.

가속도계에서 기울기 판독 값을 읽습니다.
기울기 판독 값을 LED 매트릭스에 표시 할 기울기 레벨로 변환하십시오.
이전 루프에서 기울기 레벨 판독 값의 변화를 확인하십시오.
다양한 틸트 케이스 및 방향에 대한 LED 좌표 배열을 만듭니다.
LED를 마이크로에 배치 : 비트 LED 매트릭스.

우리가 포함시켜야 할 몇 가지 추가 기능은 다음과 같습니다.

초기 경사 위치 교정.
기본 기울기 보정으로 돌아갑니다.

![](https://i.imgur.com/cPWw6yl.png)


## 만드는 방법
---

### 1 단계 : 변수 정의하기

우리는 그림과 같이 필요한 변수를 정의하는 것으로 시작합니다. 몇 가지 변수의 분석은 다음과 같습니다.

tiltList : [왼쪽, 오른쪽, 앞으로, 뒤로] 순서로 값 0-4에서 기울기의 범위를 저장하는 배열입니다.
tiltBoundary : 0 (틸트 없음)과 1 (약간 틸트) 사이의 첫 번째 틸트 레벨의 경계입니다.
prevState : 반복 사이의 기울기 변화를 확인하는 데 사용되는 tiltList와 동일한 형식으로 이전 루프의 micro : 비트의 기울기 값을 저장하는 배열
ledPlotList : (x, y) 형태로 좌표 배열을 표시합니다. 배열을 정의하기 위해 number [] [] 형식을 사용하여 number 유형의 변수 중첩 배열을 나타냅니다.

![](https://i.imgur.com/dxySnVH.png)

### 2 단계 : 기울기 값을 레벨로 변환

5x5 LED 매트릭스는 많은 정보만을 표시 할 수 있으므로 실제 기울기 값은 표시에 유용하지 않습니다.

대신 tiltExtent () 함수는 가속도계의 기울기 값을 나타내는 num 매개 변수를 사용하고 이러한 기울기 값 (num)을 0에서 4까지의 기울기 수준으로 변환합니다.

0은 지정된 방향으로 기울기가 없음을 나타내고 4는 매우 큰 기울기를 나타내는 반면 오류가 있으면 -1이 반환됩니다.

여기서, tiltBoundary 및 tiltSensitivity는 틸트 레벨 간의 경계 값으로 사용됩니다.


![](https://i.imgur.com/qlNuqXy.png)

### 3 단계 : 틸트 레벨 컴파일

checkRoll () 및 checkPitch ()는 tiltExtent ()에서 얻은 기울기 수준을 롤 (왼쪽 - 오른쪽) 및 피치 (앞뒤 방향) 축의 tiltList에 각각 기록합니다.

기울기 값을 사용하기 전에 나중에 기록 된 교정 기능에서 얻은 피치 (제로 피치)와 롤 (제로 롤) 모두에 대해 제로 값을 사용하여 교정합니다.

가속도계 판독 값이 왼쪽 및 전방 기울기에 대해 음수이므로 Math.abs () 함수를 사용하여이 두 방향의 매개 변수로 tiltExtent () 함수에 제공 할 음수 값의 모듈러스를 가져와야합니다.
![](https://i.imgur.com/DdzPN5C.png)

### 4 단계 : LEDPlotList 함수 작성

tiltList에서 기울기 레벨을 얻음으로써 우리는 이제 발생할 수있는 여러 가지 경우에 대해 주도되는 플로팅 함수를 작성할 수 있습니다.

plotSingle () : 주어진 방향으로 기울기를 매개 변수로 사용하여 한 방향으로 만 기울입니다.
plotDiagonal () : 매개 변수로 어느 방향 으로든 기울이는 정도를 가져서 같은 크기의 두 방향으로 기울입니다.
plotUnequal () : 각 방향의 기울기를 매개 변수로 사용하여 크기가 다른 두 방향으로 기울입니다. plotDiagonal ()을 먼저 사용하고 나중에 ledPlotList 배열에 추가합니다.

이러한 플로팅 함수는 ledPlotList에 대한 좌표를 나중에 플롯 할 배열을 씁니다.

![](https://i.imgur.com/v49F5bk.png)
![](https://i.imgur.com/3ueJy2X.png)
![](https://i.imgur.com/hRWdNtS.png)

### 5 단계 : 각 경우에 대한 플롯 LED 매트릭스

4 단계의 세 가지 경우에서 플로팅 기능을 사용하여 가능한 다양한 조합의 기울기에 대해 실제 LED 매트릭스를 플로팅 할 수 있습니다. 4 단계의 세 가지 기능은 방향과 구별되지 않으므로 LED 매트릭스에 전달 된 좌표 값을 조정하여 LED를 올바른 방향으로 플롯해야합니다.

![](https://i.imgur.com/3pCbbkX.png)
![](https://i.imgur.com/KkGJgEP.png)

PlotResult ()에는 led.plot (x, y)를 사용하여 기울기의 종류를 확인하고 이에 따라 LED 행렬을 그려주는 여러 개의 if 조건이 있습니다. 기울기의 가능한 조합은 다음과 같습니다.

한 방향 : 왼쪽 전용 또는 오른쪽 전용.

![](https://i.imgur.com/gGINyUC.png)

단일 방향 : 전진 전용 또는 전진 전용.

![](https://i.imgur.com/8ifs068.png)

두 방향 : 앞으로 왼쪽 또는 뒤로 - 왼쪽.

![](https://i.imgur.com/SyChxHQ.png)

두 방향 : 앞으로 - 오른쪽 또는 뒤로 - 오른쪽.

![](https://i.imgur.com/Ncgeroq.png)

참고 : 두 방향의 기울이기의 경우 각 조합의 크기는 같거나 다를 수 있습니다 (maxX와 maxY를 비교하여 확인). 따라서 plotDiagonal () 또는 plotUnequal ()을 각각 사용하여 플롯됩니다.

### 6 단계 : 교정 기능 쓰기

대량의 코드를 완료하면 calibTilt () 및 resetTilt () 함수를 추가합니다.

calibTilt ()를 사용하면 마이크로 비트의 현재 위치에서 틸트를 0으로 설정할 수 있습니다.
resetTilt ()는 보드의 보정을 원래 상태로 재설정합니다.

![](https://i.imgur.com/1CygCKv.png)

### 7 단계 : 상태 함수 쓰기

간단한 함수 checkState ()를 추가하여 틸트 수준이 이전 반복에서 변경되었는지 확인합니다.

이전 반복 (즉, stateChange == 0)으로부터의 기울기 레벨의 변화가 없다면, 다음 반복으로 직접 이동하여 LED 매트릭스의 플롯을 건너 뛰고, 필요한 계산을 감소시킬 수 있습니다.

![](https://i.imgur.com/znXLpT1.png)

### 8 단계 : 모두 함께 파트 1!

이제는 반복적으로 실행하기 위해 마침내 필요한 모든 기능을 마이크로 비트의 무한 루프에 배치 할 수 있습니다.

먼저 input.onButtonPressed ()를 사용하여 micro : bit의 버튼 A와 B를 calibTilt () 및 resetTilt () 함수에 각각 설정하고 보정이 완료되면 LED 매트릭스에 눈금을 그립니다.

![](https://i.imgur.com/SJQ48XN.png)

### 9 단계 : 함께 두 번째 파트 2!

그런 다음 단계 0에서 코드 흐름에 따라 필요한 함수를 실행하고 상태 변경을 확인합니다 (마지막 반복 이후 micro : bit의 기울기가 변경되었음을 의미).

기울기 레벨, 즉 stateChange == 1에 변화가 있으면 코드는 prevState를 새로운 기울기 레벨로 업데이트하고 다음 반복을 위해 stateChange를 다시 0으로 설정하고 PlotResult ()를 사용하여 LED 매트릭스에 업데이트 된 기울기 레벨을 그립니다.

![](https://i.imgur.com/sYU2wKx.png)  

이 코드를 직접 입력하지 않으려면 아래 링크에서 직접 다운로드 할 수 있습니다.

[https://makecode.microbit.org/56811-31458-64502-76623](https://makecode.microbit.org/56811-31458-64502-76623)  

Or you can download from the page below.  

<div style="position:relative;height:0;padding-bottom:70%;overflow:hidden;"><iframe style="position:absolute;top:0;left:0;width:100%;height:100%;" src="https://makecode.microbit.org/#pub:56811-31458-64502-76623" frameborder="0" sandbox="allow-popups allow-forms allow-scripts allow-same-origin"></iframe></div>  

### 10 단계 : 어셈블리

완성 된 코드를 마이크로 비트로 플래시하십시오.
마이크로 비트와 배터리 팩을 어떤 물체에도 안전하게 부착하면 사용할 준비가되었습니다!

![](https://i.imgur.com/pWejt7w.png)  


### 신난다!

electric spirit level로 재미있게! 그 동안 틸트 센서의 기능을 확장하거나 게임으로 바꾸는 것이 어떻습니까?
