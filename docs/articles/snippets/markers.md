# 标记

标记是3D世界中那些稍微透明的物体，你可以步行或开车穿过它们。

**Client Side**

```js
import * as alt from 'alt-client';

// 使用默认选项在本地玩家pos创建标记
const defaultMarker = new alt.Utils.Marker(alt.Player.local.pos);

// 在0,0,72处创建红色的默认标记
const redMarker = new alt.Utils.Marker(new alt.Vector3(0, 0, 72), { color: alt.RGBA.red });

// 之后你可以改变颜色、比例等。
redMarker.scale = new alt.Vector3(3.0); // Makes marker bigger
```
