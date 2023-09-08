# 按键绑定（针对 PC 端）及预设 UI

**阅读本文大概需要 10 分钟**

本文概述了如何使用按键绑定和鼠标锁定功能快捷设置 PC 游戏端的控制方案，以及新建项目的预设UI包括哪些功能。

## 什么是按键绑定？

**按键绑定**是指开发者在制作游戏时，可以在移动端 UI 基础上完成按键绑定，高效快捷的设置 PC 游戏端玩家的控制方案，而不需要重新设计一套 PC 端 UI，玩家在游戏中键盘鼠标操作将会映射到对应的移动端 UI 上；

**预设 UI**是指编辑器为新建项目提供的一套可修改的移动端预设 UI，及对应的 PC 端按键绑定。

## UI 编辑器内的【按键绑定】菜单

- 双击工程内容中的任意 UI 文件，点击 UI 编辑器工具栏的【按键绑定】按钮后，打开【按键绑定】菜单，开发者可以在此菜单直接将移动端 UI（包括摇杆、摄像机滑动区、按钮）绑定或覆盖到键鼠键位上
- 每个 UI 文件的按键绑定菜单会自动读取该 UI 文件内所有的摇杆、摄像机滑动区、按钮、按钮（废弃）这四类 UI 控件

![](https://wstatic-a1.233leyuan.com/productdocs/static/boxcnuSXwPA9zT2bIOvFYbhS1kc.png)

- 对于摄像机滑动区、按钮、按钮（废弃）：点击/释放/按住绑定的键鼠按键就会触发点击/释放/按住对应 UI 控件
- 对于摇杆：支持绑定摇杆的上下左右四个方向和仅按下/抬起，

  - 上下左右四个方向：分别对应摇杆输入值为（0,0.8）、（0,-0.8）、（-0.8,0）和（0.8,0）四种效果
  - 仅按下/抬起：按下绑定的键鼠按键会触发摇杆的按下抬起事件，但摇杆的当前值保持在（0,0）不变
  - 使用场景：为了实现一边开火一边转动摄像机来瞄准，射击游戏中的开火键推荐使用摇杆控件；而如果需要为 PC 端提供控制方案，可以将开火摇杆的仅按下/抬起绑定在鼠标左键，这样，玩家可以按住鼠标右键拖动或者直接拖动鼠标（鼠标锁定时）来转动摄像机进行瞄准，并使用左键触发摇杆完成开火

![](https://wstatic-a1.233leyuan.com/productdocs/static/boxcnhmdpZQxxnjfkGIrqSvmxfe.gif)

- 点击菜单中的按钮可以更换绑定，点击重置按钮可以恢复到默认绑定键位

## 键鼠绑定相关 API

- 除了 UI 编辑器内的按键绑定菜单，开发者也可以使用脚本自定义控制方案，或者覆盖默认键位

**脚本示例：**

```ts
const JumpBtn = this.uiWidgetBase.findChildByPath('Canvas/Button_Jump') as Button
//绑定按键
InputUtil.bindButton(Keys.Up,JumpBtn)
//解绑按键
InputUtil.unbindButton(Keys.Up)
```

- 为了满足开发者针对 PC 游戏端的不同需求，比如希望某个摄像机滑动区/摇杆是否可以被鼠标点击，因此在 VirtualJoystickPanel 和 TouchPad 类中提供了是否被鼠标控制的接口，编辑器内也可以在摄像机滑动区/摇杆属性面板中勾选此属性

![](https://wstatic-a1.233leyuan.com/productdocs/static/boxcnGgX88e45wIi9fPbNj6s2Sc.png)

- 此功能的使用场景请见 [UI 控件-摇杆](https://docs.ark.online/UI/UIWidget-Joystick.html) 和 [UI 控件-摄像机滑动区](https://docs.ark.online/UI/UIWidget-Touchpad.html)

**脚本示例：**

```ts
let touchpad=this.uiWidgetBase.findChildByPath('Canvas/UITouchPad_1') as TouchPad
let joystick=this.uiWidgetBase.findChildByPath('Canvas/UIVirtualJoystickPanel_1') as VirtualJoystickPanel
//设置摇杆和摄像机滑动区可以被鼠标点击
touchpad.controlByMouseEnable(true)
joystick.controlByMouseEnable(true)
//获取摇杆和摄像机滑动区是否可以被鼠标点击
let bool1 = touchpad.controlByMouseEnable
let bool2 = joystick.controlByMouseEnable
```

## 新建项目的预设 UI 控制方案

- 为新建项目提供一个预设UI文件，并且绑定了一套键鼠默认键位，作为默认的控制方案
- 预设UI文件包括左侧的摇杆、右侧的摄像机滑动区和右下的两个按钮（跳跃/攻击）
- 预设UI脚本文件内包括控制点击跳跃按钮可以实现跳跃、点击攻击按钮播放动作的逻辑
- 请注意：尽管在新建项目中，鼠标左右键都能控制摄像机，但两者的逻辑不同，鼠标左键控制摄像机类似于移动端用手指在摄像机滑动区内拖动来控制摄像机，而鼠标右键是通过按键绑定菜单里绑定到摄像机滑动区来控制摄像机的，如果不希望鼠标右键控制摄像机，可以在菜单里解除绑定

![](https://cdn.233xyx.com/online/S6R3KZWP1EE31694155920054.png)

| 按键               | 行动             | 对应的预设UI控件        |
| -------------------- | ------------------ | ------------------------- |
| **W**        | 向前移动         | VirtualJoystickPanel |
| **S**        | 向后移动         |
| **A**        | 向左移动         |
| **D**        | 向右移动         |
| **鼠标右键** | 旋转镜头（按住） | TouchPadDesigner |
| **空格键**   | 跳跃             | Button_Jump |
|              | 攻击/热武器开火  | Button_Attack |

## PC 端鼠标锁定功能及 API

- PC 游戏端允许使用鼠标锁定功能，玩家按 Shift 可以切换鼠标锁定，鼠标锁定情况下隐藏鼠标，转动鼠标直接旋转镜头和人物朝向，这样可以同时解放左键和右键留给其他角色行动，比如实现 PC 射击游戏常见的右键瞄准 + 左键射击
![](https://cdn.233xyx.com/1681457046992_545.gif)
- 开发者可以在脚本中控制是否允许玩家切换鼠标锁定状态

**脚本示例：**

```ts
//设置为不允许玩家切换鼠标锁定状态
InputUtil.setMouseLockable=false
//设置为允许玩家切换鼠标锁定状态
InputUtil.setMouseLockable=true;
```
