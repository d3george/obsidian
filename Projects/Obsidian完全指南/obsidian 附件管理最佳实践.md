---
tags:
  - source/article
  - type/summary
  - topic/obsidian
uploaded: 2025-11-10
url: https://xtoolism.github.io/xtool/Obsidian/obsidian-%E9%99%84%E4%BB%B6%E7%AE%A1%E7%90%86%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5
---

### [custom-attachment-location](obsidian://show-plugin?id=obsidian-custom-attachment-location)

核心功能:

- 将笔记的图片移动到笔记名称的目录 (`./assets/${filename}`)，如果没有图片，则不创建目录；
- 文件名统一命名: 我习惯自己命名，这样便于检索，所以关闭这个特性；
- **批量处理**: 将整个库/指定目录/单文件内的附件按配置规则移动并重命名. 一键整理的福音；
- **自动处理更新**: 笔记重命名，笔记移动，文件重命名会自动更新关联， 这个自动更新功能太强了，其他插件会存在引用失效问题 

配置见截图：
![](./assets/Obsidian%20附件管理最佳实践/image.png)

### [image-converter](obsidian://show-plugin?id=image-converter)
我们使用该插件来控制粘贴图片时的一些行为
-  **Folder** 控制粘贴的图片保存到哪个文件夹下
	![](./assets/Obsidian%20附件管理最佳实践/image-1.png)
- **FileName** 控制粘贴的图片的文件名
	![](./assets/Obsidian%20附件管理最佳实践/image-2025-11-11-20-19-56.png)
- **Link format** 控制粘贴图片在文章类的显示形式
	- **Link format**: 选项要设置为 Markdown
	- **Path format** 选项要设置为 Relative
	
	 这样粘贴后自动生成 `![](./assets/文件名/image-时间戳.png)`这种格式的 link, 方便在任何支持 Markdown 的编辑器中显示
	![](./assets/Obsidian%20附件管理最佳实践/image-2025-11-11-20-22-03.png)



按照上面配置好之后，此时在 `B/demo.md`笔记中复制图片，图片会自动保存为 `B/assets/demo/image-n.png`

![](./assets/Obsidian%20附件管理最佳实践/image-2.png)