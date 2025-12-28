# [Qt 笔记] JSON 配置文件解析与数据驱动设计

> **📌 项目备注**：这份笔记以“**自助雨伞租赁系统**”的站点配置为实例编写，主要解决地图静态站点的坐标映射与信息加载问题。

## 1. 核心架构思想：数据与逻辑分离

在雨伞租借系统中，站点的坐标、名称等属于**静态配置**。

- **做法**：将这些数据写入 `JSON` 文件，而不是硬编码在 C++ 代码里。
- **优势**：修改站点位置无需重新编译代码，只需修改 JSON 文件即可；代码逻辑更纯粹，易于维护。

------

## 2. 基础数据模型 (The Model)

使用 `struct`（结构体）作为 **POD (Plain Old Data)** 对象，仅用于存放从 JSON 提取出的数据。

C++

```
struct StationConfig {
    int stationId;      // 唯一标识符（如：1, 2, 3...）
    QString name;       // 站点名称（如：文德楼）
    double posX;        // 地图横坐标百分比 (0.0 - 1.0)
    double posY;        // 地图纵坐标百分比 (0.0 - 1.0)
    QString description;// 站点描述（如：主要教学楼）
};
```

------

## 3. Qt JSON 解析全流程 (5 个阶段)

解析 JSON 就像是将“原始材料”翻译成“精密零件”的过程。

### 阶段 1：读取原始字节流 (`QFile` -> `QByteArray`)

从资源文件 (`:/map/map_config.json`) 读取二进制数据。

C++

```
QFile file(":/map/map_config.json");
if (!file.open(QIODevice::ReadOnly)) return;
QByteArray data = file.readAll(); 
file.close();
```

### 阶段 2：构建逻辑蓝图 (`QByteArray` -> `QJsonDocument`)

将原始字节解析为有结构的 JSON 文档对象。

C++

```
QJsonDocument doc = QJsonDocument::fromJson(data);
```

### 阶段 3：拆解容器层级 (`QJsonObject` & `QJsonArray`)

按照 JSON 的嵌套结构逐层进入。

C++

```
QJsonObject root = doc.object(); // 获取最外层 { }
QJsonArray array = root["stations"].toArray(); // 获取内部的 [ ] 站点列表
```

### 阶段 4：翻译为 C++ 对象 (循环提取)

遍历数组，将 JSON 内部的通用类型手动转换为 C++ 结构体。

C++

```
for (const QJsonValue &value : array) {
    QJsonObject obj = value.toObject(); 
    StationConfig cfg;
    cfg.stationId = obj["station_id"].toInt();    // 翻译为 int
    cfg.name = obj["name"].toString();            // 翻译为 QString
    cfg.posX = obj["pos_x"].toDouble();           // 翻译为 double
    // ...
}
```

### 阶段 5：分类归档 (`QMap<int, StationConfig>`)

将解析好的结构体存入带有索引功能的容器。

- **Key**: `stationId` —— 就像储物柜的**号码牌**。
- **Value**: `StationConfig` —— 柜子里存放的**站点详细档案**。

------

## 4. Qt JSON 核心类快查表

| **类名**            | **作用**                 | **形象比喻** |
| ------------------- | ------------------------ | ------------ |
| **`QJsonDocument`** | 整个 JSON 文档的管理中心 | 解析说明书   |
| **`QJsonObject`**   | 键值对集合，对应 `{ }`   | 快递盒       |
| **`QJsonArray`**    | 有序列表，对应 `[ ]`     | 物品清单     |
| **`QJsonValue`**    | 通用的基础数据单元       | 待确定的零件 |