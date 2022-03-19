# 1.新建一个物品Potion
和之前Gun等基本结构类似
## 1.1Potion基础属性设置`set Default()`
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
## 1.2药水想增加多种Buff，使用重写函数`UseItem`
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

### 1.2.1解释上方`player.AddBuff`函数
```c#
public void AddBuff(int type, int time1, bool quiet = true);
```
* type是buff的id
* buff的持续时间
* quiet之后做笔记

## 2.1BuffID
[BuffID](https://terraria-zh.gamepedia.com/%E5%A2%9E%E7%9B%8A_ID "泰拉的BuffID") 

给NPC加Buff也用此函数。
## 2.2自创Buff
1. 新建一个叫Buffs的文件夹（命名空间叫namespace TemplateMod2.Buffs）
2. 把贴图拖进文件夹。新建.cs文件，类名和贴图名一样
3. ModBuff特有的三个重写函数`SetDefaults()`，`Update(Player player`,` ref int buffIndex)`，`Update(NPC npc, ref int buffIndex)`
4. 对方程进行细化。

```C#
using TemplateMod2.Utils;
using Terraria;
using Terraria.ModLoader;

namespace TemplateMod2.Buffs
{
    class GreenLight : ModBuff
    {   public override void SetDefaults() {
            DisplayName.SetDefault("川乌射罔");
            Description.SetDefault("接受药宗的制裁！");
            // 因为buff严格意义上不是一个TR里面自定义的数据类型，所以没有像buff.XXXX这样的设置属性方式了
            // 我们需要用另外一种方式设置属性
            // 这个属性决定buff在游戏退出再进来后会不会仍然持续，true就是不会，false就是会
            Main.buffNoSave[Type] = true;
            // 用来判定这个buff算不算一个debuff，如果设置为true会得到TR里对于debuff的限制，比如无法取消
            Main.debuff[Type] = false;
            // 当然你也可以用这个属性让这个buff即使不是debuff也不能取消，设为false就是不能取消了
            this.canBeCleared = false;
            // 决定这个buff是不是照明宠物的buff，以后讲宠物和召唤物的时候会用到的，现在先设为false
            Main.lightPet[Type] = false;
            // 决定这个buff会不会显示持续时间，false就是会显示，true就是不会显示，一般宠物buff都不会显示
            Main.buffNoTimeDisplay[Type] = false;
            // 决定这个buff在专家模式会不会持续时间加长，false是不会，true是会
            this.longerExpertDebuff = false;
            // 如果这个属性为true，pvp的时候就可以给对手加上这个debuff/buff
            Main.pvpBuff[Type] = true;
            // 决定这个buff是不是一个装饰性宠物，用来判定的，比如消除buff的时候不会消除它
            Main.vanityPet[Type] = false;

        }

        public override void Update(Player player, ref int buffIndex)
        {    // 把玩家的所有生命回复清除，否则可能会把debuff效果抵消掉
            if (player.lifeRegen > 0)
            {
                player.lifeRegen = 0;
            }
            player.lifeRegenTime = 0;
            // 让玩家的减血速率增加40
            //player.lifeRegen -= 40;
            player.buffTime[buffIndex] = 2;
            // 让玩家的减血速率随着时间而减少
            // player.buffTime[buffIndex]就是这个buff的剩余时间
            player.lifeRegen -= player.buffTime[buffIndex];

            
            // 使用这个物品=>触发Buff
            player.AddBuff(ModContent.BuffType<GreenLight>(), 60000);

        }

        // 如果返回true就代表buff重新添加成功
        public override bool ReApply(Player player, int time, int buffIndex)
        {
            // 这段代码的作用就是当玩家被重新添加buff的时候延长buff时间
            player.buffTime[buffIndex] += time;
            return true;
        }

        public override void Update(NPC npc, ref int buffIndex)
        {
            base.Update(npc, ref buffIndex);
        }


    }
}
```

[回到顶部](#1.新建一个物品Potion)
