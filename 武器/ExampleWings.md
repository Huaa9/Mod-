# 简单翅膀的程序

```C#
using Terraria.Localization;
using Terraria.ModLoader;
using Terraria;
using Terraria.ID;

namespace TemplateMod2.Items.Accessories
{   
    //Attribute TML读到这个类的时候就会知道这是个翅膀装备。因为翅膀必须有个4帧图，尾缀为Wings，比较特殊。
    [AutoloadEquip(EquipType.Wings)]
    public class ExampleWings : ModItem
    {
        public override void SetStaticDefaults()
        {
            DisplayName.SetDefault("exsample wing");
            DisplayName.AddTranslation(GameCulture.Chinese, "简单翅膀");

            Tooltip.SetDefault("here is a exsample wing\n" + "du kannst mit ihm überall hingehen.");
            Tooltip.AddTranslation(GameCulture.Chinese, "使用翅膀能去任何地方！");

            base.SetStaticDefaults();

        }

        public override void SetDefaults()
        {
            item.width = 30;
            item.height = 28;

            item.accessory = true;
            item.defense = 2;
            item.rare = 3;
            item.value = Item.sellPrice(0,1,1,0);
            item.expert = false;



            base.SetDefaults();
        }
//-------------------------------------------------------------------------
        //首饰的一些属性，伤害，速度
        public override void UpdateAccessory(Player player, bool hideVisual)
        {
            player.wingTimeMax = 180;
            // 让翅膀的wingTime在每一帧都是一个固定的值
            //player.wingTime = player.wingTimeMax; 无限飞行
            player.statLifeMax2 += 100;
            //可以滞空
            /*if (!player.controlJump && !player.controlDown)
            {
                player.gravDir = 0f;
                player.velocity.Y = 0;
                player.gravity = 0;
                player.noFallDmg = true;
            }
            if (player.controlDown)
            {
                player.gravity = Player.defaultGravity;
                player.gravDir = 1;
                player.noFallDmg = true;
            }*/

            base.UpdateAccessory(player, hideVisual);
        }
        public override void VerticalWingSpeeds(Player player, ref float ascentWhenFalling, ref float ascentWhenRising, ref float maxCanAscendMultiplier, ref float maxAscentMultiplier, ref float constantAscend)
        {
            ascentWhenFalling = 5f;
            ascentWhenRising = 20f;
            maxAscentMultiplier = 5f;
            base.VerticalWingSpeeds(player, ref ascentWhenFalling, ref ascentWhenRising, ref maxCanAscendMultiplier, ref maxAscentMultiplier, ref constantAscend);
        }
        public override void HorizontalWingSpeeds(Player player, ref float speed, ref float acceleration)
        {
            speed = 8.5f;
            acceleration = 3;
            base.HorizontalWingSpeeds(player, ref speed, ref acceleration);
        }
//-------------------------------------------------------------------------
        public override void AddRecipes()
        {
            ModRecipe Examplewing1 = new ModRecipe(mod);
            Examplewing1.AddIngredient(ItemID.MudBlock, 1);
            Examplewing1.AddTile(TileID.WorkBenches);
            Examplewing1.SetResult(this);
            Examplewing1.AddRecipe();
            


            base.AddRecipes();
        }

    }
```
