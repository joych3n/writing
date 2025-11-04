# 浅爬一下 figma 的主要技术

Figma 是一款基于云端、实时的协作式界面设计和原型工具，可谓是人见人爱的设计神器，美观的界面，丰富的功能，优异的性能，使用体验非常棒，作为前端工程师，很好奇 figma 是基于哪些技术做到如此优秀，尤其是性能方面，简直不像网页能做到的水平，今天就浅爬一下 figma 的主要技术。

## 1、WebAssembly + Rust

最关键的底层引擎（渲染、约束 layout、矢量计算等）不是 JS 写的，是 Rust 写的，然后编译成 WebAssembly 跑在浏览器。

为什么？：

- JS 对大量矢量运算、矩阵运算太慢

- WebAssembly 更接近 native 性能

- Rust 内存安全、不容易有 C++ 那样的野指针问题

实现了在浏览器中跑一个 Photoshop 的感受，靠的是 wasm。

## 2、Canvas 2D / WebGL 渲染

设计工具有很多矢量图、组件树，传统 DOM 根本承不住。
所有画板、图层的渲染都在 Canvas / WebGL 上用 GPU 来画。（“DOM 只负责界面 UI，不负责具体设计画面”）

## 3、实时协同：CRDT + WebSocket

多人实时协作，核心是“冲突控制 + 状态同步”。CRDT（Conflict-free Replicated Data Type）是目前热门方案，figma 也在朝这个思路演进，大概原理如下：

- 每个客户端本地能直接编辑

- WebSocket 每个编辑实时发给 server

- server 广播给其他客户端

- 本地算法自动把多个修改合并成一致结果

**总结一句话：Figma = WebAssembly + Canvas/WebGL + 实时协同同步协议**
