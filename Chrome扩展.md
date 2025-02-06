# Chrome扩展

## 唯一必须的文件是manifest.json

```js
// manifest.json中的manifest_version，name，version是必须的
// 修改manifest.json、Service Worker、content scripts后需要重新加载插件

// 示例
{
    "manifest_version": 3,
    "name": "tes",
    "description": "Base Level Extension",
    "version": "1.0",
    "action": {
        "default_popup": "index.html",
        "default_icon": "hello_extensions.png"
    },
    "background": {
        "service_worker": "background.js"
    },
    "commands": {
        "_execute_action": {
            "suggested_key": {
                "default": "Ctrl+B",
                "mac": "Command+B"
            }
        }
    }
}

// 默认的图标
"action": {
    "default_icon": "hello_extensions.png"
}

// 点击扩展图标后弹出的页面
"action": {
    "default_popup": "index.html"
}

// 图标
"icons": {
  "16": "images/icon-16.png",		// 用于扩展程序页面和上下文菜单上的网站图标
  "32": "images/icon-32.png",		// Windows计算机通常需要此大小
  "48": "images/icon-48.png",		// 显示在“扩展程序”页面上
  "128": "images/icon-128.png"		// 在安装和Chrome应用商店中显示
}

// 添加内容脚本，内容脚本会在打开网站后自动生效
// 通过matches来确定将内容脚本注入哪些网站，匹配模式由三部分组成： <scheme>://<host><path>。可以包含“*”字符
"content_scripts": [
  {
    "css": ["style.css"],
    "js": ["scripts/content.js"],
    "matches": ["http://*/*", "https://*/*", "file:///*"],
    "exclude_matches": [
      "https://plus.google.com/hangouts/*"
    ]
  }
]

// Service Worker会监控浏览器事件
"background": {
  "service_worker": "background.js"
}

// 使用"type": "module"使Service Worker可以导入模块
"background": {
  "service_worker": "service-worker.js",
  "type": "module"
}
    
// 点击扩展程序图标后，将图片改变
"action": {
  "default_icon": {
    "16": "images/icon-16.png",
    "32": "images/icon-32.png",
    "48": "images/icon-48.png",
    "128": "images/icon-128.png"
  }
}

//
"permissions": ["activeTab"]

// 使用chrome api中的scripting api需要添加scripting权限
"permissions": ["activeTab", "scripting"]

// 使用chrome api中的存储api需要添加scripting权限
"permissions": ["storage"]

// 添加扩展程序的快捷键
"commands": {
  "_execute_action": {
    "suggested_key": {
      "default": "Ctrl+B",
      "mac": "Command+B"
    }
  }
}

// 定义可以被网页访问的资源
"web_accessible_resources": [{
  "resources": ["inject.css", "shadow.css"],
  "matches": ["http://*/*", "https://*/*", "file:///*"]
}]
```

## 在service worker中运行的chrome api

```js
chrome.runtime.onInstalled.addListener(() => {
  chrome.action.setBadgeText({
    text: "OFF",
  });
});
```

## chrome api

```js
chrome.runtime.getURL("settings.html")		// 返回string

// 可以用于设置状态或完成一些安装任务
chrome.runtime.onInstalled.addListener(() => {})
// 当点击扩展程序图标时运行
chrome.action.onClicked.addListener(async (tab) => {})
// 更改布局
await chrome.scripting.insertCSS({
    files: ["focus-mode.css"],
    target: { tabId: tab.id },
});
await chrome.scripting.removeCSS({
    files: ["focus-mode.css"],
    target: { tabId: tab.id },
});

// 发送消息
const { tip } = await chrome.runtime.sendMessage({ greeting: 'tip' })
const { tip } = await chrome.runtime.sendMessage({ greeting: 'tip' }, ()=>{})
// 接收消息
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.greeting === 'tip') {
    chrome.storage.local.get('tip').then(sendResponse);
    return true;
  }
});

// 存储数据
chrome.storage.sync.set({speed: 2.0})
chrome.storage.sync.set({speed: 2.0}, function() {})
// 获取存储的speed值，如果不存在则返回默认值1.0
chrome.storage.sync.get({speed: 1.0}, function (storage) {console.log(storage.speed)})
// 移除数据
chrome.storage.sync.remove(['speed'])

// 设置icon
chrome.action.setIcon({
    path: {
        "19": "icons/icon19" + suffix,
        "38": "icons/icon38" + suffix,
        "48": "icons/icon48" + suffix
    }
});
```

