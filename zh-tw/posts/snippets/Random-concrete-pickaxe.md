---
isOriginal: true
category:
  - 程式碼片段
tag:
  - 伺服器腳本｜server side
  - 啟動腳本｜startup side
---

# 隨機混凝土鎬

## 啟動腳本

```js
StartupEvents.registry("item", (event) => {
  event.create("random_concrete_pickaxe", "pickaxe").texture(":item/iron_pickaxe").maxDamage(250);
});
```

## 伺服器腳本

```js
BlockEvents.broken((event) => {
  const { entity, block } = event;

  if (entity.mainHandItem.id === "kubejs:random_concrete_pickaxe" && block.hasTag("forge:stone")) {
    let color = Object.keys(Color.DYE)[Utils.random.nextInt(16)];
    block.popItem(`${color}_concrete`);
    block.set("air");
    event.cancel();
  }
});
```