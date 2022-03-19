# 1.Potion基础属性设置`set Default()`
```C#
// 物品的使用方式，还记得2是什么吗
item.useStyle = 2;
// 喝药的声音
item.UseSound = SoundID.Item3;
// 决定这个物品使用以后会不会减少，true就是使用后物品会少一个，默认为false
item.consumable = true;
// 决定使用动画出现后，玩家转身会不会影响动画的方向，true就是会，默认为false
item.useTurn = true;
// 告诉TR内部系统，这个物品是一个生命药水物品，用于TR系统的特殊目的（比如一键喝药水），默认为false
item.potion = true;
// 这个药水能给玩家加多少血，跟potion一起使用喝完药就会有抗药性debuff
item.healLife = 50;
// 加buff的方法1：设置物品的buffType为buff的ID
// 这里我设置了中毒debuff（2333
item.buffType = BuffID.Poisoned;
// 用于在物品描述上显示buff持续时间
item.buffTime = 60000;
```

## 1.1BuffID
[BuffID](https://terraria-zh.gamepedia.com/%E5%A2%9E%E7%9B%8A_ID "泰拉的BuffID") 
### 1.1.1想加多种Buff，使用重写函数`UseItem`
```C#
// 当物品使用的时候触发，返回值貌似是什么都不会有影响
public override bool UseItem(Player player) {
    // 给玩家加上中毒buff，持续 60000 / 60 = 1000秒
    // 第一个填buff的ID，第二个填持续时间
    player.AddBuff(BuffID.Poisoned, 60000);
    // 给玩家加上猛毒buff，持续 60000 / 60 = 1000秒 
    player.AddBuff(BuffID.Venom, 60000);
    return false;
}
//BuffTime单位是次，帧理解为N次/秒。泰拉默认1秒刷新60次（60帧）
```
感觉此函数`player.AddBuff`融合了`item.buffType`和`item.buffTime`
#### 1.1.1.1解释上方`player.AddBuff`函数
```c#
public void AddBuff(int type, int time1, bool quiet = true);
```
* type是buff的id
* buff的持续时间
* quiet之后做笔记

给NPC加Buff也用此函数。
## 1.2自创Buff
