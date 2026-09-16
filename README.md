# AI Agent 一页简历模板

面向 AI Agent 岗位的一页简历模板，提供 Markdown 和 LaTeX 两种版本。
当前版本为紧凑双实习双项目布局，顶部右侧支持个人照片。

## 文件

- `AI_Agent_一页简历模板.md`：适合直接编辑或转换为其他格式。
- `AI_Agent_一页简历模板.tex`：适合生成排版稳定的 PDF。
- `photo.jpg`：顶部右侧个人照片，可替换为同名 JPG/PNG 文件。

## 使用

替换模板中的方括号占位内容。LaTeX 版本使用 XeLaTeX 编译：

```bash
xelatex AI_Agent_一页简历模板.tex
```

如果使用 Overleaf，请将 Compiler 设置为 `XeLaTeX`，并将 `photo.jpg` 与 `.tex` 文件一起上传。
