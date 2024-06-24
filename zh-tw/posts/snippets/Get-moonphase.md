---
isOriginal: true
category:
  - 程式碼片段
tag:
  - 伺服器腳本｜server side
---

# 獲取當前月相

```js
const moonPhaseList = ["滿", "虧凸", "下弦", "殘", "新", "峨嵋", "滿", "滿"];

LevelEvents.tick("overworld", (event) => {
  const { moonPhase, dayTime, server } = event;
  
  if (dayTime % 24000 === 1) {
    server.tell(`今天的月像是${moonPhaseList[moonPhase]}月`);
  }
});
```