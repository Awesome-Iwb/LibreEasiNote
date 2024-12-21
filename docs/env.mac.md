### 1. MacOS 上面开启EasiNote的调试环境：

> dist/window/base.js <br>
> `WindowBase.prototype.initWindow`

```javascript
window.webContents.openDevTools({
    mode: 'detach',
});
```

### 2. MacOS 上面让EasiNote允许主进程调试

```bash
/Applications/希沃白板.app/Contents/MacOS/希沃白板 --inspect=9229
```

