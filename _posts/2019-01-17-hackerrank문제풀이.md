https://www.hackerrank.com


- 문제

![문제](/assets/deploy/hackerrank-print-function.png)

- 정답

```python
def printnum(x):
    limit = x + 1
    start = 1
    global num
    num = []
    
    for i in range(start, limit):
        num.append(str(start))
        start += 1
    return num    

if __name__ == '__main__':
    n = int(input())
    num = []
    printnum(n)
    result = ''.join(num)
    print(int(result))
```



sg-09ce169f65c90e5d9 추가