---
title: "JS排序算法"
date: 2019-10-26T19:34:36+08:00
draft: false
---

## 一 选择排序

选择排序 即在当前区间选出一个最小值和区间第一个数交换位置，区间大小减一<br>
一共有n个数， 第一次遍历n个数，第二次遍历n-1个数，以此类推，<br>
共遍历 n + (n - 1) + (n - 2) + ... + (n - n + 1) + (n - n) 次 (加0不影响最终结果)<br>
例如   数组一共有5个数，遍历 5 + 4 + 3 + 2 + 1 + 0 次<br>
也可以写作 n + ... + 3 + 2 + 1 + 0 = (n * (n + 1)) / 2 = 1/2(n^2) + (1/2)n <br>
时间复杂度O(n^2)

```js
const log = console.log.bind(console)

//求当前区间内最小值下标
let minIndex = (numbers) => {
  let index = 0
  for(let i = 1; i < numbers.length; i++){
    if(numbers[i] < numbers[index]){
      index = i
    }
  }
  return index
}

let swap = (array, i, j) => {
  let temp = array[i]
  array[i] = array[j]
  array[j] = temp
}

let sort = (numbers) => {
  //遍历到区间只剩下两个数就可以停止了, 也就是i等于numbers.length - 2
  //如8个数 i等于numbers.length - 2 = 6, 区间是arr[6]-arr[7] 没有arr[8] :)
  for(let i = 0; i < numbers.length - 1; i++){
    log(`------`)
    log(`i: ${i}`)
    //求当前区间内的最小值在整个数组内的下标
    //为当前区间内的最小值的下标 加 区间前面的数字个数
    //例如 [6,2,4,5]  1 + 0
    //再比如 6,[2,4,5] 0 + 1
    let index = minIndex(numbers.slice(i)) + i
    log(`index: ${index}`)
    log(`min: ${numbers[index]}`)
    if(index!==i){
      swap(numbers, index, i)
      log(`swap ${index}:${i}`)
      log(numbers)
    }
  }
  return numbers
}
```

## 二 快速排序

快速排序,即取数组中间位置的数，比它小的数放在它的左边，比它大的放在右边
然后区间变为它的左边的数组成的数组 和 它的右边的数组成的数组
最后区间只剩一个数终止
```js
let quickSort = arr => {
  if (arr.length <= 1) { 
    return arr; 
  }
  let pivotIndex = Math.floor(arr.length / 2);
  let pivot = arr.splice(pivotIndex, 1)[0];
  let left = [];
  let right = [];
  for (let i = 0; i < arr.length; i++){
    if (arr[i] < pivot) { 
      left.push(arr[i])
    } else { 
      right.push(arr[i]) 
    }
  }
  return quickSort(left).concat([pivot], quickSort(right))
}

```

## 三 归并排序

```js
let merge = (a, b) => {
  if (a.length === 0) {
    return b
  }
  if (b.length === 0) {
    return a
  }
  return a[0] > b[0] ?
    [b[0]].concat(merge(a, b.slice(1))) :
    [a[0]].concat(merge(a.slice(1), b))
}

let mergeSort = arr => {
  let k = arr.length
  if (k === 1) { 
    return arr
  }
  let left = arr.slice(0, Math.floor(k / 2))
  let right = arr.slice(Math.floor(k / 2))
  return merge(mergeSort(left), mergeSort(right))
}

```

## 四 计数排序

```js
let countSort = arr => {
  let hashTable = {}, max = 0, result = []
  for(let i = 0; i < arr.length; i++){ 
    if(!(arr[i] in hashTable)){ 
      hashTable[arr[i]] = 1
    }else{
      hashTable[arr[i]] += 1
    }
    if(max < arr[i]) {
      max = arr[i]
    }
  }
  for(let j = 0; j <= max; j++){ 
    if(j in hashTable){
      for(let i = 0; i < hashTable[j]; i++){
        result.push(j)
      }
    }
  }
  return result
}

```

时间不够，待更新