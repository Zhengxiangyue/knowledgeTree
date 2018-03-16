## volatile

Consider this code,

```
int some_int = 100;

while(some_int == 100)
{
   //your code
}
```

When this program gets compiled, the compiler may optimize this code, if it finds that the program **never** ever makes any attempt to change the value of `some_int`, so it may be tempted to optimize the `while` loop by changing it from `while(some_int == 100)` to simply `while(true)` so that the execution could be fast (since the condition in `while` loop appears to be `true` always). *(if the compiler doesn't optimize it, then it has to fetch the value of some_int (if it's not loaded on a register) and compare it with 100, each time which obviously is a little bit slow.)*

However, sometimes, optimization (of some parts of your program) may be **undesirable**, because it may be that someone else is changing the value of `some_int` from **outside the program which compiler is not aware of**, since it can't see it; but it's how you've designed it. In that case, compiler's optimization would **not** produce the desired result!

So, to ensure the desired result, you need to somehow stop the compiler from optimizing the `while`loop. That is where the `volatile` keyword plays its role. All you need to do is this,

```
volatile int some_int = 100; //note the 'volatile' qualifier now!
```

https://stackoverflow.com/questions/4437527/why-do-we-use-volatile-keyword-in-c