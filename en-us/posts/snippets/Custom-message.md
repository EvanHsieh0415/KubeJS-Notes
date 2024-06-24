---
isOriginal: true
category:
  - Code Snippets
tag:
  - Serve Side
---

# Custom Message Display

```js
PlayerEvents.chat((event) => {
  let { player, message, server } = event;
  server.tell([Text.yellow(player.displayName), " ", Text.gray("»"), " ", Text.white(message)]);
  event.cancel();
});
```