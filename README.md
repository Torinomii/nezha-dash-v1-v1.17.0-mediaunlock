[Demo](https://uptime.fcos.dev/)

![image](https://raw.githubusercontent.com/Torinomii/nezha-dash-v1-v1.17.0-mediaunlock/refs/heads/main/unlock.png)
根据nezha-dash-v1 v1.17.0版本修改


# 下载并修改 nezha-dash-v1 前端
找到探针的dashboard目录```/opt/nezha/dashboard/```
docker的话在```/dashboard/```
```
cd /opt/nezha/dashboard/
wget https://github.com/hamster1963/nezha-dash-v1/releases/download/v1.17.0/dist.zip
unzip dist.zip && mv dist/ user-dist/
```
现在目录看起来类似这样的
```
/opt/nezha/dashboard/user-dist
├── animated-man.webp
├── apple-touch-icon.png
├── assets
│   ├── ...
│   ├── ...
│   ├── index.eyIaazA7.js
│   ├── ...
└──  index.html
```
备份文件```cp user-dist/index.eyIaazA7.js user-dist/index.eyIaazA7.js.bak```

你可以直接下载我修改好的 ```[https://github.com/Torinomii/nezha-dash-v1-v1.17.0-mediaunlock/raw/refs/heads/main/index.eyIaazA7.js](https://github.com/Torinomii/nezha-dash-v1-v1.17.0-mediaunlock/raw/refs/heads/main/user-dist/assets/index.eyIaazA7.js)```

或者手动修改```vim user-dist/index.eyIaazA7.js```找到并修改下列变量新增解锁标签

```
    y4 = {
        Detail: "Detail",
        Network: "Network",
        Unlock: "Unlock"	//新增标签语言
    },
	
	a5 = {
        Detail: "详情",
        Network: "网络",
        Unlock: "解锁"	//新增标签语言
    },
	
	T5 = {
        detail: "詳細資訊",
        network: "網路",
        unlock: "解鎖"	//新增标签语言
    },
```
修改标签添加媒体解锁的输出，由于我把解锁信息格式化成markdown，这里读取markdown
```
function Y2() {
    const n = Z();
    r.useEffect(() => {
        window.scrollTo({
            top: 0,
            left: 0,
            behavior: "instant"
        })
    }, []);
    const l = ["Detail", "Network", "Unlock"], //新增标签
        [_, e] = r.useState(l[0]),
        {
            id: s
        } = l1();
	const [unlockData, setUnlockData] = r.useState(null);
    const [loading, setLoading] = r.useState(false);
    const [error, setError] = r.useState(null);
	
	    // 文件读取效果
    r.useEffect(() => {
        if (_ !== l[2] || !s) return;

        const controller = new AbortController();
        
        const fetchData = async () => {
    try {
        setLoading(true);
        const response = await fetch(`/unlockdata/${s}.md`, { //这里读取的是/opt/nezha/dashboard/user-dist/unlockdata/服务器id.md
            signal: controller.signal,
            headers: {
                // 'Content-Type': 'text/plain', // 通常 GET 请求不需要 Content-Type
                'Accept': 'text/markdown'       // 明确接受 markdown 类型
            } 
        });
        
        if (!response.ok) throw new Error(`文件读取失败: ${response.status}`);
        
        const markdownContent = await response.text(); 
        
        setUnlockData(markdownContent);
        setError(null);
        
    } catch (err) {
        if (err.name !== 'AbortError') {
            setError(err.message);
            setUnlockData(null);
        }
    } finally {
        setLoading(false);
    }
};

        fetchData();
        return () => controller.abort();
    }, [s, _]); // 当 serverId 或标签变化时重新加载
	
	
    return s ? a.jsxs("div", {
        className: "mx-auto w-full max-w-5xl px-0 flex flex-col gap-4 server-info",
        children: [a.jsx(Z2, {
            server_id: s
        }), a.jsxs("section", {
            className: "flex items-center my-2 w-full",
            children: [a.jsx($, {
                className: "flex-1"
            }), a.jsx("div", {
                className: "flex justify-center w-full max-w-[200px]",
                children: a.jsx(W2, {
                    tabs: l,
                    currentTab: _,
                    setCurrentTab: e
                })
            }), a.jsx($, {
                className: "flex-1"
            })]
        }), a.jsx("div", {
            style: {
                display: _ === l[0] ? "block" : "none"
            },
            children: a.jsx(U2, {
                server_id: s
            })
        }), a.jsx("div", {
            style: {
                display: _ === l[1] ? "block" : "none"
            },
            children: a.jsx(T2, {
                server_id: Number(s),
                show: _ === l[1]
            })
        }),//新增 Unlock 标签部分
		a.jsx("div", { 
			style: { 
				display: _ === l[2] ? "block" : "none",
			},
			children: a.jsxs("div", {
				className: unlockStyles().container,
				children: [
				loading && a.jsx("div", {
					className: unlockStyles().loading,
					children: "加载中..."
				}),
				error && a.jsx("div", {
					className: unlockStyles().error,
					children: `错误: ${error}`
				}),
				unlockData && a.jsx("pre", {
					className: unlockStyles().pre,
					children: unlockData
				})
				]
			})
		})
		
		]
    }) : (n("/404"), null)
}
```
添加解锁标签的输出样式
```
function unlockStyles() {
  return {
    container: "space-y-4 p-4 bg-stone-100/70 rounded-[12px] shadow-sm dark:bg-stone-800/70",
    loading: u(
      "absolute inset-0 flex items-center justify-center bg-white/80 text-sm font-medium",
      "dark:bg-stone-900/80 backdrop-blur-sm"
    ),
    error: u(
      "rounded-lg bg-red-100 p-3 text-red-700 text-sm font-medium",
      "dark:bg-red-900/30 dark:text-red-400"
    ),
    pre: u(
      "overflow-x-auto rounded-lg bg-white p-4 text-[13px] leading-6 shadow-sm",
      "dark:bg-stone-900 dark:text-stone-300",
      "scrollbar-thin scrollbar-track-transparent scrollbar-thumb-stone-200",
      "dark:scrollbar-thumb-stone-700"
    )
  };
}
```
### 保存文件 
\
 新增对应vps解锁信息文件```user-dist/unlockdata/服务器id.md``` 例如服务器id为1

这里使用的是我修改过的检测脚本，当然你可以使用原版解锁自己修改为需要的 [1-stream/RegionRestrictionCheck](https://github.com/1-stream/RegionRestrictionCheck)
```
mkdir user-dist/unlockdata/
bash <(curl -L -s https://github.com/Torinomii/RegionRestrictionCheck/raw/main/check.sh) -N 2 -M 4 | sed -e "s/\x1B\[[0-9;]*[a-zA-Z]//g" -e "s/\r//g" > /opt/nezha/dashboard/user-dist/unlockdata/1.md
```
需要哪台vps的解锁信息就把相对应的服务器id.md上传到```dashboard/user-dist/unlockdata/```
你可以通过scp/ftp/git等等传输方式把其他vps取得的解锁信息文件拉取

***重启探针***后刷新页面就能看到解锁信息了

# 可能遇到的问题:
如果遇到刷新后空白页的情况，可以在nginx添加下面内容禁止缓存暂时解决
```
add_header Cache-Control "no-store, no-cache, must-revalidate, max-age=0";
add_header Pragma "no-cache";
add_header Expires "0";
```
