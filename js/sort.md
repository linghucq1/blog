# js 对象数组多维排序简写

对以下数组进行排序，要求：价格低排前面、同价格邮费低在前、前两个相等销量高的在前
```javascript
const arr = [
  { name: '商品01', price: 123.0, sales: 12345, postage: 19 },
  { name: '商品02', price: 124.0, sales: 12345, postage: 0 },
  { name: '商品03', price: 123.0, sales: 13345, postage: 29 },
  { name: '商品04', price: 125.0, sales: 12345, postage: 9 },
  { name: '商品05', price: 123.0, sales: 14345, postage: 19 }
];
```

<br/>

用短路写法可以一行搞定
``` javascript
arr.sort((a, b) => {
  return a.price - b.price || a.postage - b.postage || b.sales - a.sales;
});
// arr:
// [
//   { name: '商品01', price: 123, sales: 12345, postage: 19 },
//   { name: '商品05', price: 123, sales: 14345, postage: 19 },
//   { name: '商品03', price: 123, sales: 13345, postage: 29 },
//   { name: '商品02', price: 124, sales: 12345, postage: 0 },
//   { name: '商品04', price: 125, sales: 12345, postage: 9 }
// ];
```
<br/>

实现一个对象数组多维排序函数
``` javascript
const keys = ['price', 'postage', 'sales'];
const orders = [1, 1, -1];

function multiSort(array, keys, orders) {
  if (!keys) return array;
  if (!orders) {
    orders = Array.from({ length: keys.length }, () => 1);
  }
  return array.sort((a, b) => {
    for (let [index, key] of keys.entries()) {
      if (a[key] - b[key] === 0) continue;
      return (a[key] - b[key]) * orders[index];
    }
  });
}

multiSort(arr, keys, orders)
// arr:
// [
//   { name: '商品01', price: 123, sales: 12345, postage: 19 },
//   { name: '商品05', price: 123, sales: 14345, postage: 19 },
//   { name: '商品03', price: 123, sales: 13345, postage: 29 },
//   { name: '商品02', price: 124, sales: 12345, postage: 0 },
//   { name: '商品04', price: 125, sales: 12345, postage: 9 }
// ];
```
