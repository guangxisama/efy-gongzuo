[README.md](https://github.com/user-attachments/files/34299574/README.md)
# DJI Osmo Action 4 · 3D 结构拆解实验室

一个使用 **Three.js** 程序化构建的 DJI Osmo Action 4 交互式三维拆解页面。整机、镜头、传感器、主板、电池、屏幕等全部由代码实时生成，不加载任何外部 3D 模型文件。

![初始装配视图](assets/preview/initial.jpg)

![爆炸拆解视图](assets/preview/exploded.jpg)

## 快速开始

**直接双击 `index.html` 即可运行。** 页面已内置 Three.js r147 与后期处理插件，离线可用，不需要 npm、构建工具或本地服务器。

也可以启动一个本地静态服务器：

```bash
python -m http.server 8000
# 打开 http://127.0.0.1:8000/
```

推荐使用最新版 Chrome / Edge / Safari，并启用硬件加速。

## 交互说明

| 操作 | 效果 |
| --- | --- |
| 鼠标拖拽 / 单指滑动 | 自由环绕旋转视角 |
| 滚轮 / 双指捏合 | 缩放镜头，查看部件细节 |
| 点击模型部件 | 沿预设装配轴拆解该部件并高亮，打开参数详情 |
| 再次点击该部件 | 收回部件 |
| 点击右侧部件清单 | 拆解对应模块；内部模块会自动展开中框以露出结构 |
| 点击标注卡片 | 选择并展开对应部件 |
| 底部「全部拆解」 | 展开 / 收回全部 10 个模块，进入爆炸视图 |
| 「自动旋转」 | 开启 / 暂停环绕动画 |
| 「部件标注」 | 显示 / 隐藏三维标注 |
| 「复位」 | 全部装配、重置视角与选择状态 |
| 左侧视角栏 | 等轴 / 正面 / 顶部 / 背面视角 |
| `Space` | 全部拆解 / 装配 |
| `R` | 复位 |
| `A` | 自动旋转 |
| `L` | 部件标注 |
| `1` – `9`、`0` | 快速选择前十项部件 |
| `?` | 打开操作引导 |
| `Esc` | 关闭帮助 / 取消选择 |

## 功能

- **10 个可交互模块**：超广角镜头、1/1.3" CMOS、影像主控板、1770mAh 电池、石墨烯散热模组、2.25" 后置触控屏、一体式中框 / 前壳、顶部按键与麦克风、磁吸快拆底座、USB-C / microSD 侧盖。
- **分层拆解动画**：每个部件拥有独立的分离向量、运动速度与阻尼，中框在展开时自动半透明化，便于观察内部堆叠。
- **屏幕翻转检视**：后置触控屏拆解时会绕自身中轴旋转 180°，把显示面转向观察者。
- **三维标注系统**：DOM/SVG 标签与 WebGL 场景实时投影同步，显示部件名称、关键参数与引导线。
- **实时高亮**：悬停与选择状态使用边缘发光、材质微自发光、相机对齐 HUD 环和 Bloom 辉光共同表达。
- **完整参数面板**：部件索引、分类、详细说明、四项核心规格、材料 / 工艺与快速定位。
- **程序化细节**：滚花镜筒、螺丝、散热铜管、主板芯片与走线、电池包装、USB-C 舌片、磁吸触点、麦克风开孔等均以代码生成。
- **响应式布局**：桌面端右侧参数栏，窄屏 / 手机端自动切换为底部抽屉与横向部件清单。
- **无障碍与辅助**：键盘快捷键、ARIA 标签、加载进度和错误兜底提示。

## 技术实现

- **Three.js r147（本地 UMD 版本）**：`lib/three.min.js` + `OrbitControls` + `EffectComposer` / `RenderPass` / `UnrealBloomPass`。
- **全程序化建模**：使用 `ExtrudeGeometry` 制作圆角机身与中框，`CylinderGeometry` / `TorusGeometry` / `TubeGeometry` / `SphereGeometry` 构成镜头、铜管和细节，`CanvasTexture` 生成前后屏 UI、电池标签、PCB 走线与传感器表面。
- **PBR 渲染**：`MeshPhysicalMaterial`、金属度 / 粗糙度、Clearcoat、基于 PMREM 的程序化环境贴图、ACES Filmic 色调映射与软阴影。
- **后处理**：UnrealBloom 低阈值辉光 + 半透明叠加层 + 屏幕空间标注。
- **物理感相机**：OrbitControls 阻尼、自动环绕、带缓动的预设机位与部件定位。
- **零外部资源**：无字体、贴图、HDR、GLTF/GLB 或远程请求；运行时网络完全可选。

## 项目结构

```text
.
├── index.html
├── assets/
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── app.js          # 场景、程序化模型、动画与交互
│   └── preview/            # README 预览图
├── lib/                    # 本地 Three.js 与后处理插件
│   ├── three.min.js
│   ├── OrbitControls.js
│   ├── EffectComposer.js
│   ├── RenderPass.js
│   ├── UnrealBloomPass.js
│   ├── Pass.js
│   ├── ShaderPass.js
│   ├── CopyShader.js
│   ├── LuminosityHighPassShader.js
│   └── THREE-LICENSE.txt
└── README.md
```

## 说明

这是一个面向展示与交互体验的概念化数字孪生拆解作品。尺寸、镜头规格、电池容量、屏幕参数等来自公开产品信息；内部堆叠、PCB 布局、材质与部分结构细节为程序化艺术还原，不代表 DJI 官方 CAD 或维修图纸。

DJI、Osmo Action 是 DJI 的商标。本项目为非官方、非商业的技术演示。
