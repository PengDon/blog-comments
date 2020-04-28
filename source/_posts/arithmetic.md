---
title: JavaScript算法1
categories: 
- Arithmetic
tags: 
- Javascript
# mathjax: true # 支持数学公式
---

## 

### 1. 输出没有连续重复字符的组合数

> **问题描述**：把一个字符串中的所有的字符重新排列，然后生成一个新的字符串，返回的新字符串中没有连续重复的字符。连续重复是以单个字符为判断标准。
 例如：aab应该返回 2， 因为它总共有 6 种排列方式： aab， aab， aba， aba， baa， baa，但是其中只有 2 个没有连续重复的字符（字符 a 是本例中的重复字符）：aba，aba

 > **预期结果**：
 permAlone("aab")应该返回 2。
 permAlone("aaa")应该返回 0。
 permAlone("aabb")应该返回 8。
 permAlone("abcdefa")应该返回 3600。
 permAlone("abfdefa")应该返回 2640。
 permAlone("aaab")应该返回 0。


``` bash
function permAlone(str) {
    // 匹配是否有重复字符
    let reg = /(\w)\1+/g;
    if (str.match(reg) !== null && str.match(reg)[0] === str) return 0;
    let arr = str.split('');
    // 利用es6解构赋值交换元素位置
    function swap(n1,n2){
        [arr[n1],arr[n2]] = [arr[n2],arr[n1]]
    } 
    let tempArr = [];
    function generate(len){
        if(len === 1 && !arr.join('').match(reg)){
            tempArr.push(arr.join(''))
        }else{
            for(let i = 0; i!=len;++i){
                generate(len-1);
                swap(len%2?0:i,len-1)
            }
        }
    } 
    generate(arr.length) 
    // 利用filter去重返回新数组
    return tempArr.length;
}
```

### 2. 输出对等分差

> **问题描述**：两个集合的对称差分是只属于其中一个集合，而不属于另一个集合的元素组成的集合，例如：集合let A = [ 1, 2, 3]和let B = [ 2, 3, 4]的对称差分为A △ B = C = [ 1, 4]。 集合论中的这个运算相当于布尔逻辑中的异或运算。
设定两个数组 (例如：let A = [1, 2, 3]，let B = [2, 3, 4])作为参数传入，返回对称差分数组（A △ B = C = [1, 4]），且数组中没有重复项。

 > **预期结果**：
 sym([1, 2, 3, 3], [5, 2, 1, 4])应该返回[3, 4, 5]。
 sym([1, 2, 3], [5, 2, 1, 4, 5])应该返回[3, 4, 5]。
 sym([1, 2, 5], [2, 3, 5], [3, 4, 5])应该返回[1, 4, 5]。
 sym([1, 1, 2, 5], [2, 2, 3, 5], [3, 4, 5, 5])应该返回[1, 4, 5]。
 sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3])应该返回[2, 3, 4, 6, 7]。
 sym([3, 3, 3, 2, 5], [2, 1, 5, 7], [3, 4, 6, 6], [1, 2, 3], [5, 3, 9, 8], [1])应该返回[1, 2, 4, 5, 6, 7, 8, 9]。

 ``` bash
function sym(...args) {
    // 利用reduce组合数据，再利用Set去重
    return [...new Set(args.reduce(diffArray))].sort();
}

// 区分两个数组,返回不同部分
function diffArray(arr1, arr2) {
   return arr1
        .filter(element => !arr2.includes(element))
        .concat(arr2.filter(element => !arr1.includes(element)));
}
```