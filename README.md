# @linkdesign/gis

GIS 组件库

基于高德地图，react组件化了一些基础覆盖物类，内置/封装了一些常用GIS工具函数、[高德api](https://lbs.amap.com/api/javascript-api-v2/summary)以及功能组件；
整合了[Leafletjs](https://leafletjs.com/)，支持离线地图；

![组件目录](https://intranetproxy.alipay.com/skylark/lark/0/2022/png/111014/1645414841083-07112e03-a004-496b-9d85-9f0d74ecca66.png) 

## 安装

```shell
npm install @linkdesign/gis --save
```

## 使用

首先在应用的入口文件（entry）引入样式文件：

```js
// In your entry
// 内部依赖 @alifd/next，但打包编译后的样式文件不包含 @alifd/next 的样式（避免样式覆盖导致的诸多问题）
// 如果没有引入，需要另外引入： import '@alifd/next/index.css';
import '@linkdesign/gis/lib/index.less';
```

使用组件：

```js
import { Map, Marker, Utils } from '@linkdesign/gis';

const { turf, gcoord } = Utils;
```

## 能力一览
### 基础组件
- [x] 在线地图、离线地图
- [x]  点、线、面
	- [x] 点标记、圆点
	- [x] 圆、矩形、多边形 
- [x] 图层
	- [x] TileLayer
	- [x] GeoJSON
	- [x] esri
		- [x] TiledMapLayer
		- [x] DynamicMapLayer
		- [x] AuthMapLayer
- [x] 气泡、信息窗
- [x] 控件：比例尺、旋转缩放
- [x] 坐标系转换
- [x] 拖拽选址
- [x] 位置预览
- [x] 覆盖物编辑器
- [x] json编辑器
- [x] [GIS计算函数库 turf](https://turfjs.org/)（中心点、bounds、点线面关系判断等）
- [x] [坐标转换工具 gcoord](https://www.npmjs.com/package/gcoord)
- [ ] ......

### 业务组件
- [x] 点聚合
- [x] 海量点
- [ ] ......

## DEMO

_**!!! demo 中使用的 key 都是测试专用，随时可能失效，切勿在线上环境中使用 !!！**_

_**!!! demo 中使用的 key 都是测试专用，随时可能失效，切勿在线上环境中使用 !!！**_

_**!!! demo 中使用的 key 都是测试专用，随时可能失效，切勿在线上环境中使用 !!！**_

### Map

```jsx
import { config, Map } from '@linkdesign/gis';

config.key = 'e51afcf01df9d306ad46e7614964ce3e';

const App = () => {
  return (
    <Map
      onComplete={(map) => {
      window.map = map;
      }}
      onClick={(a, b) => {
      console.log(a, b);
      }}
      onMoveEnd={(a, b) => {
      console.log(a, b);
      }}
      onZoomEnd={(a, b) => {
      console.log(a, b);
      }}
    />
  );
}

ReactDOM.render(<App />);
```

### 点、线、面

```jsx
import React, { useEffect, useRef } from 'react';
import { Map, config, Utils, Marker, Polygon, Circle, Rectangle } from '@linkdesign/gis';

const { gcoord, turf } = Utils;

const PATH = [[
  [116.472958, 39.995534],
  [116.472161, 39.994808],
  [116.471294, 39.995364],
  [116.47126, 39.995384],
  [116.471239, 39.995388],
  [116.471218, 39.995381],
  [116.471184, 39.995355],
  [116.470344, 39.994622],
  [116.469502, 39.993967],
  [116.469193, 39.993716],
  [116.469032, 39.993863],
  [116.468815, 39.994108],
  [116.468625, 39.994355],
  [116.468471, 39.99466],
  [116.468421, 39.994811],
  [116.468366, 39.995156],
  [116.468306, 39.996157],
  [116.468308, 39.996557],
  [116.468483, 39.996884],
  [116.468834, 39.997188],
  [116.469481, 39.997764],
  [116.470511, 39.998708],
  [116.471404, 39.999517],
  [116.471553, 39.999568],
  [116.471713, 39.999563],
  [116.471929, 39.999463],
  [116.473228, 39.998584],
  [116.474008, 39.998046],
  [116.474541, 39.997674],
  [116.474541, 39.997576],
  [116.474604, 39.997049],
  [116.47457, 39.996944],
  [116.474103, 39.99657],
  [116.473535, 39.996057],
  [116.472958, 39.995534]
]];

const App = () => {
  return (
    <div style={{ height: 500 }}>
      <Map
        center={[116.471361, 39.996256]}
        zoom={16}
        mapStyle="amap://styles/whitesmoke"
        onComplete={(map) => {
          window.map = map;
        }}
      >
        <Marker
          position={[116.471361, 39.996256]}
          onAdd={(o, e) => {
            console.log('onAdd', o, e);
          }}
          onClick={(o, e) => {
            console.log('onClick', o, e);
          }}
          onMouseOver={(o, e) => {
            console.log('onMouseOver', o, e);
          }}
          onMouseOut={(o, e) => {
            console.log('onMouseOut', o, e);
          }}
        />
      
        <Polyline
          tooltip={{
            direction: 'top',
            content: '123',
            offset: [0, -10],
          }}
          path={[
            [116.472958, 39.995534],
            [116.468308, 39.996557],
          ]}
          strokeColor="#f00"
          strokeWeight="10"
          extData={{ text: 222 }}
          onAdd={(o, e) => {
            console.log('onAdd', o, e);
          }}
        />
    
        <Polygon path={PATH} />

        <Circle
          center={[116.471361, 39.996256]}
          radius={50}
          fillColor="#409EFF"
          strokeColor="#000A58"
          fillOpacity={0.5}
        />

        <Rectangle
          bounds={[
            [116.471404, 39.999517],
            [116.474008, 39.998046],
          ]}
          fillColor="#409EFF"
          strokeColor="#000A58"
          fillOpacity={0.5}
        />
      </Map>
	  </div>
  );
}

ReactDOM.render(<App />);
```

## 意见或建议

如果您有任何问题或建议，欢迎在 [issues](https://github.com/H5DevHoward/@linkdesign/gis/issues) 中反馈

对组件库的建设有任何想法或建议，请联系：

- [翊安](dingtalk://dingtalkclient/action/sendmsg?spm=a2o8d.corp_prod_issue_list.0.0.6a88718cyxurWU&dingtalk_id=vtyx2bd) (howard.zh@alibaba-inc.com、h5devhoward@gmail.com)

