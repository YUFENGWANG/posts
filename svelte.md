# Svelte

[Svelte](https://github.com/sveltejs/svelte)是一款的前端库，作者 Rich Harris。2020 年在 React/Vue/Angular 三分天下的局面下，Svelte 又有何特色，本文将做简单分析及评测。

## 主要特点

- Write less, Do more
- 没有虚拟 dom
- 无需额外状态管理，内置响应式

详细介绍请参考[官方文档](https://svelte.dev/)。

## 评测

1. [Js-framework-benchmark](https://github.com/krausest/js-framework-benchmark), keyed results
   ![](./images/1.png)
   数据分析：在大数据量的 dom 操作场景下，Svelte 表现不错。超过 React/Vue/Angular。Inferno 很强
   ![](./images/2.png)
   数据分析：在移动端模拟的场景下。svelte 的数据表现不错，超过 React/Vue/Angular。
   ![](./images/3.png)
   数据分析：在内存使用上，Svelte 仍然领先 React/Vue/Angular。仅次于 Vanillajs.

注： 上述结果仅代表在特定测试 case，特定浏览器，特定硬件条件下，不同框架/库的表现结果差异。在实际应用场景中，可能会有不同，仅做参考。

2. SSR 性能对比

   在选用框架或库的时候，对 ssr 的支持也是重要考量。本次评测，React 选择 Next.js，Vue 选用 Nuxt.js，Angular 用官方的 ssr 教程，Svelte 选取官方支持的 Sapper.js 来做本次 ssr 性能测试。

   压力测试工具选用[wrk](https://github.com/wg/wrk)，

   测试环境: 公司的 Mac（2.3 GHz 双核 Intel Core i5，8 GB 2133 MHz LPDDR3）

   源代码见 gitlab，[链接](https://registry.code.tuya-inc.top/wangyf/ssr-test)

   本次压力测试仅以渲染 hello，world 为例。使用 wrk 开 12 个线程，400 个连接，连续压测 30s，测试 4 轮，每个框架每轮的测试顺序都跟上一轮不一致。所有 ssr 框架的代码均 build 后跑在生产模式。

   测试代码：

   ```
   wrk -t12 -c400 -d30s http://127.0.0.1:[port]
   ```

   wrk 的测试输出示例：这里我们只选取`Requests/sec: 1034.80`(qps)这行作为衡量性能的指标

   ```
   Running 30s test @ http://127.0.0.1:3000
     12 threads and 400 connections
     Thread Stats   Avg      Stdev     Max   +/- Stdev
       Latency   229.12ms   55.94ms 712.60ms   85.70%
       Req/Sec    91.01     67.48   290.00     63.47%
     31140 requests in 30.09s, 51.70MB read
     Socket errors: connect 156, read 144, write 0, timeout 0
   Requests/sec:   1034.80
   Transfer/sec:      1.72MB
   ```

   下面是测试 4 轮后的结果对比：

|                 | Vue/Nuxt                                   | Angular/Angular-SSR                        | Svelte/Sapper                                | React/Next                                  |
| :-------------- | ------------------------------------------ | ------------------------------------------ | -------------------------------------------- | ------------------------------------------- |
| Round1(V/A/S/R) | Requests/sec:1034.80，Transfer/sec: 1.72MB | Requests/sec:1081.67，Transfer/sec: 1.18MB | Requests/sec:14177.70，Transfer/sec: 17.02MB | Requests/sec:5762.73，Transfer/sec: 11.96MB |
| Round2(R/V/A/S) | Requests/sec:1002.95，Transfer/sec: 1.67MB | Requests/sec:1078.95，Transfer/sec: 1.18MB | Requests/sec:12865.78，Transfer/sec: 15.45MB | Requests/sec:6468.29，Transfer/sec: 13.43MB |
| Round3(S/R/V/A) | Requests/sec: 937.85，Transfer/sec: 1.56MB | Requests/sec:1085.22，Transfer/sec: 1.18MB | Requests/sec:13053.71，Transfer/sec: 15.67MB | Requests/sec:6790.16，Transfer/sec: 14.10MB |
| Round4(A/S/R/V) | Requests/sec:1028.02，Transfer/sec: 1.71MB | Requests/sec:1087.90，Transfer/sec: 1.18MB | Requests/sec:13464.50，Transfer/sec: 16.17MB | Requests/sec:7092.70，Transfer/sec: 14.73MB |

数据分析: 在 ssr 的场景下，同样渲染一个最简单的 hello，world。Svelte/Sapper 的 QPS 摇摇领先，是第二名 React/Next 的差不多 2 倍。Vue 最差(不知道接下来 vue3 出来后是否会有所改善？期待之），Angular 次之。

注： 本次测试仅代表个人测试结果。不合理之处欢迎大家探讨

## 总结

优点：Svelte 性能出众，代码量少，bundle 体积小。以 compiler 为核心，具有很大的想象空间。

缺点：生态，语法。

一句话概括：在 2020 年，svelte 值得学习，尤其是对性能要求高，对 bundle 体积有要求的场景下(移动端)，可以考虑试用。
