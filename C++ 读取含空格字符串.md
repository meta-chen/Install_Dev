# C++ 读取含空格字符串

```c++
string s;
getline(cin, s);
```



```c++
char ss[50];
cin.get(ss, 50);
/*保留最后的换行符在缓冲区，使用cin.get()可读*/
char c;
cin.get(c);
```



```c++
char ss[50];
cin.getline(ss, 50);
/*舍弃最后的换行符*/
```

