```
graph TD
A(createChunkAssets) --> B{chunk.entry}
B{chunk.entry} --> |true|C[mainTemplate]
B{chunk.entry} --> |null/false|D[chunkTemplate]
C[mainTemplate] --> |处理入口js文件| E[render事件]
D[chunkTemplate] --> |处理异步加载js|F[render方法]
E[render事件] --> G[Template]
F[render方法] --> G[Template]
G[Template] --> |renderChunkMoc|H[ConcatSource]
H[ConcatSource] --> I{chunk.module}
I{chunk.module} --> |true|J[封装module]
J[封装module] --> |各模块doBlock方法|K[添加到Source map]
K[添加到Source map] --> I{chunk.module}
I{chunk.module} --> |null/false|L[将source添加assets]
L[将source添加assets] --> M(return assets)
```