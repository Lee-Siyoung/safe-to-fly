## ROS 공부 (https://www.youtube.com/playlist?list=PLRG6WP3c31_VIFtFAxSke2NG_DumVZPgw)
### 1. 로봇, 센서, 모터
ros는 다양한 종류의 로봇, 센서, 모터를 하는데 사용되고 오픈소스가 많다. 상세한 정보나 사용법은https://robots.ros.org/, http://wiki.ros.org/Sensors 로스 위키에 잘 나와 있다. 센서를 이용한 실습은 센서가 없는 관계로 영상을 보면서 배웠다. 

![image](https://user-images.githubusercontent.com/57993534/126041592-f4157dd5-aa33-4009-bdfe-0403cb4652d5.png)

모터 실습도 영상을 보면서 배웠다. 

모터 패키지 http://wiki.ros.org/Motor%20Controller%20Drivers

### 2. 임베디드 시스템
ros에서는 임베디드 시스템을 설치할 수 없지만 실시간성 확보 및 하드웨어 제어를 위해 ros가 설치된 pc와 임베디드 시스템 간의 연결을 하게 해주는 rosserial라는 패키지를 제공해준다. 

rosserial : pc와 제어기 간의 메시지 통신을 위해 중계자 역할을 수행하는 ros패키지
#### rosserial server
- rosserial_python
- rosserial_server
- rosserial_java
#### rosserial client
- rosserial_arduino
- rosserial_embeddedlinux
- rosserial_windows
- rosserial_mbed
- rosserial_tivac
#### rosserial 프로토콜
![image](https://user-images.githubusercontent.com/57993534/126041624-8184d312-f64c-481d-aa6d-2095ca3aa9f0.png)

#### rosserial 제약사항
- 메모리 : 퍼블리셔, 서브스크라이버 개수 및 송신, 수신 버퍼의 크기를 미리 정의해야 함
- Float64 : 마이크로컨트롤러는 64비트 실수연산을 지원하지 않아 32비트형으로 변경함
- Strings : 문자열 데이터를 String 메시지 안에 저장하지 않고 외부에서 정의한 문자열 데이터의 포인터 값만 메시지에 저장함
- Arrays : 메모리 제약사항으로 배열의 크기를 지정해 사용
- 통신 속도 : UART 같은 경우 115200bps와 같은 속도로는 메시지의 개수가 많아지면 응답 및 처리속도가 느려 짐

#### OpenCR (Open-source Control module for ROS)
- ros를 지원하는 임베디드 보드, 터틀봇3에서 메인 제어기로 사용됨
- 오픈소스 H/W, S/W
- rosserial의 제약 사항을 극복하기 위한 구성
예제)     
![image](https://user-images.githubusercontent.com/57993534/126041631-0a257486-85de-4ab9-9727-3b3ca18f45b5.png)     
![image](https://user-images.githubusercontent.com/57993534/126041632-ffec4a39-4131-4c4f-b805-b4a17b331eb4.png)


### 3. 모바일 로봇
Turtle : 터틀봇은 컴퓨터 프로그래밍 언어를 쉽게 가르치자는 취지로 고안되었고 ros의 표준 플랫폼으로 자리 잡았다.     
TurtleBot : ros 공식 로봇 플랫폼    
![image](https://user-images.githubusercontent.com/57993534/126041637-7a7482fd-94d2-4cf9-9585-58a5a91431cd.png)

#### TurtleBot3 개발환경(네트워크)
- TurtleBot
  - ROS_MASTER_URI = http://IP_OF_REMOTE_PC:11311
  - ROS_HOSTNAME = IP_OF_TURTLEBOT
- Remote PC
  - ROS_MASTER_URI = http://IP_OF_REMOTE_PC:11311
  - ROS_HOSTNAME = IP_OF_REMOTE_PC
 
![image](https://user-images.githubusercontent.com/57993534/126041650-def6884d-987a-4989-804f-4a1e73474f61.png)

![image](https://user-images.githubusercontent.com/57993534/126041652-bfdb120c-5ecf-4e70-b580-c0c54d372458.png)

![image](https://user-images.githubusercontent.com/57993534/126041655-496522b8-dd61-420c-b589-00f0e300251e.png)

### 4. SLAM과 내비게이션
#### SLAM : Simultanceout Localization And Mapping, 동시적 위치 추정 및 지도 작성

#### 길 찾기에 필요한 것
1. 위치 : 로봇의 위치 계측/추정히는 기능        
  - GPS
  - 추측 항법 : 양 바퀴 축의 회전 값을 이용
  
2. 센싱 : 벽, 물체 등의 장애물의 계측하는 기능     
  - 거리센서
  - 비전센서
  - Depth camera
  
3. 지도 : 길과 장애물 정보가 담긴 지도    

4. 경로 : 목적지까지 최적 경로를 계산하고 주행하는 기능       
  - 내비게이션
  - 위치 추정
  - 경로 탐색/계획

#### Gmapping : OpenSLAM에 공개된 SLAM의 한 종류, ros에서 패키지로 제공
- 특징 : Rao-Blackwellized 파티클 필터, 파티클 수 감소, 그리드 맵

#### 지도 작성 : Gmapping + TurtleBot3
![image](https://user-images.githubusercontent.com/57993534/126041697-79d3d29e-5f3a-41e0-8260-9ef7c0e323c0.png)

#### SLAM 관련 노드 처리 과정
1. sensor_node    
2. turtlebot3_teleop     
3. turtlebot3_core     
4. slam_gmapping        
5. map_server     

![image](https://user-images.githubusercontent.com/57993534/126041709-95725f39-00a4-41b3-98fc-8f54d4fc5633.png)

#### 위치 추정(localization)
- 칼만 필터 : 잡음이 포함되어 있는 선형 시스템에서 대상체의 상태를 추적하는 재귀 필터
- 파티클 필터 : 시행 착오법을 기반으로한 시뮬레이션을 통해 예측하는 기술로 대상 시스템에 확률 분포로 임의로 생성된 추정값을 파티클(입자) 형태로 나타낸다.       
1. 초기화    
2. 예측     
3. 보정    
4. 위치 추정       
5. 재추출     

#### 내비게이션 
- Dynamic Window Approach : 로봇의 속도 탐색 영역에서 로봇과 충돌 가능한 장애물을 회피하면서 목표점까지 빠르게 다다를 수 있는 속도를 선택하는 방법

#### TurtleBot3 시뮬레이션
![image](https://user-images.githubusercontent.com/57993534/126041718-638afc43-4ad7-459b-972e-0bf85f680df6.png)


#### 가상 SLAM with Gazebo
![image](https://user-images.githubusercontent.com/57993534/126041722-53a86fae-5e1d-4203-b4e5-0c682e93e730.png)

#### 가상 내비게이션 with Gazebo
![image](https://user-images.githubusercontent.com/57993534/126041726-5db031fd-8487-4ee6-85bc-5db6d1a1e4a3.png)


