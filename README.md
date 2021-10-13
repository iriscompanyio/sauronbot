 # SAURONBOT
_Robot diferencial_
## Modelos
SE cuenta con los siguiente modelos de desarrollo:
* sauronbot_model_01 [model_01]     --  llantas, lidar   
* sauronbot_model_02 [model_02]     --  llantas, lidar, RpiCamera
* sauronbot_model_03 [rover_habich] --  llantas, lidar, RpiCamera, Realsense
## Comenzando 🚀
_Estas instrucciones te permitirán obtener una copia del proyecto en funcionamiento en tu máquina local para propósitos de desarrollo y pruebas._
Mira **Deployment** para conocer como desplegar el proyecto.
### Pre-requisitos 📋
_Que cosas necesitas para instalar el software y como instalarlas_
* [Ubuntu 18.04](https://releases.ubuntu.com/18.04/)
  * [Tutorial de instalación](https://www.muylinux.com/2018/06/18/guia-instalacion-ubuntu-18-04-lts/)
* [ROS-melodic](http://wiki.ros.org/melodic/Installation/Ubuntu)

### Instalación 🔧

_Una serie de ejemplos paso a paso que te dice lo que debes ejecutar para tener un entorno de desarrollo ejecutandose_
_Dí cómo será ese paso_

* _1.- Descarga de archivos_ 
```
cd $HOME
mkdir -p sauronbot_ws/src
cd sauronbot_ws/src
git clone https://github.com/jsotelo1024/sauronbot_msgs.git
git clone https://github.com/jsotelo1024/sauronbot.git
```
* _2.- Instalacion de dependencia: mqtt_bridege_
```
cd $HOME
cd sauronbot_ws/src
git clone -b python2.7 https://github.com/groove-x/mqtt_bridege.git
sudo apt install python-pip
cd mqtt_bridege
pip install -r requirements.txt
sudo apt-get ros-melodic-rosbridge-server
```

* _3.- Instalacion de dependencia: mqtt_bridege_
```
cd $HOME
mkdir -p sauronbot_ws/src
cd sauronbot_ws/src
git clone -b master https://github.com/Slamtec/rplidar_ros.git
```

---
## NOTA: SOLO MOVIMIENTO DEL ROBOT DIFERENCIAL
Para ahorrar recursos en la SBC, se puede usar solo el paquete sauronbot_bringup, que se optiene de dos maneras.

### **Opcion 1 (Recomendada):**
Clonar la rama [melodic-SBC](https://github.com/jsotelo1024/sauronbot/tree/melodic-SBC)

### **Opcion 2 :**
* 1.- Clonar el repositorio
```
git clone -b melodic-devel git@github.com:jsotelo1024/sauronbot.git
```
* 2.- Borrar directorios innecesarios
```
rm -rf sauronbot_description sauronbot_gazebo sauronbot_mqtt sauronbot_slam CHANGELOG.md README.md
```
* 3.- Modificar sauronbot/package.xml
~~~
    <buildtool_depend>catkin</buildtool_depend>
    <exec_depend>sauronbot_bringup</exec_depend>
--  <exec_depend>sauronbot_slam</exec_depend>
--  <exec_depend>sauronbot_description</exec_depend>
--  <exec_depend>sauronbot_teleop</exec_depend>
--  <exec_depend>sauronbot_gazebo</exec_depend>
--  <exec_depend>sauronbot_mqtt</exec_depend>
~~~ 
* 4.- Modificar sauronbot_bringup/package.xml
~~~
    <exec_depend>std_msgs</exec_depend>

--  <exec_depend>sauronbot_description</exec_depend>
    <exec_depend>joint_state_publisher</exec_depend>
    <exec_depend>robot_state_publisher</exec_depend>
    <exec_depend>rosserial_python</exec_depend>
    <exec_depend>rplidar_ros</exec_depend>
~~~