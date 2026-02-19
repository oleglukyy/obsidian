Two questions:
### How much time does this algorithm need to finish?
### How much space does this algorithm need for this computation?

Big-O Notation gives an uper bopund of the complexity in the worst case, helping to quantify performance as the input size becomes arbitrarily large.

Big-O Notation

n - The size of the input
Complexities ordered in from smallest to largest

Constant Time: O(1)
Logarithmic Time: O(log(n))
Linear Time: O(n)
Linearithmic Time: O(nlog(n))
Quadratic Time: O(n²)
Cubic Time: O(n³)
Exponential Time: O(bⁿ), b > 1
Factorial Time: O(n!)

Big-O Properties

O(n + c) = O(n)
O(cn) = O(n), c > 0

Let f be a function that describes the running time of a particular algorithm for an input of size n:

f(n) = 7log(n)³ + 15n² + 2n³ + 8
O(f(n)) = O(n³)


O(1)

a-> 1
b->2
c-> a+5*b;

```
i:=0
do{
i=i+1;
}while (i<11);
```


0(n)

Run in linear time
```
i:=0
do{
i=i+1;
}while (i<n);

f(n)=n
O(f(n)) = O(n)
```