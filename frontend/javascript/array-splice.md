---
title: "大数组移除给定索引元素"
tags:
  - 八股
  - 前端
  - JS
  - 算法
created: 2026-07-17
summary: "大数组移除给定索引元素几种方法对比"
---

## 给定大数组，给定索引，移除元素。时间复杂度

**移除大数组中多个指定索引元素: splice()函数，还是自己比较?**

浏览器针对splice()函数有优化，但如果移除多个元素，每次均需要元素前移重组数组（后面的元素前移重组，保持连续内存，否则内存会出现「空洞」），因此时间复杂度为0(n)，删除量大时影响效率。
自己比较，创建新数组，压入。
如果删除元素较少，即需要保留大量连续元素，每次push均会操作内存，影响效率。
最快是fori循环(可以提自己测过fori/forof/forEach()效率，ppt里有)，自己比较，记录开始/结束索引，一次性压入。（fori > foreach > forof）
newArray.push(...arr.slice(start, end));（批量push）

- 单个索引：优先原地修改（splice / 覆盖 + pop），简单且引擎优化好；
- 多个索引：优先哈希表（Set/Map）+ 一次遍历，避免 O (n²) 复杂度；

<hr/>

#### 多个索引:

set方法平均用时168.19
map方法平均用时125.30
obj方法平均用时210.75
**splice方法平均用时60.99**(800万数组，移除9个)

```js
// 初始化
      const data = [];
      for (let i = 0; i < 8000000; i++) {
        number = Math.floor(Math.random() * 10000).toString();
        data.push({ id: number });
      }

      // set记录遍历
      function removeSet(data, indexs) {
        const setx = new Set();
        const result = [];
        indexs.forEach((index) => {
          setx.add(index);
        });

        for (let i = 0; i < data.length; i++) {
          if (!setx.has(i)) {
            result.push(data[i]);
          }
        }

        return result;
      }

      // map记录遍历
      function removeMap(data, indexs) {
        const mapx = new Map();
        const result = [];
        indexs.forEach((index) => {
          mapx.set(index, 1);
        });

        for (let i = 0; i < data.length; i++) {
          if (!mapx.has(i)) {
            result.push(data[i]);
          }
        }

        return result;
      }

      // 3.对象属性记录遍历
      function removeObj(data, indexs) {
        const result = [];
        const obj = {};
        indexs.forEach((index) => {
          obj[index] = 1;
        });

        for (let i = 0; i < data.length; i++) {
          if (!obj.hasOwnProperty(i)) {
            result.push(data[i]);
          }
        }
        console.log(obj);
        return result;
      }

      function timeTest() {
        const indexTest = [
          0, 100, 500, 999, 1000, 9999, 99999, 999999, 7999999,
        ];

        let setTotal = 0,
          mapTotal = 0,
          objTotal = 0;
        // 1. 预热：先执行一次，让引擎编译优化
        removeSet(data, indexTest);
        removeMap(data, indexTest);
        removeObj(data, indexTest);
        const testTimes = 15;

        // 2. 循环测试多次
        for (let i = 0; i < testTimes; i++) {
          const setStart = performance.now();
          const setResult = removeSet(data, indexTest);
          const setEnd = performance.now();
          setTotal += setEnd - setStart;

          const mapStart = performance.now();
          const mapResult = removeMap(data, indexTest);
          const mapEnd = performance.now();
          mapTotal += mapEnd - mapStart;

          const objStart = performance.now();
          const objResult = removeObj(data, indexTest);
          const objEnd = performance.now();
          objTotal += objEnd - objStart;
        }
        // 3. 计算平均耗时
        const setAvg = (setTotal / testTimes).toFixed(2);
        const mapAvg = (mapTotal / testTimes).toFixed(2);
        const objAvg = (objTotal / testTimes).toFixed(2);

        console.log(`set方法平均用时${setAvg}`);
        console.log(`map方法平均用时${mapAvg}`);
        console.log(`obj方法平均用时${objAvg}`);
      }

      timeTest();
```

