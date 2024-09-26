---
isOriginal: true
author:
  - name: 芒果凍布丁
  - name: Prunoideae
    url: https://github.com/Prunoideae
category:
  - 程式碼片段
tag:
  - 伺服器腳本
  - 程式碼範例
  - KubeJS Create
  - 動力機械
star: true
---

# KubeJS Create 所提供的 `CreateEvents`

## 蒸氣鍋爐熱量源 - `boilerHeatHandler`

```js
CreateEvents.boilerHeatHandler((event) => {
  // 熱量等級說明：
  // -1：此方塊不提供任何形式的熱量。
  // 0 ：此方塊提供被動熱量。（例如未加燃料的烈焰燃燒器）
  // 1 ：此方塊提供1單位的熱量（在狀態欄中顯示1個綠色條，例如加燃料的烈焰燃燒器）
  // 2 ：此方塊提供2單位的熱量（例如烈焰蛋糕燃燒器）
  // X ：此方塊提供X單位的熱量，狀態欄中顯示X個綠色條
  //
  // 注意，回調函數僅在鍋爐有更新信號時調用。
  // 例如相鄰方塊的狀態變化、破壞或放置等。

  // 添加單個方塊的加熱器。
  // 回調函數的參數類型為 BlockContainerJS。
  event.add("minecraft:diamond_block", (block) => {
    return 2;
  });

  // 添加方塊篩選器的加熱器。
  event.addAdvanced("#minecraft:logs", (block) => {
    if (block.id === "minecraft:oak_log") {
      return 1;
    } else {
      return 2;
    }
  });
});
```

## 管道流體效果 - `pipeFluidEffect`

```js
CreateEvents.pipeFluidEffect((event) => {
  // 添加液體處理器，支援 FluidIngredient。

  event.add(
    Fluid.water(), // 流體
    (pipe, fluid) => {
      const { world } = pipe;
      if (world.server.tickCount % 5 != 0) return;

      world.getEntitiesWithin(pipe.AOE).forEach((entity) => {
        if (entity.living) {
          entity.heal(5);
        }
      });
    }
  );
});
```

## 注液器注液方塊 - `spoutHandler`

```js
CreateEvents.spoutHandler((event) => {
  // 創建注液器處理器，需要提供 ID，因為這裡沒有辦法生成一個一致的 UUID。
  //
  // 注液器每個 tick 都會以 simulate = true 的方式調用處理器，如果返回值 > 0，則注液器將開始注液動畫，
  // 動畫結束時，處理器將再次以 simulate = false 的方式調用。
  //
  // 返回的整數表示此操作應該消耗多少單位液體。
  // 單位視模組載入器而不同，Forge = 1MB、Fabric = 1 unit
  // 1 B（Bucket，桶） = 1000 MB（MiliBucket，千分之一桶） = 81000 unit（單位流體）

  event.add(
    "kubejs:obsidian", // ID
    "minecraft:lava", // 目標方塊
    (block, fluid, simulate) => {
    if (fluid.id === Fluid.water().id && fluid.amount >= 100) {
      if (!simulate) {
        block.set("minecraft:obsidian");
      }
      return 100;
    }
    return 0;
  });
});
```