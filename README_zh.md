# 轻量级分布式组件管理部件<a name="ZH-CN_TOPIC_0000001128264105"></a>

-   [简介](#section11660541593)
-   [目录](#section1464106163817)
-   [约束](#section1718733212019)
-   [使用](#section10729231131110)
-   [相关仓](#section176111311166)

## 简介<a name="section11660541593"></a>

轻量级分布式组件管理模块负责跨设备启动FA的能力，支持分布式场景下的应用协同。轻量级分布式调度模块组成如下图所示：

![](figures/zh-cn_image_0000001081284974.png)

## 目录<a name="section1464106163817"></a>

轻量级分布式组件管理源代码目录结构如下表所示：

**表1**主要源代码目录结构

<a name="table43531856201716"></a>
<table><thead align="left"><tr id="row20416556201718"><th class="cellrowborder" valign="top" width="50%" id="mcps1.1.3.1.1"><p id="p10416456121716"><a name="p10416456121716"></a><a name="p10416456121716"></a>名称</p>
</th>
<th class="cellrowborder" valign="top" width="50%" id="mcps1.1.3.1.2"><p id="p1841645631717"><a name="p1841645631717"></a><a name="p1841645631717"></a>描述</p>
</th>
</tr>
</thead>
<tbody><tr id="row64161056151718"><td class="cellrowborder" valign="top" width="50%" headers="mcps1.1.3.1.1 "><p id="p4160914132218"><a name="p4160914132218"></a><a name="p4160914132218"></a>dmsfwk_lite</p>
</td>
<td class="cellrowborder" valign="top" width="50%" headers="mcps1.1.3.1.2 "><p id="p541645611177"><a name="p541645611177"></a><a name="p541645611177"></a>轻量级分布式组件管理</p>
</td>
</tr>
</tbody>
</table>

其中轻量级分布式组件管理的源代码目录结构如下：

```
├── BUILD.gn
├── include
│  ├── dmslite.h        # 轻量级分布式组件管理对外接口
│  ├── dmslite_check_remote_permission.h     # 轻量级分布式组件管理权限管理模块
│  ├── dmslite_famgr.h                       # 轻量级分布式组件管理FA管理模块
│  ├── dmslite_inner_common.h                # 轻量级分布式组件管理内部通用文件
│  ├── dmslite.h                             # 轻量级分布式组件管理服务实现
│  ├── dmslite_log.h                         # 日志模块
│  ├── dmslite_parser.h                  # 分布式消息解析模块
│  ├── dmslite_tlv_common.h                  # TLV格式数据解析模块
│  └── dmslite_session.h                     # 跨设备通信收发模块
├── readme.md
├── LICENSE
├── source
    ├── distributed_schedule_service.c
    ├── dmslite.c
    ├── dmslite_check_remote_permission.c
    ├── dmslite_famgr.c
    ├── dmslite_msg_parser.c
    ├── dmslite_tlv_common.c
    └── dmslite_session.c
```

## 约束<a name="section1718733212019"></a>

**语言限制**：C/C++语言

**组网环境**：必须确保设备在同一个局域网中，主从设备能互相ping通

**操作系统限制**：OpenHarmony操作系统

**远程启动的约束与限制：**

-   支持远程启动FA
-   远程启动前必须确保主设备与从设备间分布式组网成功，否则无法远程启动

## 使用<a name="section10729231131110"></a>

-   **轻量级分布式组件管理模块编译**

轻量级分布式组件管理模块，其代码所在目录如下：

```
foundation/ability/dmsfwk_lite
```

在针对不同平台进行编译时，需要提前指定目标平台：

```
hb build
```

-   **主设备程序开发**（以拉起FA为例）

构造意图参数want，设置需要启动的远端设备ID，包名，元能力类名信息，以及分布式标志位Want.FLAG\_ABILITYSLICE\_MULTI\_DEVICE以使能分布式启动

```
import ohos.aafwk.ability.Ability;
import ohos.aafwk.content.Want;
import ohos.bundle.ElementName;

// 构造want参数
Want want = new Want();
ElementName name = new ElementName(remote_device_id, "ohos.dms.remote_bundle_name", "remote_ability_name"); 
want.setElement(name); // 将待启动的FA信息添加到Want中
want.setFlags(Want.FLAG_ABILITYSLICE_MULTI_DEVICE); // 设置分布式标记，若不设置将无法使用分布式能力

// 启动远程设备FA
startAbility(want); // 按照Want启动指定FA，want参数命名以实际开发平台API为准
```

-   **预置条件**

从设备侧需安装对应包名的FA

-   **运行**（以拉起FA为例）

执行主设备侧的startAbility即可拉起从设备FA

## 相关仓<a name="section176111311166"></a>

轻量级分布式组件管理

[**dmsfwk\_lite**](https://gitee.com/openharmony/ability_dmsfwk_lite)

