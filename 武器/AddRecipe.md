# 添加合成表
教程来自于：[第一个远程武器-手枪](https://fs49.org/2020/03/09/%e7%ac%ac%e4%b8%80%e4%b8%aa%e8%bf%9c%e7%a8%8b%e6%ad%a6%e5%99%a8-%e6%89%8b%e6%9e%aa/ "裙中世界-第一部分")

## 在类名之下设置
```C#
public override void AddRecipes() {
        ModRecipe recipe1 = new ModRecipe(mod);
        // 合成材料，需要10个泥土块
        recipe.AddIngredient(ItemID.DirtBlock, 10);
        // 以及在工作台旁边，一定要是一个TileID
        recipe.AddTile(TileID.WorkBenches);
        // 生成1个这种物品
        recipe.SetResult(this);
        // 这样可以生成50个
        // recipe.SetResult(this, 50);

        // 把这个合成表装进tr的系统里
        recipe1.AddRecipe();
    }
```

## 合成表组

### 只可用于类名mod
```C#
using Terraria;
using Terraria.ID;
using Terraria.ModLoader;

// 命名空间，注意它要与文件夹的名字相同
namespace TemplateMod2 {

    // 主要Mod类
    public class TemplateMod2 : Mod
    {

        // 给一个实例指针，以后会非常有用的
        private static TemplateMod2 instance;

        // 构造函数
        public TemplateMod2()
        {
            instance = this;
        }

        public static TemplateMod2 Instance
        {
            get;
        }


        public override void AddRecipeGroups()
         {
             //() => Language.GetTextValue("LegacyMisc.37") + "火把",
             RecipeGroup PlatformGroup121 = new RecipeGroup(() => "一些平台",
                 new int[]
                 {
                 ItemID.LivingWoodPlatform,
                 ItemID.HoneyPlatform,
                 ItemID.MushroomPlatform,
                 });

             RecipeGroup.RegisterGroup("TemplateMod2:PlatformGroup121", PlatformGroup121);

             RecipeGroup Torchgroup = new RecipeGroup(() => "任意" + "火把",
                 new int[]
                 {
                 ItemID.Torch,
                 ItemID.BlueTorch,
                  });
             //登记合成组至Terraria（合成组可不是合成配方，到时候使用还是需要recipe1.AddRecipe();）：这个东西第一个参数是名字，第二个参数是上面的合成组
             RecipeGroup.RegisterGroup("TemplateMod2:Torchgroup", Torchgroup);

         }

    }

}
```
    
**直接在mod类下写addRecipeGroups方程，再把合成组的名字赋予类的名字。**  
使用合成组，
1. 第二行直接用类下的名字。切记**不要**使用`Mod:PlatformGroup121`，属于跨级要报错。
2. `AddRecipeGroup`其实就相当于一个**打包**的`AddIngredient`。意思就是说`AddTile，SetResult，AddRecipe`这些指令不要忘记写，不然会报错：超出索引边界。

```C#
public override void AddRecipes()
{
        ModRecipe Bstone3 = new ModRecipe(mod);
        Bstone3.AddRecipeGroup("TemplateMod2:PlatformGroup121", 2); //使用合成组，名字，需要数量
        Bstone3.AddTile(TileID.WorkBenches);
        Bstone3.SetResult(this);
        Bstone3.AddRecipe();
}
```

### 一些小**Tips**
```C#
        ModRecipe 合成表名字 = new ModRecipe(mod);
        合成表名字.AddIngredient(ItemID.Blabla,数量);
        合成表名字.AddTile(TileID.Blabla);    //添加合成环境
        合成表名字.SetResult(this);           //生成个数
        合成表名字.AddRecipe();               //加载这个合成表

        RecipeGroup.RegisterGroup("MOD:类名", 合成配方名字) //来加载这个合成表组，这个东西第一个参数是名字，第二个参数是上面的合成组
```

[回到顶部](#添加合成表)

