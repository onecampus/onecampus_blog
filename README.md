# http://onecampus.github.io

## Hugo Download
https://github.com/spf13/hugo/releases

## Hugo 初尝
```
hugo new site hugo_blog
cd hugo_blog

hugo new first.md
hugo undraft content/first.md

mkdir themes
cd themes
git clone https://github.com/spf13/hyde.git
cd hyde
rm -rf .git

cd ..
cd ..
hugo server -t hyde --watch
```

## Website Configuration
**`toml` or `yaml` or `json`**
```
baseurl = "http://onecampus.github.io/"
languageCode = "zh-CN"
title = "Onecampus T.D."
theme = "hyde"

[params]
	description = "I am the bone of my sword."
	#disqusShortname = "spf13"

[[menu.main]]
	Name = "organization"
	Url = "https://github.com/onecampus"
```

- more about **`toml`**：http://segmentfault.com/a/1190000000477752
- more about **`configuration`**：http://gohugo.io/overview/configuration/

## 用 Hugo 编写团队博客
1. **fork**
2. **write articles**
3. **pull requests**

## Other
- **themes**：https://github.com/spf13/hugoThemes
- **syntax higlighting**：http://gohugo.io/extras/highlighting
  - 默认的 Server-Side 着色有问题，建议改为 Client-Side Syntax Highlighting，使用 **[highlight.js](https://highlightjs.org/)**