# microros

Workspace ROS 2 untuk menjalankan **micro-ROS Agent**, yaitu jembatan antara
mikrokontroler (node micro-ROS) dan jaringan ROS 2 / DDS.

Isi repositori ini adalah salinan paket
[micro-ROS-Agent](https://github.com/micro-ROS/micro-ROS-Agent) (v3.0.6) di dalam
struktur workspace `colcon`.

## Struktur

```
microros/
├── src/
│   └── micro-ROS-Agent/
│       └── micro_ros_agent/   # paket ROS 2 (CMakeLists.txt, package.xml, launch/, src/)
├── LICENSE
└── README.md
```

Folder `build/`, `install/`, dan `log/` dihasilkan oleh `colcon` dan tidak ikut
di-commit.

## Prasyarat

- Linux (diuji di Ubuntu)
- ROS 2 yang sudah terpasang dan di-*source*
- `colcon`, `rosdep`, `git`, `cmake`

## Build

```bash
git clone https://github.com/ARF-XLVI/microros.git microros_ws
cd microros_ws

source /opt/ros/$ROS_DISTRO/setup.bash
rosdep update
rosdep install --from-paths src --ignore-src -y

colcon build
source install/local_setup.bash
```

## Menjalankan

Agent mendukung transport UDP/TCP (IPv4 dan IPv6) serta serial.

```bash
# Serial
ros2 run micro_ros_agent micro_ros_agent serial --dev /dev/ttyUSB0 -b 115200

# UDP
ros2 run micro_ros_agent micro_ros_agent udp4 --port 8888
```

Atau lewat launch file:

```bash
ros2 launch micro_ros_agent micro_ros_agent_launch.py
```

Setelah klien micro-ROS di mikrokontroler terhubung, topiknya terlihat dengan:

```bash
ros2 topic list
```

## Catatan

- Port serial yang dipakai bersama Docker kadang mengalami *packet loss*.
- Versi snap punya masalah pada komunikasi serial; disarankan build dari source
  (seperti di atas) atau memakai versi Docker.

## Lisensi

Berkas `LICENSE` di root repositori ini berlisensi MIT. Kode micro-ROS-Agent di
`src/micro-ROS-Agent/` tetap memakai lisensi aslinya (Apache 2.0), lihat
`src/micro-ROS-Agent/LICENSE` dan `NOTICE`.

## Kredit

Kode agent berasal dari proyek [micro-ROS](https://micro.ros.org/) dan
[eProsima Micro XRCE-DDS Agent](https://github.com/eProsima/Micro-XRCE-DDS-Agent).
