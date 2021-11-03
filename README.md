 # SAURONBOT
_Robot diferencial_
## Modelos
SE cuenta con los siguiente modelos de desarrollo:
* sauronbot_model_01 **[model_01]**     --  llantas, lidar   
* sauronbot_model_02 **[model_02]**     --  llantas, lidar, RpiCamera
* sauronbot_model_03 **[rover_habich]** --  llantas, lidar, RpiCamera, Realsense
* sauronbot_model_04 **[model_04]** --  llantas, lidar, RpiCamera
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

## Funcionamiento 🚀
### Conexion con PC-SBC 🔧
* _1.- Scanear la red local en busca del SBC_

Para lo cual se debe tener instalado nmap:

```
sudo apt-get install nmap
```
Se procede con la busqueda de la IP del SBC:
```
nmap -sP <IP_PC>/<#NETMASK>
```

* _2.- Conectarse mediante ssh a la SBC_

```
ssh {USER}@{IP_ADDRESS_OF_SBC}
```

### Corriendo ROS en PC-SBC 🔧

* _1.- Configurar $HOME/.bashrc de la PC_

```
++  export ROS_IP={IP_ADDRESS_OF_PC}
++  export ROS_HOSTNAME={IP_ADDRESS_OF_PC}
++  export ROS_MASTER_URI={IP_ADDRESS_OF_PC}:11311
++  echo "ROS_IP: ${ROS_IP}"
++  echo "ROS_HOSTNAME: ${ROS_HOSTNAME}"
++  echo "ROS_MASTER_URI: ${ROS_MASTER_URI}"
```

* _2.- Configurar $HOME/.bashrc de la SBC_

```
++  export ROS_IP={IP_ADDRESS_OF_SBC}
++  export ROS_HOSTNAME={IP_ADDRESS_OF_SBC}
++  export ROS_MASTER_URI={IP_ADDRESS_OF_PC}:11311
++  echo "ROS_IP: ${ROS_IP}"
++  echo "ROS_HOSTNAME: ${ROS_HOSTNAME}"
++  echo "ROS_MASTER_URI: ${ROS_MASTER_URI}"
```

* _3.- Sincronizar relojes_


Tener instalado ntpdate:
```
sudo apt-get install ntpdate
```
Ejecutar en la PC

```
sudo ntpdate ntp.ubuntu.com
```
Ejecutar en la SBC

```
sudo ntpdate ntp.ubuntu.com
```
## ACTIVACION DE LA CÁMARA 🚀
* _1.- Lanzar el nodo en la SBC mediante:_
```
roslaunch raspicam_node camerav2_410x308_30fps.launch
```

* _2.- Lanzar el GUI de configuracion en la PC_
```
rosrun rqt_reconfigure rqt_reconfigure
```

* _3.- Vizualizar la imagen en la PC mediante:_
```
rqt_image_view
```

## MAPEO 🚀
* _1.- Ejecutar en la SBC_

```
roslaunch sauronbot_bringup sauronbot_robot.launch
```

En la PC
* _2.- Ejecutar el slam con el método hector_

```
roslaunch sauronbot_slam sauronbot_slam.launch slam_methods:=hector
```

* _3.- Desplzarse por el ambiente con teleop_

```
roslaunch sauronbot_teleop sauronbot_teleop_key.launch
```

* _4.- Guardar el mapa generado_

```
rosrun map_server map_saver -f {RUTA}/{NAME_MAP}
```

## NAVEGACIÓN 🚀
* _1.- Lanzar el nodo del robot en el SBC_

```
roslaunch sauronbot_bringup sauronbot_robot.launch
```
* _2.- Lanzar el nodo de navegación en la PC_

```
roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=<Ruta_del_mapa>/<nombre_del_mapa>.yaml
```

## MONITOREO DE ROS 🚀
Ros te brinda herramientas para analizar el flujo de datos.
* _1.- Vizulizar el arbol de tf_

```
rosrun rqt_tf_tree rqt_tf_tree
```
* _2.- Monitorear los ttpicos_

```
rqt
```
Seleccionar _Plugins/Topics/Topic Monitor_

<img src="https://drive.google.com/uc?export=view&id=1V1NW5-CdyPlD71YSbCmO87Xw6ycYgDZt" width="600">

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