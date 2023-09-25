# 帮助文本

帮助文本是出现在游戏左上角的小框。

你不能改变这个文本的位置。

**Client Side**

```js
alt.onServer('showHelpText', showHelpText);

export function showHelpText(text, sound, milliseconds) {
  native.beginTextCommandDisplayHelp("STRING");
  native.addTextComponentSubstringPlayerName(text);
  native.endTextCommandDisplayHelp(0, false, sound, milliseconds);
}
```

## Example Usage

**Server Side**

```js
alt.on('playerConnect', player => {
    alt.emitClient(player, 'showHelpText', 'Press ~INPUT_MOVE_UP_ONLY~ to move forward.', true, 5000);
});
```

**Client Side**

```js
showHelpText('Press ~INPUT_MOVE_UP_ONLY~ to move forward.', true, 5000);
```
