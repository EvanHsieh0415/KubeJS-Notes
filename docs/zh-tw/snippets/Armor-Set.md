---
isOriginal: true
category:
  - 程式碼片段
tag:
  - 伺服器腳本
  - 客戶端腳本
  - 套裝效果
  - 物品提示
---

# 套裝效果 + Tooltip提示

使用 NBT 達成 Tooltip 提示

## 伺服器腳本

```js
global.armorSets = {
  netherite: "Netherite Armor Set",
};

PlayerEvents.inventoryChanged((event) => {
  const { item, player, slot } = event;

  if (item.hasTag("forge:armors") && !item?.nbt?.armor_set) {
    item.setNbt({ armor_set: "" });
  }

  for (let key of Object.keys(global.armorSets)) {
    let setName = player.armorSlots.every((armor) => Utils.id(armor.id).path.startsWith(key))
      ? key
      : "";

    player.armorSlots.forEach((eachArmor) => {
      if (!eachArmor.empty) eachArmor.setNbt({ armor_set: setName });
    });
    player.inventory.getStackInSlot(slot).setNbt({ armor_set: setName });
    player.persistentData.putString("armor_set", setName);
  }
});
```

## 客戶端腳本

```js
ItemEvents.tooltip((event) => {
  event.addAdvancedToAll((item, isAdvanced, tooltip) => {
    if (item.hasTag("forge:armors") && item?.nbt?.armor_set !== undefined) {
      /** @type {{ armor_set: string }} */
      const { armor_set } = item.nbt;

      if (global.armorSets[armor_set] !== undefined) {
        tooltip.add(1, ["Active Armor Set: ", Text.green(global.armorSets[armor_set])]);
      } else {
        tooltip.add(1, ["Active Armor Set: ", Text.red("No Active")]);
      }
    }
  });
});
```