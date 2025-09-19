# beijing-subway-schedule

北京地铁时刻表。

基于本项目开发的动态可视化列车运行图，请见[beijing-subway-routemap](https://github.com/BoyInTheSun/beijing-subway-routemap)。

2024年7月起，数据采用[Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools)，目前本项目仅维护*6号线*相关及`train_schedule_all`内文件。

## 项目结构

[imgs](imgs)是从[网站](#数据来源)爬取的原始**到站时刻表**图片。

[arrival_schedule](arrival_schedule)是经过处理的**到站时刻表**。部分内容可能和原始图片有出入，详见[数据处理](#数据处理)

[train_schedule](train_schedule)是由前者转换而来的**列车时刻表**，形如`{虚拟车号: [[站1, 05:00], [站2, 05:03], ...], ...}`。这里的`虚拟车号`是按照开始运行的时间由小至大排列的。
特别地，由于6号线存在越行列车，**在站待避列车**形如`{..., ["物资学院路", "07:35"], ["草房", "07:38"], ["常营", "07:40-"], ["常营", "07:43"], ["黄渠", "07:45"], ["褡裢坡", "07:47"], ...`，**越行列车**形如`{..., ["物资学院路", "07:37"], ["草房", "07:40"], ["常营", "(07:42"], ["黄渠", "(07:44"], ["褡裢坡", "07:45"], ...}`，存在情况`["通运门","(08:34-"]`

[train_schedule_all](train_schedule_all)是由前者合并而来的，区分工作日和双休日。形如`{线路名：{运行方向：{虚拟车号: [[站1, 05:00], [站2, 05:03], ...], ...}, ...}, ...}`。这里的`虚拟车号`形如`AABCCC`，`AA`代表线路名，详见[统计表格](#统计表格)，`B`代表双休日/单休日和运行方向，0-4代表工作日，5-9代表双休日，取模5余n代表运行方向（目前仅有0-1）。`CCC`按照列车开始运行的时间由小至大排列。

[station_spacing](station_spacing)是官网公布的站间距信息。虽然我也不知道这个有什么用。也许可以用来计算乘车价格？

## 数据来源

北京地铁有多个运营公司，分别是[北京市地铁运营有限公司](https://www.bjsubway.com/contact/)及其子公司、[北京京港地铁有限公司](https://www.mtr.bj.cn/)、[北京市轨道交通运营管理有限公司](http://www.bjmoa.cn/)、北京公交有轨电车有限公司等。在上述网站（的前二者）中，官网公布了各地铁站的**到站时刻表**

## 数据处理

爬取**到站时刻表**图片，用一定的规则将其转换为二值图，进行OCR，错误识别需人工纠错，转换为**列车时刻表**，期间需要对时刻表的错误进行人工纠错。

经过考虑后，我并不打算对处理过程的代码和中间文件开源，一是因为代码质量差，依赖复杂，可读性不佳，二是仅需单次运行，即可获得结果，且更新频率极低。

对于官方时刻表的错误，可以说是意料之中、情理之外。幸运的是，正确的时刻表是占多数的，我能根据其他推算出如何修改错误时刻表。或者说，我假定多数为正确的。

在[memo.md](memo.md)中展示了人工纠错的所有操作。

上述过程极易出现错误，如果你发现了任何问题，请提issue，我会及时核对。

## 统计表格

*除6号线外，更新时间为原作者更新时间。*

| 线路 | 代号 | 色号 | 运营公司 | 数据来源 | 上次更新 |
|---|---|---|---|---|---|
| 1号线八通线 | 01 | ![](https://img.shields.io/badge/%23C23A30-C23A30?style=flat-square) | 【运二】北京市地铁运营有限公司运营二分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2024-12-20 |
| 2号线 | 02 | ![](https://img.shields.io/badge/%23006098-006098?style=flat-square) | 【运三】北京市地铁运营有限公司运营三分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2024-12-21 |
| 3号线 | 03 | ![](https://img.shields.io/badge/%23CE093D-CE093D?style=flat-square) | 【运四】北京市地铁运营有限公司运营四分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-05-08 |
| 4号线大兴线 | 04 | ![](https://img.shields.io/badge/%23008E9C-008E9C?style=flat-square) | 【京港】北京京港地铁有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-10 |
| 5号线 | 05 | ![](https://img.shields.io/badge/%23A6217F-A6217F?style=flat-square) | 【运一】北京市地铁运营有限公司运营一分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-06-03 |
| 6号线 | 06 | ![](https://img.shields.io/badge/%23D29700-D29700) | 【运一】北京市地铁运营有限公司运营一分公司 | 本项目整理 | 2024-1-27 |
| 7号线 | 07 | ![](https://img.shields.io/badge/%23F6C582-F6C582?style=flat-square) | 【运一】北京市地铁运营有限公司运营一分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2023-12-01 |
| 8号线 | 08 | ![](https://img.shields.io/badge/%23009B6B-009B6B?style=flat-square) | 【运三】北京市地铁运营有限公司运营三分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2023-12-01 |
| 9号线 | 09 | ![](https://img.shields.io/badge/%238FC31F-8FC31F?style=flat-square) | 【运二】北京市地铁运营有限公司运营二分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2023-12-01 |
| 10号线 | 10 | ![](https://img.shields.io/badge/%23009BC0-009BC0?style=flat-square) | 【运三】北京市地铁运营有限公司运营三分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2023-12-01 |
| 11号线 | 11 | ![](https://img.shields.io/badge/%23ED796B-ED796B?style=flat-square) | 【运二】北京市地铁运营有限公司运营二分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |
| 12号线 | 12 | ![](https://img.shields.io/badge/%23BD6F16-BD6F16?style=flat-square) | 【运二】北京市地铁运营有限公司运营二分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-05-08 |
| 13号线 | 13 | ![](https://img.shields.io/badge/%23F9E700-F9E700?style=flat-square) | 【运三】北京市地铁运营有限公司运营三分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-05-01 |
| 14号线 | 14 | ![](https://img.shields.io/badge/%23D5A7A1-D5A7A1?style=flat-square) | 【京港】北京京港地铁有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-09-01 |
| 15号线 | 15 | ![](https://img.shields.io/badge/%23582C68-582C68?style=flat-square) | 【运四】北京市地铁运营有限公司运营四分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-07-04 |
| 16号线 | 16 | ![](https://img.shields.io/badge/%2376A32E-76A32E?style=flat-square) | 【京港】北京京港地铁有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |
| 17号线 | 17 | ![](https://img.shields.io/badge/%2300A9A9-00A9A9?style=flat-square) | 【京港】北京京港地铁有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |
| 19号线 | 19 | ![](https://img.shields.io/badge/%23D6ABC1-D6ABC1?style=flat-square) | 【运管】北京市轨道交通运营管理有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |
| S1线 | S1 | ![](https://img.shields.io/badge/%23B35A02-B35A02?style=flat-square) | 【运二】北京市地铁运营有限公司运营二分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2023-12-01 |
| 亦庄T1线 | T1 | ![](https://img.shields.io/badge/%23E5061B-E5061B?style=flat-square) | 【公交】北京公交有轨电车有限公司 | - | - |
| 亦庄线 | YZ | ![](https://img.shields.io/badge/%23E40077-E40077?style=flat-square) | 【运一】北京市地铁运营有限公司运营一分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2024-05-01 |
| 大兴机场线 | DA | ![](https://img.shields.io/badge/%23004A9F-004A9F?style=flat-square) | 【运管】北京市轨道交通运营管理有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |
| 房山线 | FS | ![](https://img.shields.io/badge/%23E46022-E46022?style=flat-square) | 【运二】北京市地铁运营有限公司运营二分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2023-12-01 |
| 昌平线 | CP | ![](https://img.shields.io/badge/%23DE82B2-DE82B2?style=flat-square) | 【运四】北京市地铁运营有限公司运营四分公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-20 |
| 燕房线 | YF | ![](https://img.shields.io/badge/%23E46022-E46022?style=flat-square) | 【运管】北京市轨道交通运营管理有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |
| 西郊线 | XJ | ![](https://img.shields.io/badge/%23E50619-E50619?style=flat-square) | 【公交】北京公交有轨电车有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |
| 首都机场线 | CA | ![](https://img.shields.io/badge/%23A29BBB-A29BBB?style=flat-square) | 【京城】北京京城地铁有限公司 | [Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools) | 2025-01-19 |

## 许可证

本项目采取[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.zh-hans)许可证。

*6号线*数据由本项目作者自行整理，其他数据来自[Beijing-Subway-Tools](https://github.com/Mick235711/Beijing-Subway-Tools)并进行了微调。感谢[@Mick235711](https://github.com/Mick235711)整理数据并提供适用于本项目格式的接口。
