--- 
date: 2024-05-20T16:27:03+08:00
title: "HUGO为文章添加访问密码"
---
## 前言
因为一些原因想给博客的一些文章添加访问密码，网上一番搜索整理加调试，遂成此文。
{{< notice warning >}}
此方法的加密并非算法加密，不适用于一些需要高度机密的场合。
{{< /notice >}}

## 步骤
修改主题的header.html

```
vim themes/moonlake/layouts/partials/head.html
```

在head.html底部添加js脚本

```javascript
{{ if ( .Params.password | default "" ) }}
    <script>
        (function(){
            if (prompt('请输入文章密码') != {{ .Params.password }}){
                alert('密码错误！');
                if (history.length === 1) {
                    window.opener = null;
		    window.open('', '_self');
		    window.close();
		} else {
		    history.back();
		}
	    }
	})();
    </script>
{{ end }}
```

之后只要在加密文章的头部加上`password`属性即可进行加密，只有输入了正确密码才能打开文章，否则会回退到之前的页面。用法如下：

```ymal
title: "文章标题"
password: test
```

不添加`password`属性则不会加密


