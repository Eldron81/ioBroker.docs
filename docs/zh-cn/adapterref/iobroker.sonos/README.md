---
translatedFrom: en
translatedWarning: 如果您想编辑此文档，请删除“translatedFrom”字段，否则此文档将再次自动翻译
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/zh-cn/adapterref/iobroker.sonos/README.md
title: ioBroker.sonos
hash: 8ki7pVY1vipOx7uD6wir5Vt5QLb7JfSXgEcoFRN4qUs=
---
![商标](../../../en/adapterref/iobroker.sonos/admin/sonos.png)

![安装数量](http://iobroker.live/badges/sonos-stable.svg)
![NPM 版本](http://img.shields.io/npm/v/iobroker.sonos.svg)
![下载](https://img.shields.io/npm/dm/iobroker.sonos.svg)

#ioBroker.sonos
![测试和发布](https://github.com/ioBroker/ioBroker.sonos/workflows/Test%20and%20Release/badge.svg) [![翻译状态](https://weblate.iobroker.net/widgets/adapters/-/sonos/svg-badge.svg)](https://weblate.iobroker.net/engage/adapters/?utm_source=widget)

使用 ioBroker 控制和监控 SONOS 设备。

## 组的处理
* 处理 SONOS 组的状态：
   * **`coordinator`**：设置/获取协调器，即主控和协调组的 SONOS 设备。它需要 SONOS 设备的 IP 地址（通道名称）作为协调器，但使用下划线“_”而不是点“.”，因此使用例如“192_168_0_100”来表示 IP 地址“192.168.0.100”。如果设备不属于任何组，则该值等于自己的通道名称 (IP)。
   * **`group_volume`**: 组的音量
   * **`group_muted`**: 组的静音状态。
   * **`add_to_group`**：将某个SONOS设备添加到该状态下的SONOS设备。使用带下划线的 IP 地址（见上文）。
   * **`remove_from_group`**：从该状态下的SONOS设备中移除某个SONOS设备。使用带下划线的 IP 地址（见上文）。

*) 如果在 SONOS 应用程序中进行更改，这些状态将更新。

## 与 sayIt 适配器一起使用
要使用 [sayit 适配器](https://github.com/ioBroker/ioBroker.sayit) 与此 SONOS 适配器，确保 [web 适配器](https://github.com/ioBroker/ioBroker.web) 也会实例化并运行。 Web 适配器需要允许 SONOS 适配器从 sayit 适配器读取生成的 MP3 文件。

### 警告：结合 sayIt 适配器的稳定性问题
请注意：如果将“文本转语音”与 sayIt 适配器一起使用，则此 SONOS 适配器会出现稳定性问题。观察到的症状：

1. 任意将音量更改为 0 或 100 %。
2.随机数个文本转语音序列后无响应

文本转语音的解决方法是使用 [SONOS HTTP API](https://github.com/jishi/node-sonos-http-api)。

## VIS 中的收藏夹和队列
使用状态 `favorites_list_html` 和 `queue_html` 在 VIS 中使用基本的 html 小部件显示播放列表和当前队列。通过单击一行，播放列表或曲目将立即播放。
使用以下 css 类格式化表格：

###收藏夹
* `sonosFavoriteTable`: 最喜欢的表
* `sonosFavoriteRow`: 包含收藏信息的行
* `sonosFavoriteNumber`: 收藏数量
* `sonosFavoriteCover`：最喜欢的专辑封面（使用 `.sonosFavoriteCover img` 抓取图像）
* `sonosFavoriteTitle`: 最喜欢的名字

＃＃＃ 队列
*`.sonosQueueTable`：孔表
* `.sonosQueueRow`：包含轨道信息的行
* `.currentTrack`: 添加到包含当前播放曲目的行
* `.sonosQueueTrackNumber`: 编号或轨道
* `.sonosQueueTrackCover`：曲目的专辑封面（使用 `.sonosQueueTrackCover img` 抓取图像）
* `.sonosQueueTrackArtist`: 艺术家姓名
* `.sonosQueueTrackAlbum`：专辑名称（如果不需要，请使用 `display:none`）
* `.sonosQueueTrackTitle`: 标题名称

对于长列表，将 `overflow:auto;` 或 `overflow-y:auto;` 添加到基本 html 小部件。
请注意：不支持突出显示当前播放的最爱。

### 示例 CSS
```
.sonosFavoriteTable {
    color: #bbb;
    font-size: 12px;
}
.sonosFavoriteRow {
    cursor: pointer;
}
.sonosFavoriteNumber {}
.sonosFavoriteCover img {
    width: 30px;
    height: 30px;
}
.sonosFavoriteTitle {}

.sonosQueueTable {
    color: #bbb;
    font-size: 12px;
}
.sonosQueueRow {
    display: table-row;
    cursor: pointer;
}
.sonosQueueRow.currentTrack {
    color: #fff;
    font-weight: bold;
}
.sonosQueueTrackNumber {}
.sonosQueueTrackCover img {
    width: 30px;
    height: 30px;
    display: table-column;
}
.sonosQueueTrackArtist {
    display: table-row;
}
.sonosQueueTrackAlbum {
    display: none;
}
.sonosQueueTrackTitle {
    display: table-row;
}
```

＃＃ 去做
* 使用 https://github.com/svrooij/node-sonos-ts 重写

＃＃ 配置
- Web 服务器 - [可选] 是否启用 Web 服务器
- 端口 - 如果启用了 Web 服务器，则为该端口。默认 8083
- 更新经过时间（毫秒） - 以毫秒为单位的间隔，在播放标题时更新经过的计时器的频率。 （默认 2000）

<!-- 下一个版本的占位符（在行首）：

### **正在进行中** -->

## Changelog
### WORK IN PROGRESS
* (Standarduser & Cee-Jey) Added new states `favorites_list_html` and `queue_html with covers`
* (Standarduser) Changed default album art if no cover was found

### 2.2.3 (2022-07-04)
* (Rello) Added track number state

### 2.2.2 (2022-06-12)
* (Apollon77) Prevent js-controller warnings

### 2.2.1 (2022-06-12)
* (Apollon77) fix group volume state

### 2.2.0 (2022-06-08)
* (Apollon77) Remove logic that adjusted the group volume after one member volume was set
* (Apollon77) Make sure to not set state values for devices that are not configured
* (Apollon77) Try to catch network errors

### 2.1.7 (2021-12-20)
* (bluefox) Corrected error with "SONOS not found"

### 2.1.4 (2021-12-17)
* (bluefox) Catch possible errors by the start and unload

### 2.1.1 (2020-11-08)
* (Apollon77) Prevent crash case (Sentry IOBROKER-JS-CONTROLLER-S7, #78)

### 2.1.0 (2020-05-31)
* (bluefox) TTS Refactoring

### 2.0.2 (2020-05-25)
* (bluefox) Refactoring

### 2.0.1 (2019-11-04)
* (klein0r) create sonos cache directory

### 2.0.0 (2019-08-13)
* (bluefox) no web server any more
* (bluefox) update all used npm packages

### 1.8.0 (2019-01-04)
* (bluefox) Support js-controller compact mode

### 1.7.7 (2018-08-06)
* (bluefox) Fixed error with node.js 6

### 1.7.5 (2018-08-06)
* (bluefox) Trying to correct fade-out

### 1.7.4 (2018-07-23)
* (bluefox) The group volume has a valid role now
* (bluefox) Important changes: state cover.png renamed to "cover_png"
* (bluefox) added shuffle, repeat and crossfade modes. To enable it you must delete device from list and add it again
* (bluefox) better icon
* (bluefox) fix fade out option

### 1.7.1 (2018-07-17)
* (bluefox) Ready for npm6

### 1.7.0 (2018-07-16)
* (bluefox) Added the support of Admin3

### 1.6.2 (2017-08-16)
* (soef) no duration/elapsed update on radio

### 1.6.0 (2017-04-09)
* (justr1) Enhance group Handling

### 1.5.0 (2017-02-23)
* (bluefox) use new configuration dialog

### 1.4.4 (2017-01-29)
* (soef) removeFromGroup extended

### 1.4.3 (2017-01-08)
* (bluefox) Allow to use the sonos web via with proxy

### 1.4.2 (2016-12-29)
* (bluefox) add states for vis control and change some roles

### 1.3.1 (2016-12-27)
* (bluefox) Fix TTS if fade was 0

### 1.3.0 (2016-12-13)
* (bluefox) Fix api changes of SONOS module

### 1.2.1 (2016-12-10)
* (bluefox) add web adapter as dependency

### 1.2.0 (2016-10-25)
* (bluefox) tts was rewritten because of new sonos-discovery interface

### 1.1.0 (2016-10-20)
* (bluefox) update sonos npm packets
* (bluefox) configurable fadeIn and fadeOut

### 1.0.0 (2016-10-16)
* (bluefox) fix fade out

### 0.2.2 (2016-09-30)
* (bluefox) fix types of states

### 0.2.1 (2016-09-25)
* (soef) fixed restore of radio after sayIt

### 0.2.0 (2016-07-28)
* (soef) fixed restore of radio after sayIt
* (bluefox) fix log outputs
* (bluefox) update libraries and use fix versions of it

### 0.1.10 (2016-05-26)
* (bluefox) check type of "state"

### 0.1.9 (2016-05-20)
* (bluefox) change default port to 8080

### 0.1.8 (2016-02-22)
* (hagen) - Better handling of radio stations (show -> album, streamInfo -> artist)
* (hagen) New state 'current_type' to tell if a track or radio is playing
* (hagen) (Hopefully) fixed the unreliable cover art update

### 0.1.7 (2015-09-14)
* (bluefox) fix favorites set

### 0.1.6 (2015-02-25)
* (bluefox) implement tts if radio is playing

### 0.1.5 (2015-02-13)
* (bluefox) set volume by TTS

### 0.1.4 (2015-01-04)
* (bluefox) catch errors if states deleted

### 0.1.3 (2015-01-02)
* (bluefox) enable npm install

### 0.1.2 (2014-12-12)
* (bluefox) redirect logging messages to ioBroker

### 0.1.1 (2014-12-04)
* (bluefox) translate grid in config dialog

### 0.1.0 (2014-12-04)
* (bluefox) use sonos-web-controller module as tarball from git

### 0.0.5 (2014-11-24)
* (bluefox) support of new naming concept

### 0.0.4 (2014-11-22)
* (bluefox) support of text to speech

### 0.0.3 (2014-11-01)
* (bluefox) support of text to speech and cover image

### 0.0.2 (2014-11-01)
* (bluefox) improve configuration edit

## License

The MIT License (MIT)

Copyright (c) 2014-2022, bluefox <dogafox@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.