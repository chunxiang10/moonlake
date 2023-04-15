---
title: "更多富文本显示"
date: "2023-03-18T16:26:23+0800"
tags: ["hugo"]
---

主题的富文本显示方式

支持mermaid

{{<mermaid>}}
flowchart LR

    A --> B

    B --> C

    C --> D 

    D --> B
{{</mermaid>}}

```bash
\{\{<mermaid>\}\}
flowchart LR
    A --> B
    B --> C
    C --> D 
    D --> B
\{\{</mermaid>\}\}
```

{{< notice note >}}
这里是一个注释
{{< /notice >}}

```bash
# 实际使用时要删除\符号
\{\{< notice note >\}\}
这里是一个注释
\{\{< /notice >\}\}
```

{{< notice tip >}}
关于事情的一些提示
{{< /notice >}}

```bash
# 实际使用时要删除\符号
\{\{< notice tip >\}\}
关于事情的一些提示
\{\{< /notice >\}\}
```

{{< notice example >}}
举个例子
{{< /notice >}}

```bash
# 实际使用时要删除\符号
\{\{< notice example >\}\}
举个例子
\{\{< /notice >\}\}
```

{{< notice question >}}
你有什么问题吗？
{{< /notice >}}

```bash
# 实际使用时要删除\符号
\{\{< notice question >\}\}
你有什么问题吗？
\{\{< /notice >\}\}
```

{{< notice info >}}
请注意，这里是一些信息
{{< /notice >}}

```bash
# 实际使用时要删除\符号
\{\{< notice info >\}\}
请注意，这里是一些信息
\{\{< /notice >\}\}
```

{{< notice warning >}}
FBI WARNING！
{{< /notice >}}

```bash
# 实际使用时要删除\符号
\{\{< notice warning >\}\}
FBI WARNING！
\{\{< /notice >\}\}
```

{{< notice error >}}
请检查错误！
{{< /notice >}}

```bash
# 实际使用时要删除\符号
\{\{< notice error >\}\}
请检查错误！
\{\{< /notice >\}\}
```