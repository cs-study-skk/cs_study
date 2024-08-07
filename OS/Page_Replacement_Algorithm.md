# 페이지 교체 알고리즘 (Page Replacement Algorithm)

## 📌 페이지 교체 알고리즘이란?
- 운영 체제가 메모리를 관리하는 과정에서 페이지 폴트를 처리할 때 **어떤 페이지를 메모리에서 제거하고 새로운 페이지를 메모리에 로드할지 결정하는 방법**을 의미한다.
- 이때 **페이지 폴트(Page Fault)** 란 가상 메모리 주소를 통해 실제 물리 주소를 얻고 메모리에 접근했지만 **내가 찾는 페이지가 실제 메모리에 현재 적재되어 있지 않을 때**이다.
- 페이지 교체 알고리즘은 다양하며 **페이지 부재를 줄이고 시스템의 성능을 향상 시키는 것**이 핵심이다.

<br>

## 📌 무작위 페이지 교체 알고리즘 (Random)
- 가장 간단하게 구현할 수 있는 알고리즘이다.
- 스왑 아웃(swap out)될 대상 페이지를 특별한 로직 없이 무작위로 선정한다.
- 성능은 좋지 않다.

> **스왑 아웃 (Swap Out)**
> 운영체제가 메모리 관리를 위해 사용하는 기법 중 하나로 주로 시스템의 메모리가 부족할 때 발생한다.
> 이럴 경우 프로세스의 메모리 일부를 하드 디스크와 같은 보조 저장 장치로 이동시켜 물리적 메모리(RAM)를 확보하는데 이를 스왑 아웃(Swap Out)이라고 한다.

<br>

## 📌 FIFO(First In First Out) 알고리즘
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/c5c4cd16-a320-4357-bf06-81358710058f)
- 가장 먼저 메모리에 올라온 페이지를 대상 페이지로 선정해 가장 먼저 스왓 아웃 시키는 알고리즘이다.
- 큐로 구현되어 있어 새로운 페이지는 항상 맨아래 삽입되는데 이때 메모리가 꽉 차면 맨 위의 페이지가 스왑 아웃 되고, 나머지 페이지들이 위쪽으로 이동하며 새로운 페이지가 다시 맨밑으로 들어온다.
- 구현이 간단하다.
- 페이지 교체 알고리즘에서는 앞으로 사용하지 않을 페이지의 스왑 아웃이 중요한데 FIFO는 그렇지 않아 성능이 떨어진다.
- Belady`s Anomaly 현상이 발생할 수 있다.

<br>

> **Belady`s Anomaly**
> <br>
> ![image](https://github.com/cs-study-skk/cs_study/assets/39427152/06ab18d7-7a50-4079-b1d0-2eb98151d740)
> 프레임의 개수가 많아져도 페이지 폴트(Page Fault)가 줄어들지 않고 오히려 늘어나는 비정상적인 현상
> <br>
> FIFO는 페이지를 교체할 때 가장 먼저 메모리에 들어온 페이지를 먼저 내보내는데 오래된 페이지가 자주 사용되는 경우 필요한 페이지가 자주 교체되어 페이지 폴드가 증가할 수 있다.
> <br>
> 특정한 페이지 참조 패턴에서는 페이지 프레임이 많아질수록 메모리에 불필요한 페이지가 더 오래 남아 있게 되며 자주 사용되는 페이지가 교체될 가능성이 커진다.
> <br>
> 이 문제를 막기 위해서 LRU (Least Recently Used), OPT (Optimal Page Replacement)와 같은 알고리즘을 사용한다.

<br>

## 📌 최적 페이지 교체 알고리즘(Optimal)
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/95e04147-0479-41d1-b62e-4716ba642841)
- 앞으로 사용할 페이지를 미리 살펴보고 페이지 교체 선정 시점부터 사용 시점까지 가장 멀리 있는 페이지를 선정한다.
- 즉, 앞으로 사용하지 않을 가능성이 제일 높은 페이지를 대상 페이지로 선정하여 스왑 아웃 시킨다.
- 이론적으로는 가장 효율적인 알고리즘이지만 앞으로의 페이지 참조를 예측해야 하므로 실현이 어려운 알고리즘이다.

<br>

## 📌 LRU 페이지 교체 알고리즘(Least Recently Used)
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/fab486e9-b822-4092-ab55-d4f47c3f8e34)
- 메모리에 올라온 후 가장 오랜 시간 동안 사용되지 않는 페이지를 스왑 아웃 시키는 알고리즘이다.
- 가장 오래 사용되지 않은 페이지를 판단하는 방법으로는 여러 가지가 있다.
  - 페이지 접근 시간에 기반한 구현: 페이지에 접근한 시각을 기록해 판단.
  - 카운터에 기반한 구현: 메모리를 추가적으로 사용해 페이지에 접근한 시각이 아닌 몇 번째로 접근했는지를 기록해 판단. (페이지 접근 시간이나 카운터 둘 다 메모리 공간이 추가로 더 들어감)
  - 참조 비트 시프트 방식: 참조 비트의 초깃값은 0이고 페이지에 접근할 때마다 1로 바뀜. 참조 비트는 주기적으로 한 칸씩 오른쪽 시프트(>>)가 되는데 이렇게 참조 비트를 갱신하다 대상 페이지 선정할 필요가 있으면 참조 비트 중 가장 작은 값을 대상으로 페이지를 선정.
- 최근 자주 사용되는 페이지는 계속 메모리에 남아 있게 되기 때문에 성능이 비교적 좋지만 구현이 복잡하고 시간과 공간 오버 헤드가 크다.

<br>

## 📌 LFU 페이지 교체 알고리즘(Least Frequently Used)
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/57349014-335c-40a9-bb0b-6dab3e2410d2)
- 페이지가 몇 번 사용되었는지를 기준으로 대상 페이지를 선정한다.
- 현재 프레임에 있는 페이지마다 그동안 사용된 횟수를 세어 그 횟수가 가장 적은 페이지를 스왑 아웃 시킨다.
- LRU 알고리즘과 성능이 비슷하고, 페이지 접근 횟수를 표시하는데 메모리의 추가 공간이 필요되므로 낭비되는 메모리 공간이 많다.
- 자주 사용되는 페이지가 오래 유지될 수 있지만 최근에 급격히 사용 빈도가 늘어난 페이지가 교체될 수도 있다.

<br>

## 📌 NUR 페이지 교체 알고리즘(Not Used Recently)
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/5c38e193-587e-4217-80a6-6b7fea3649fa)
- LRU, LFU 페이지 교체 알고리즘과 거의 비슷하지만 불필요한 메모리 공간 낭비 문제를 해결할 수 있는 알고리즘이다.
- 접근 비트는 페이지에 접근, 즉 읽기나 실행 연산 되면 1이 되고, 변경 비트는 페이지가 변경, 즉 쓰기나 추가 연산이 되면 1이 된다.
- 우선 고려 대상은 접근(참조) 비트이다. 접근 비트가 0인 페이지를 먼저 찾고, 없으면 변경 비트가 0인 페이지를 찾는다.
  - R=0, M=0: 최근에 사용되지 않았고 수정되지 않은 페이지.
  - R=0, M=1: 최근에 사용되지 않았지만 수정된 페이지.
  - R=1, M=0: 최근에 사용되었지만 수정되지 않은 페이지.
  - R=1, M=1: 최근에 사용되었고 수정된 페이지.
- 페이지 교체 시, NUR 알고리즘은 먼저 R=0, M=0인 페이지를 찾고, 없으면 R=0, M=1인 페이지를 찾는 식으로 진행합니다. 이 과정은 시스템 성능을 유지하면서도 페이지 교체를 효율적으로 수행할 수 있게 해줍니다.
- 만약 모든 비트가 (1, 1)이라면 모든 페이지 비트를 (0, 0)으로 초기화한다.
- 구현이 간단하고 자주 사용되지 않는 페이지를 우선적으로 교체할 수 있다.
- 페이지의 수정 여부를 고려함으로써, 불필요한 디스크 쓰기를 줄일 수 있습니다.
- 주기적으로 참조 비트를 리셋하는 추가 작업이 필요합니다.
- 페이지 교체 시, 모든 페이지의 비트를 검사해야 하므로 다소의 오버헤드가 발생할 수 있습니다.
- 각 페이지의 참조를 유지 및 업데이트하고 비트를 수정하기 위해 추가적인 하드웨어 지원이나 소프트웨어 오버헤드가 필요하기 때문에 최근에는 잘 사용하지 않는 알고리즘이다. LRU가 더 많이 쓰인다.

<br>

## 📌 시계 알고리즘
![image](https://github.com/cs-study-skk/cs_study/assets/39427152/9ab25e6e-1120-488e-a985-9a006360eaf8)
- 원형 큐를 사용해 대상 페이지를 선정하는 알고리즘이다.
-  참조 비트를 사용하여 페이지 교체 시 효율성을 높이는 방식이다.
- 참조 비트 설정: 각 페이지 프레임에는 참조 비트가 있습니다. 페이지가 참조될 때마다 해당 페이지의 참조 비트는 1로 설정됩니다.
- 페이지 순회: 페이지 교체가 필요할 때, "시계 바늘" 포인터가 원형 리스트를 순회하면서 참조 비트가 0인 페이지를 찾습니다.
- 참조 비트 확인:
  - 참조 비트가 1인 경우: 참조 비트를 0으로 리셋하고 다음 페이지로 이동합니다.
  - 참조 비트가 0인 경우: 해당 페이지를 교체합니다.
- 순환: 시계 바늘은 계속 순환하며 참조 비트가 0인 페이지를 찾습니다. 참조 비트가 0인 페이지를 찾을 때까지 이 과정을 반복합니다.
- FIFO(First-In-First-Out) 알고리즘의 단점을 보완하며, 최근에 사용되지 않은 페이지를 우선적으로 교체함으로써 시스템의 성능을 향상시킨다.
  
출처: https://rob-coding.tistory.com/37 [💻평범한 공대생의 개발 노트💻:티스토리]
