# js 对象数组多维排序简写

```javascript
const arr = [
  { name: '商品01', price: 123.0, sales: 12345, postage: 19 },
  { name: '商品02', price: 123.0, sales: 12345, postage: 0 },
  { name: '商品03', price: 123.0, sales: 13345, postage: 29 },
  { name: '商品04', price: 125.0, sales: 12345, postage: 9 },
  { name: '商品05', price: 123.0, sales: 14345, postage: 19 }
];

const sortArr = arr.sort((a, b) => {
  return a.price - b.price || a.postage - b.postage || b.sales - a.sales;
});

function multiSort(arr, keys, orders) {
  if (!keys) return arr;
  if (!orders) {
    orders = Array.from({ length: keys.length }, () => 1);
  }
  return arr.sort((a, b) => {
    for (let [index, key] of keys) {
      if (a[key] - b[key] === 0) continue;
      return (a[key] - b[key]) * orders[index];
    }
  });
}
```
