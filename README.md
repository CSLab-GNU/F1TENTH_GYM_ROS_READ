# F1TENTH Gym ROS
F1TENTH Gym 환경을 차량에 이식하기 용이하기 하기 위하여 ROS 환경으로 구현한 것입니다.

## Quickstart
### System Requirements
* Ubuntu 18.04 LTS or Ubuntu 20.04 LTS
* ROS Melodic or ROS Noetic
* Docker 20.10.11 or later
* Python 2.7
* Python 3.6 or later

### Test Environment
* Ubuntu 18.04
* ROS Melodic
* Python 2.7

**ROS Melodic (or later)와 Docker가 설치 되어 있어야 합니다. 설치 방법은 [Docker 설치방법](https://docs.docker.com/engine/install/ubuntu/), [ROS 설치방법](http://wiki.ros.org/melodic/Installation/Ubuntu)
) 을 따라 설치하여 주시길 바랍니다.**

### Installation
1. catkin_ws/src 폴더에 해당 Repository 를 Clone 합니다
```bash
$ mkdir ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ git clone https://github.com/CSLab-GNU/f1tenth_gym_ros
```
2. docker image를 빌드합니다.
```bash
$ cd f1tenth_gym_ros
$ sudo ./build_docker.sh
```

3. (Optional) catkin_ws/src/Sample-Code 폴더에 예제 코드를 Clone 합니다.
```bash
$ cd ~/catkin_ws/src
$ git clone https://github.com/CSLab-GNU/Sample-Code
$ cd ..
$ catkin_make
$ source devel/setup.bash
```

### Run

1. 환경을 실행시키기 위해, 다음 Shell Script를 실행합니다.
```bash
$ sudo ./docker.sh
```

2. Host 시스템의 다른 Terminal 창에서 주행 관련 launch 파일을 실행합니다.
```bash
$ roslaunch <패키지명> <주행알고리즘>.launch
```


#### Change Map
환경에 설정된 맵을 변경하는 방법은 다음과 같습니다.  
1. params.yaml 파일을 수정합니다.
```yaml
...
map_path: '/f1tenth/maps/<맵파일>.yaml'
...
```
2. gym_bridge.launch 파일을 수정합니다.
```xml
...
<arg name="map" default="$(find f1tenth_gym_ros)/maps/<맵파일>.yaml"/>
...
```
3. 이후 docker image를 다시 build 합니다.
```bash
$ sudo ./build_docker.sh
```

맵 관련 파일은 <지도 이름>.png 와 <지도 이름>.yaml 파일 두가지가 존재하여야합니다.  
이전 대회에서 사용되었던 맵은 [f1tenth/f1tenth_racetracks](https://github.com/f1tenth/f1tenth_racetracks) 에서 확인할 수 있습니다.

#### (Optional) Launch file을 만드는 방법.

차량과 F1tent Gym ROS 에서 코드를 실행하기 위한 Launch 파일은 다음과 같은 형식을 가집니다.

```yaml
<launch>
    <include file="$(find f1tenth_gym_ros)/launch/gym_bridge_host.launch" /> 
    // simulation 상에서 작동시키기 위해서 꼭 들어가야 하는 부분입니다. gym bridge를 같이 실행시켜줍니다.
    <rosparam command="load" file="$(find <패키지이름>)/sim_params.yaml" /> 
    // 외부 파라미터를 불러오기 위한 부분입니다. 
    <node name="<노드이름>" pkg="<패키지 이름>" type="<주행코드 이름>.py" output="screen"> 
    // 주행 코드를 실행하기 위해서 불러오는 부분입니다. 

    </node>
</launch>
```
#### (Optional) ROS 패키지를 추가하는 방법.
Git에서 받은 코드를 추가하기 위해서는 다음 방법을 따릅니다.  
1. catkin_ws/src 폴더에 해당 내용을 클론 혹은 추가합니다.
```bash
cd ~/catkin_ws/src
git clone <코드>
```
2. catkin_ws 폴더로 와서 catkin_make를 해줍니다.
```bash
cd ~/catkin_ws
catkin_make
```
