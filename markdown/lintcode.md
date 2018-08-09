```js
/** 2018.8.5
 * @param a: An integer
 * @param b: An integer
 * @return: The sum of a and b 
 */
const aplusb = function (a, b) {
  while (b) {
    [a, b] = [a ^ b, (a & b) << 1];
  }
  return a;
}
// 不带进位的 a+b 与 a+b 的进位 相加
// 不带进位00 0、01 1、11 1. &
// 进位00 0、01 1、11 0 ^
// 但是进位需要左移一位
```

```js
/** 2018.8.6
 * @param n: A long integer
 * @return: An integer, denote the number of trailing zeros in n!
 */
const trailingZeros = function (n) {
   return n ? Math.floor(n / 5) + trailingZeros(Math.floor(n / 5)) : 0
}
// 计算0的个数 实际上是计算 2 * 5 的次数。
// 例如 10！， 1*10， 2*5，会产生末尾0 但是10也可以看成 2*5
```

```js
/** 2018.8.6
 * @param A: sorted integer array A
 * @param B: sorted integer array B
 * @return: A new sorted integer array
 */
const mergeSortedArray = function (A, B) {
    const result = [];
    
    while(A.length && B.length) {
        const a = A[0];
        const b = B[0];
        
        if (a === b) {
            result.push(a, b);
            A.shift();
            B.shift();
        } else if (a < b){
            result.push(a);
            A.shift();
        } else {
            result.push(b);
            B.shift();
        }
    }
    
    return result.concat(A, B);
}
```

```js
/** 2018.8.8 KMP
  *
  *
  */
var S = 'AAAAAA';
var W = 'BBC ABCDAB ABCDABCDABDE';

function next(string) {
  var nextTable = [];

  for(var i = 0; i < string.length; i++) {
    var str = string.substring(0, i + 1);

    var isMatched = false;
    for (var k = 0; k < i - 1; k++) {
      var prefix = str.slice(0, k);
      var suffix = str.slice(-k);

      if (prefix === suffix) {
        nextTable.push(prefix.length);
        isMatched = true;
      }
    }
    if (!isMatched) {
      nextTable.push(0);
    }
  }
  return nextTable;
}

var nextTable = next(S);

var m = 0;
var i = 0;

while(m < W.length) {
  if (W[m] === S[i]) {
    var m_cache = m;
    for (var idx = i; idx < S.length; idx++) {
      m++;
      if (S[idx] !== W[m_cache + idx]) {
        i = nextTable[idx - 1];
        break;
      }

      if (idx === S.length - 1) {
        console.log(m_cache);
      }
    }
  } else {
    i = nextTable[i - 1] || 0;
    m++;
  }
}
```
