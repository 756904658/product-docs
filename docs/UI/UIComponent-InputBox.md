# UI 控件-输入框

**阅读本文大概需要 10 分钟**

本文概述了 UI 控件—输入框的各项属性以及使用方法。

## 什么是输入框？

**输入框**是输入文字或文本信息的 UI 控件，可以获取玩家输入内容并在游戏中动态修改相应控件内的文字信息。

- 变换/对齐/通用/渲染属性请见 [UI 控件的基础属性](https://docs.ark.online/UI/UIWidget-BaseProperties.html)

## 输入框属性-文本

- 字体大小、字体间距、水平显示请见 [UI 控件-文本](https://docs.ark.online/UI/UIComponent-Text.html) 。

### 输入限制

- 限制输入文字的字符数量，超过输入限制，则不会显示超出部分字符。
- 举例说明：输入游戏角色名称一般不超过 8 个字符。

### 默认文本

- 即占位文本，输入框内容为空时，默认显示的文本内容，显示在输入框控件内，且字体颜色为淡灰色。在用户输入文本时，该默认文本将会消失。

### 文本

- 输入相应文本内容，显示在输入框控件内。

### 只读

- 该文本输入框是否可以更改，勾选则设置为只读无法更改


## 输入框属性-样式

- 字体类属性请见 [UI 控件-文本](https://docs.ark.online/UI/UIComponent-Text.html)。

### 内容颜色

- 修改输入底框的颜色。

## 输入框属性-内容限制

### 内容限制

- 无限制

  - 没有任何输入限制
- 整数限制

  - 只能输入整数的限制
- 小数限制

  - 支持输入小数的限制
- 支持数字和字符

  - 支持输入字符和数字，但是不能输入中文
- 密码限制

  - 支持输入字符和数字，但输入后，显示为*符号。

## 如何使用输入框？

- 举例说明：个性签名、聊天输入、商店输入数量购买等都会需要用到输入框控件。

### 示例一：制作兑换码界面

- 首先我们需要制作一个面板，然后将输入框等 UI 控件放置在面板上，如图。

![](https://qn-cdn.233leyuan.com/online/4zdEqpqQHMXU1724051250930.png)

- 然后将 UI 与脚本进行绑定。随后编写脚本。
- 示例脚本：

```ts
export default class NewUIScript extends UIScript {

    isExchange:Boolean;
    //创建一个兑换成功或失败提示的方法：判断条件为是否兑换成功？
    Tips (tipsText: TextBlock) {
        //兑换成功，则显示兑换成功，兑换失败，则显示兑换码错误
        this.isExchange? tipsText.text="兑换成功" : tipsText.text="兑换码错误" ;
        //兑换成功，则文字显示为绿色，兑换失败，则文字显示为红色
        this.isExchange? tipsText.setFontColorByHex("00EA3FFF") : tipsText.setFontColorByHex("EA2400FF")
        //提示文字显示2秒后消失
        setTimeout(() => {
            tipsText.visibility=2;
        }, 2000);
        tipsText.visibility=0;
    }

    /** 
     * 构造UI文件成功后，在合适的时机最先初始化一次 
     */
    protected onStart() {
        //设置能否每帧触发onUpdate
        this.canUpdate = false;
         //找到对应的文本组件
        let text = this.uiWidgetBase.findChildByPath('RootCanvas/Canvas/InputBox') as InputBox
        let confirmBtn = this.uiWidgetBase.findChildByPath('RootCanvas/Canvas/Button') as Button
        let tipsText = this.uiWidgetBase.findChildByPath('RootCanvas/Canvas/TextBlock_1') as TextBlock

        //按下确认按钮后，检测验证码是否正确
        confirmBtn.onPressed.add(() => {
            if (text.text == "111111") {
                //兑换成功
                this.isExchange = true
            }
            else{
                //兑换失败
                this.isExchange = false
            }
            //执行兑换成功或失败提示的方法
            this.Tips (tipsText)
        });
    }
}
```

- 示意图：

![](https://qn-cdn.233leyuan.com/online/45CQ0oQQ52VS1724051249966.gif)

