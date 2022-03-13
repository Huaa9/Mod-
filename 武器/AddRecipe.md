## 添加合成表

# 在类名之下设置

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

## 合成表组

## 只可用于类名mod

    public class Torch : Mod {
        public override void AddRecipeGroups()
        {
            RecipeGroup group2 = new RecipeGroup(() => "任意" + "火把",
                new int[]
                {
                ItemID.Torch,
                ItemID.BlueTorch,
                });
            //这个东西第一个参数是名字，第二个参数是上面的合成组
            Group2.RegisterGroup("MOD:Torch", group);
        }
    }
    //应用这个recipe2。参数一为名字，参数二为数量。
     recipe.AddRecipeGroup("MOD:Torch", 10);
