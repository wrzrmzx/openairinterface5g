<h1 align="center">
    <a href="https://openairinterface.org/"><img src="https://openairinterface.org/wp-content/uploads/2015/06/cropped-oai_final_logo.png" alt="OAI" width="550"></a>
</h1>

<p align="center">
    <a href="https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-OAI--Public--V1.1-blue" alt="License"></a>
    <a href="https://releases.ubuntu.com/18.04/"><img src="https://img.shields.io/badge/OS-Ubuntu18-Green" alt="Supported OS"></a>
    <a href="https://www.redhat.com/en/enterprise-linux-8"><img src="https://img.shields.io/badge/OS-RHEL8-Green" alt="Supported OS"></a>
</p>

<p align="center">
    <a href="https://jenkins-oai.eurecom.fr/job/RAN-Container-Parent/"><img src="https://img.shields.io/jenkins/build?jobUrl=https%3A%2F%2Fjenkins-oai.eurecom.fr%2Fjob%2FRAN-Container-Parent%2F&label=build%20Images"></a>
</p>

<p align="center">
  <a href="https://hub.docker.com/r/rdefosseoai/oai-enb"><img alt="Docker Pulls" src="https://img.shields.io/docker/pulls/rdefosseoai/oai-enb?label=eNB%20docker%20pulls"></a>
  <a href="https://hub.docker.com/r/rdefosseoai/oai-lte-ue"><img alt="Docker Pulls" src="https://img.shields.io/docker/pulls/rdefosseoai/oai-lte-ue?label=LTE-UE%20docker%20pulls"></a>
  <a href="https://hub.docker.com/r/rdefosseoai/oai-gnb"><img alt="Docker Pulls" src="https://img.shields.io/docker/pulls/rdefosseoai/oai-gnb?label=gNB%20docker%20pulls"></a>
  <a href="https://hub.docker.com/r/rdefosseoai/oai-nr-ue"><img alt="Docker Pulls" src="https://img.shields.io/docker/pulls/rdefosseoai/oai-nr-ue?label=NR-UE%20docker%20pulls"></a>
</p>

# OpenAirInterface License #

OpenAirInterface is under OpenAirInterface Software Alliance license.

 *  [OAI License Model](http://www.openairinterface.org/?page_id=101)
 *  [OAI License v1.1 on our website](http://www.openairinterface.org/?page_id=698)

It is distributed under **OAI Public License V1.1**.

The license information is distributed under [LICENSE](LICENSE) file in the same directory.

Please see [NOTICE](NOTICE.md) file for third party software that is included in the sources.

# Where to Start #

 *  [The implemented features](./doc/FEATURE_SET.md)
 *  [How to build](./doc/BUILD.md)
 *  [How to run the modems](./doc/RUNMODEM.md)

# RAN repository structure #

The OpenAirInterface (OAI) software is composed of the following parts: 

<pre>
openairinterface5g
├── ci-scripts        : Meta-scripts used by the OSA CI process. Contains also configuration files used day-to-day by CI.
├── cmake_targets     : Build utilities to compile (simulation, emulation and real-time platforms), and generated build files.
├── common            : Some common OAI utilities, other tools can be found at openair2/UTILS.
├── doc               : Contains an up-to-date feature set list and starting tutorials.
├── executables       : Top-level executable source files.
├── LICENSE           : License file.
├── maketags          : Script to generate emacs tags.
├── nfapi             : Contains the NFAPI code. A local Readme file provides more details.
├── measure           : Sketch流量测量模块代码,测量结果通过Socket传递给流识别模块同时写入数据库.
├── measure_log       : 流量测量结果输出日志
├── auto-scripts      : OAI编译和运行脚本
│   ├── enb
│   ├── udp
│   ├── ue
├── openair1          : 3GPP LTE Rel-10/12 PHY layer / 3GPP NR Rel-15 layer. A local Readme file provides more details.
│   ├── PHY
│   ├── SCHED
│   ├── SCHED_NBIOT
│   ├── SCHED_NR
│   ├── SCHED_NR_UE
│   ├── SCHED_UE
│   └── SIMULATION    : PHY RF simulation.
├── openair2          : 3GPP LTE Rel-10 RLC/MAC/PDCP/RRC/X2AP + LTE Rel-14 M2AP implementation. Also 3GPP NR Rel-15 RLC/MAC/PDCP/RRC/X2AP.
│   ├── COMMON
│   ├── DOCS
│   ├── ENB_APP
│   ├── F1AP
│   ├── GNB_APP
│   ├── LAYER2/RLC/   : with the following subdirectories: UM_v9.3.0, TM_v9.3.0, and AM_v9.3.0.
│   ├── LAYER2/PDCP/PDCP_v10.1.0
│   ├── M2AP
│   ├── MCE_APP
│   ├── NETWORK_DRIVER
│   ├── NR_PHY_INTERFACE
│   ├── NR_UE_PHY_INTERFACE
│   ├── PHY_INTERFACE
│   ├── RRC
│   ├── UTIL
│   └── X2AP
├── openair3          : 3GPP LTE Rel10 for S1AP, NAS GTPV1-U for both ENB and UE.
│   ├── COMMON
│   ├── DOCS
│   ├── GTPV1-U
│   ├── M3AP
│   ├── MME_APP
│   ├── NAS
│   ├── S1AP
│   ├── SCTP
│   ├── SECU
│   ├── TEST
│   ├── UDP           : 添加网络流量测量功能
│   └── UTILS
└── targets           : Top-level wrappers for unitary simulation for PHY channels, system-level emulation (eNB-UE with and without S1), and realtime eNB and UE and RRH GW.
</pre>

# 部署流程

## 1、数据库安装

（1）安装MYSQL数据库

（2）设置root用户密码为123456，新建一个mytestdb的数据库（推荐）

> 或者可以通过修改代码的方式配置数据库
>
> 在measure\measure_timer.c的第53行配置了数据库连接的相关信息:

```c
conn_ptr = mysql_real_connect(conn_ptr,"localhost","root","123456","mytestdb",0,NULL,0);
```

> 可以通过修改localhost、root、123456、mytestdb使用其它账户和库

（3）创建数据表

- 数据表所在github地址:https://github.com/oai-group/sql.git
- 流量测量和流量识别需要4张表
  - [measure.sql](https://github.com/oai-group/sql/blob/master/measure.sql):流量统计信息和流识别结果
  - [total.sql](https://github.com/oai-group/sql/blob/master/total.sql)
  - [total_recv.sql](https://github.com/oai-group/sql/blob/master/total_recv.sql):下行流量信息
  - [total_send.sql](https://github.com/oai-group/sql/blob/master/total_send.sql):上行流量信息
- 连接数据库后可以通过以下命令导入数据表（此处以mytestdb为例）

```sql
use mytestdb;
/* 注意将下面的*.sql修改成文件的绝对路径 */
source measure.sql
source total.sql
source total_recv.sql
source total_send.sql
```

## 2、流量识别模块

（1）流量识别模块通过监听本机端口接收eNB的测量信息来判断流的类型，将eNB和流量识别模块安装在同一台主机上可以减少配置文件的修改。

> 流量识别模块github地址:https://github.com/oai-group/oaiflowclassification.git

（2）若数据库在localhost且用户名密码为root和123456，数据库为mytestdb（确保4张表已经创建好），则无需修改数据库连接信息

> 数据库连接信息位于oaiflowclassification/pool.py

```python
# 数据库地址
DB_ADDR = 'localhost'
# 数据库用户名
USER_NAME = 'root'
# 数据库用户密码
USER_PASSWORD = '123456'
# 数据库名称
DB_NAME = 'mytestdb'
```

> 可以通过修改localhost、root、123456、mytestdb使用其它账户和库

## 3、部署eNB和UE

- eNB
  - 若为首次编译，请使用脚本:openairinterface5g/auto-scripts/enb/auto-build-enb.sh
  - 若不是首次编译，请使用脚本:openairinterface5g/auto-scripts/enb/auto-rebuild-enb.sh

- UE
  - 若为首次编译，请使用脚本:openairinterface5g/auto-scripts/ue/auto-build-ue.sh
  - 若不是首次编译，请使用脚本:openairinterface5g/auto-scripts/ue/auto-rebuild-ue.sh

> 编译参数选择的是-w SIMU ,但他同时也会加载rfsim模块，测试时是通过rfsim模块使用物理信道进行仿真的。

## 4、测试教程

（1）启动流量识别模块

```shell
cd oaiflowclassification
python3 main.py
```

> 启动时python可能会缺少各种模块，按照提示安装即可
>
> 启动成功会提示:服务器正在监听...

（2）noS1简单测试

- eNB
  - 运行脚本:openairinterface5g/auto-scripts/enb/rfsim-noS1/auto-run-enb.sh启动eNB

- UE

  - 运行脚本:openairinterface5g/auto-scripts/ue/rfsim-noS1/auto-run-ue.sh启动UE

    - 不加参数会提示:请输入当前UE的eNB接口IP地址。请输入eNB的IP地址，确保改IP地址能和UE所在主机Ping通

    - 或者在运行脚本时直接携带参数:

      ```shell
      # 请将eNB_IP替换成eNB的IP地址
      sudo ./auto-run-ue.sh -a eNB_IP
      ```