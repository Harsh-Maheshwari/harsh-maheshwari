---
title: Obsidian Tricks
date: 2022-04-22 23:58:19
description:
tags: 
 - obsidian
 - tools
icon: material/emoticon-happy
status: [Todo|Inprogress|Published|Arxived]
external_references: 
urls: 
aliases: 
---
# [[Obsidian Tricks]]

##### [How to convert Markdown files to Medium articles](https://link.medium.com/pWSKtWlOhpb)

### Plugins
#### [Obsidian Plugins](https://youtu.be/W7kTtn9empU)
<iframe width="560" height="315" src="https://www.youtube.com/embed/W7kTtn9empU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
#### [Dataview Plugin](https://youtu.be/7kFEl7Ovsr8)
<iframe width="560" height="315" src="https://www.youtube.com/embed/7kFEl7Ovsr8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Templates
#### [Useful Templates for Obsidian](https://youtu.be/RfRFS0S-tNs)
<iframe width="560" height="315" src="https://www.youtube.com/embed/RfRFS0S-tNs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Obsidian Data Views
```dataview
table  file.path as path
where file.name = "README"
sort path desc

table file.ctime as creation, file.mtime as modification,  split(file.path, "/")[1] as folder  , status from  
"Public"
where file.name != "README"
sort folder desc
```