# 列车 API

7.0版本引入了Train API，提供了属性和方法来控制列车。它是服务器端vehicle类的一部分，并且是同步的。本文将为您概述该API以及如何使用它。

## 属性

| 属性名            | 描述                                                                                                                           |
| ------------------------- | -------------------------------------------------------------------                                                                   |
| isMissionTrain            | 获取或设置创建的列车是否是任务列车。                                                                                 |
| trainTrackId              | 获取或设置列车的轨道id。[有效id: 0 - 11]                                                                   |
| trainEngineId             | 获取火车的引擎。   | 
| trainConfigIndex          | 获取或设置火车的配置索引。你可以在trains.xml中找到所有可能的配置索引列表。   |
| trainDistanceFromEngine   | 获取或设置火车与引擎之间的距离。                                                                               |
| isTrainEngine             | 获取或设置火车是否是列车的引擎。                                                                                 |
| isTrainCaboose            | 如果火车是车尾，获取或设置。                                                                                              |
| trainDirection            | 获取或设置列车的方向。(`true`表示向前，`false`表示向后)                                              |
| trainPassengerCarriages   | 获取或设置(如果列车是客运车厢)。                                                                              |
| trainRenderDerailed       | 获取或设置(如果火车被渲染脱轨)。                                                                                     |
| trainForceDoorsOpen       | 获取或设置是否应该强行打开列车门。                                                                       |
| trainCruiseSpeed          | 获取或设置火车巡航速度。                                                                                         |
| trainCarriageConfigIndex  | 获取或设置列车车厢的配置索引。                                                                                |
| trainLinkedToBackwardId   | 获取或设置另一列与列车尾部相连的列车。                                                                  |
| trainLinkedToForwardId    | 获取或设置与列车前端相连的另一列列车。                                                                 |

## Methods

> [!WARNING]
> 请记住，这些方法只有在刷出时才有效。

| 方法名                   | 描述                                       |
| ----------------------------- | ---------------------------------------------     |
| setTrainEngineId              | 把火车用作火车的引擎。        |
| setTrainLinkedToBackwardId    | 把另一列火车连接到火车的尾部。     |
| setTrainLinkedToForwardId     | 将另一列火车连接到列车的前部。    |  

## 代码示例

下面的代码创建在轨道1上行驶的地铁列车。这条轨道从城市到帕莱托湾，然后返回。

### Single train

```js
let vehicle = new alt.Vehicle(alt.hash("metrotrain"), 0, 0, 0, 0, 0, 0);
vehicle.isMissionTrain = false;
vehicle.trainTrackId = 1;
vehicle.setTrainEngineId(null);
vehicle.trainConfigIndex = 25;
vehicle.trainDistanceFromEngine = 0;
vehicle.isTrainEngine = true;
vehicle.isTrainCaboose = false;
vehicle.trainDirection = true;
vehicle.trainPassengerCarriages = false;
vehicle.trainRenderDerailed = false;
vehicle.trainForceDoorsOpen = false;
vehicle.trainCruiseSpeed = 5;
vehicle.trainCarriageConfigIndex = 0;
vehicle.setTrainLinkedToBackwardId(null);
vehicle.setTrainLinkedToForwardId(null);
```

### 链接火车

为了将两列火车连接起来，你需要使用`setTrainLinkedToBackwardId`和` settrainlinkedtoforwardid `方法。同样重要的是，一列火车需要成为火车的引擎，另一列火车需要成为车厢。否则，你需要手动通过` traindistancefromvehicle `设置第二辆火车的距离。

```js
  // 整列火车在轨道3上行驶，从轨道3的节点索引1开始。
  
  let vehicle = new alt.Vehicle(alt.hash("metrotrain"), 193, -603, 16, 0, 0, 0);
  vehicle.isMissionTrain = false;
  vehicle.trainTrackId = 3;
  vehicle.setTrainEngineId(null);
  vehicle.trainConfigIndex = 25;
  vehicle.trainDistanceFromEngine = 0;
  vehicle.isTrainEngine = true; // 让这列火车成为整列火车的引擎
  vehicle.isTrainCaboose = false;
  vehicle.trainDirection = false;
  vehicle.trainPassengerCarriages = false; // 使这列火车成为乘客车厢
  vehicle.trainRenderDerailed = false;
  vehicle.trainForceDoorsOpen = false;
  vehicle.trainCarriageConfigIndex = 1;
  vehicle.setTrainLinkedToForwardId(null);

  let vehicle2 = new alt.Vehicle(alt.hash("metrotrain"), 193, -603, 16, 0, 0, 0);
  vehicle2.isMissionTrain = false;
  vehicle2.trainTrackId = 3;
  vehicle2.setTrainEngineId(vehicle);
  vehicle2.trainConfigIndex = 25;
  vehicle2.trainDistanceFromEngine = 0;
  vehicle2.isTrainEngine = false; // 关闭这列火车作为整列火车的引擎
  vehicle2.isTrainCaboose = false;
  vehicle2.trainDirection = false;
  vehicle2.trainPassengerCarriages = true; // 把这列火车当作乘客车厢
  vehicle2.trainRenderDerailed = false;
  vehicle2.trainForceDoorsOpen = false;
  vehicle2.trainCarriageConfigIndex = 1;
  
  vehicle2.setTrainLinkedToBackwardId(null); // 不要把火车连接到这辆车的后面
  vehicle2.setTrainLinkedToForwardId(vehicle); // 将车辆连接到车辆的前部
  vehicle.setTrainLinkedToBackwardId(vehicle2); // 将车辆2连接到车辆的后部

```