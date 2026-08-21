[简中](https://github.com/Anan-up/Small-fan/blob/main/README.md)·[文言](https://github.com/Anan-up/Small-fan/blob/main/README_Classical_Chinese.md)·[English](https://github.com/Anan-up/Small-fan/blob/main/README_English.md)

这是一个**纯前端单文件的趣味网页**——「夏日清凉小风扇」，属于那种自嘲式的小玩具项目。你可以直接点开玩。

## 项目概览

| 项目 | 内容 |
|------|------|
| 形态 | 单个 `index.html`（约 34KB），零依赖、零构建 |
| 技术 | 原生 HTML + CSS + JS，无任何框架 |
| 外部资源 | 无，两段音效（风扇嗡嗡声、按键咔哒声）以 base64 内嵌 |
| 功能 | 三档风速、摇头开关、风扇音效、深色模式 |

## 结构拆解

**1. 纯 CSS 画的风扇**
- 外圈 + 网罩（`radial-gradient` 同心圆 + `repeating-conic-gradient` 放射线）+ 中心盖，全是用伪元素和渐变画的，没有一张图片
- 三片扇叶用 `border-radius: 20% 50%` 捏出叶片形状，各旋转 120° 排布
- 加上脖子和底座，一台完整台扇

**2. 物理感动画**
- `requestAnimationFrame` 驱动，扇叶角速度按一阶惯性趋近目标值：**加速快（τ=0.9s）、减速慢（τ=2.2s）**——关机后扇叶会慢慢惯性停转，而不是瞬间停
- 三档速度：540°/s、720°/s、1350°/s
- 摇头：摆幅平滑渐变（±5°，周期 4.5s），关闭摇头时自然回正，不会"跳变"
- 切后台时用 `dt` 上限 0.1s 防止回来时角度突跳

**3. Web Audio 音效**
- 风扇声做成循环音源，换挡时**音量和音高（playbackRate）像真电机一样指数平滑过渡**（`setTargetAtTime`）
- 音频在首次点击时才初始化解码（绕开浏览器自动播放限制），失败则静默降级

**4. 细节考量**
- 跟随系统深色模式（CSS 变量切换黑白配色）
- 尊重 `prefers-reduced-motion`：用户开了"减弱动态效果"就自动不摇头
- 按钮有 `focus-visible` 焦点样式、按下下沉的拟物效果，支持触屏（去掉 tap 高亮）

