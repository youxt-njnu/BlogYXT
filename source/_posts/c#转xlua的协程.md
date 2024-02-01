---
title: C#转xlua| 针对协程实现的改写coroutine
tags:
 - coroutine
 - unity
categories:
 - c#
 - xlua
comments: true
date: 2023-12-10 20:03:05
photos: https://4k.wpcoder.cn/wp-content/uploads/2022/11/1_1668700001-1600x900.png
cover: https://4k.wpcoder.cn/wp-content/uploads/2022/11/1_1668700001-1600x900.png
---
# 背景

需要在unity里调用Mapbox SDK for Unity，并将Mapbox里的官方C#脚本案例修改为Xlua形式，在平台上才能实现交互。

# 关键代码

xlua改写

```lua
local _waitSecond = nil; 
_waitSecond = 0.3; 
coroutine.yield(UnityEngine.WaitForSeconds(_waitSecond));
```

相当于c#里的下面三句话：

```csharp
WaitForSeconds _wait; 
_wait = new WaitForSeconds(.3f); 
yield return _wait;
```

# 实现案例

c#

```csharp
using UnityEngine;
 
public class ReloadMap : MonoBehaviour {
	Coroutine _reloadRoutine;
	WaitForSeconds _wait;
	void Awake() {
		_zoomSlider.onValueChanged.AddListener(Reload);
		_wait = new WaitForSeconds(.3f);
	}
	void Reload(float value)
	{
		if (_reloadRoutine != null)
		{
			StopCoroutine(_reloadRoutine);
			_reloadRoutine = null;
		}
		_reloadRoutine = StartCoroutine(ReloadAfterDelay((int)value));
	}

	IEnumerator ReloadAfterDelay(int zoom)
	{
		yield return _wait;
		_camera.transform.position = _cameraStartPos;
		_map.UpdateMap(_map.CenterLatitudeLongitude, zoom);
		_reloadRoutine = null;
	}
}
```

XLua

```lua
local util = require 'xlua.util'
local UnityEngine = CS.UnityEngine

local _waitSecond = nil;
function Awake() 
	_zoomSlider.onValueChanged:AddListener(Reload);
	_waitSecond = 0.3;
end

function Reload(value)
    if _reloadRoutine ~= nil then
        self:StopCoroutine(_reloadRoutine);
        _reloadRoutine = nil;
    end
    _reloadRoutine = ReloadAfterDelay(math.floor(value));
    self:StartCoroutine(_reloadRoutine);

end

function ReloadAfterDelay(zoom)
    return util.cs_generator(function()
        coroutine.yield(UnityEngine.WaitForSeconds(_waitSecond));
        _camera.transform.position = _cameraStartPos;
        _map:UpdateMap(_map.CenterLatitudeLongitude, zoom);
        _reloadRoutine = nil;
    end)
end
```
