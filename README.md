# “决胜单”落地页（核心转化区域6）

这是一个基于策划方案开发的落地页子模块，已实现区域6：核心转化区的所有关键功能，包括阶梯定价、倒计时、模拟稀缺名额和支付弹窗，页面具备良好的响应式体验。

## 已完成功能

1. 实现三段式阶梯定价的静态展示。
2. 动态倒计时器（以页面加载时间为起点，倒计72小时）。
3. 模拟“剩余名额”前端随机减少。
4. “立即抢占”按钮点击后弹出支付二维码模态框。
5. 响应式页面设计，适配桌面、平板和手机设备。

## 技术栈
- 框架：Vite + Vue
- 样式：SCSS/SASS
- 版本控制：Git

## 目录结构
<img width="232" height="577" alt="image" src="https://github.com/user-attachments/assets/3709219a-a597-4454-a20a-8fad6ed149c2" />


## 运行截图
1.	在各个设备运行时：
 <img width="433" height="268" alt="image" src="https://github.com/user-attachments/assets/2d4d81e4-7261-46a2-8e1c-96644d03011a" />

（pc端）
    
<img width="455" height="605" alt="image" src="https://github.com/user-attachments/assets/874f1d2b-5c51-442a-b845-99a53a904abd" />
<img width="292" height="629" alt="image" src="https://github.com/user-attachments/assets/b7bc3792-734e-4244-8e2a-a4dec135c3b4" />

（左ipad端，右手机端）

2.	点击立即抢占，弹出一个模态框，展示了展位图
 <img width="455" height="355" alt="image" src="https://github.com/user-attachments/assets/6c294fea-6a41-4792-8448-c1230de2c8b8" />

（占位图）

## AI工具辅助提效
1. 倒计时逻辑异常调试
   我在实现倒计时器功能时，希望固定一个 72 小时后的时间点并持续显示“时:分:秒”格式的剩余时间。但初版代码中，当页面刷新后时间重置不准确，且在某些情况下倒计时变负数。
   ai提供的优化建议：
   - 使用固定的 Date.now() + 72 * 3600 * 1000 作为结束时间；
   - 倒计时更新中加入判断：如果当前时间 > 截止时间，强制归零；
   - 推荐将逻辑封装为函数，并在 onMounted() 中启动 setInterval()。
  效果：根据 AI 建议修复了逻辑判断并优化了用户体验，避免倒计时闪烁或负数。

2. SCSS响应式写法
   我在设计“核心转化区”卡片布局时，桌面视图使用 flex 横排，但在小屏设备（如手机）下卡片容易挤在一行显示，影响体验。
   ai提供的优化建议：
   ```css
    @media (max-width: 768px) {
  .card-wrapper {
    flex-direction: column;
    align-items: center;
  }

  .cta-button {
    width: 100%;
  }
}
   ```
   效果：快速解决了手机端样式错乱问题，提升了移动端用户体验。后续将该写法拓展到其他区域，如 FAQ、Header 模块。

  3. 模态框弹窗关闭体验不佳
  初版模态框只能通过点击关闭按钮 (✕) 关闭，用户点击黑色背景无反应，体验不佳。
  ai提供的优化建议：
  - 为背景容器添加 @click.self="closeModal"；
  - 使用 .stop 修饰符避免冒泡到子元素。
  效果：交互更贴合用户预期，支持点击背景关闭，模态框使用体验提升。

## 本地运行
```bash
# 安装依赖
npm install

# 本地开发预览
npm run dev
```

