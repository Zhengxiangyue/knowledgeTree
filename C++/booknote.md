set_new_handler(int) 为 new 失败提供 handler



```c++
int theSize = result.size() - 1;
        for(int i = 0 ; i < result.size() - 1 ; i++) {
            // cout << i << endl;
            randomPosition = rand() % (result.size() - i) + i;
            swap(result[i], result[randomPosition]);
        }
```

不知道为什么 result.size() 为零时会执行一遍，但是存进一个变量替换后就正常了