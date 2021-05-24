[toc]
# 问题详情
解决输入法的ime模式情况下，antd的inputNumber组件设置了formatter添加千分符时额外触发一次onChange事件导致的重复录入问题。举个例子，当输入1、2、3后，组件显示123，e.nativeEvent.inputType均是insertCompositionText；当录入4时，触发两次事件e.nativeEvent.target.value分别是1,234、12,344,e.nativeEvent.inputType分别是insertCompositionText和insertText
【问题原因】：
系统自带输入法的中文模式下输入内容后产生逗号（千分符）时，额外触发一次输入事件，导致重复录入问题
【复现必要条件】：
1.输入法：系统自带输入法（搜狗输入法的中文模式就没问题）
2.模式：中文输入模式（英文模式下没问题）
3.浏览器：Chrome（firfox火狐浏览器没问题，其他浏览器不清楚）
【解决方案】
分3个方案：
1.设置css的ime-mode: 'disabled',用来禁用ime-mode，已经从css标准废弃，对Chrome无效，对其他浏览器可能有效
2.1设置input属性type='tel'，针对Chrome做的处理，在ime模式（中文）下，光标键入后会自动切换为非ime模式（英文），但用户还能够切换回ime模式。故此方案不完善
2.2设置input属性inputmode，主要是针对手机端键盘输入模式
3.修改antd的rc-input-number库代码，在onChange事件处理时做条件判定，仅当ime模式下的输入的二次触发事件进行截获并抛弃
尽管有可能仅靠方案3即可解决问题，但由于不能确保逻辑完备，故先由方案1,2来尽量避免ime模式，方案3来处理ime模式下的逻辑

# 其他可能方案
- 可结合事件[compositionstart](https://w3c.github.io/uievents/#event-type-compositionstart)和[compositionend](https://w3c.github.io/uievents/#event-type-compositionend)来分别设置状态来对onChange事件坐相应的处理

# 参考资料
- [compositionstart]([compositionstart](https://developer.mozilla.org/en-US/docs/Web/API/Element/compositionstart_event))
- [Input method editor](https://developer.mozilla.org/en-US/docs/Glossary/Input_method_editor)
- [w3c标准中关于IME composition事件的定义](https://www.w3.org/TR/input-events-2/#event-order-during-ime-composition)
- [ime-mode](https://developer.mozilla.org/en-US/docs/Web/CSS/ime-mode)
- [inputmode](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/inputmode)
- [Everything You Ever Wanted to Know About inputmode](https://css-tricks.com/everything-you-ever-wanted-to-know-about-inputmode/)
- 