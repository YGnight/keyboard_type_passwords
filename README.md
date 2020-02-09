# 键盘组合口令字典 #

从一些泄露的口令数据中，提取了键盘组合模式的口令，并根据频率排序，生成一个包含1q2w3e4r zxcvbnm qwertyuiop等常见键盘组合的口令字典。

提取算法（比较笨），简单来说就是看后一个字符在键盘上是不是和前一个相邻：
```python
def keyboardMatch(S):
    S = S.lower()
    keyDict = {
        '1': '2q@',
        '2': '3w#',
        '3': '4e$',
        '4': '5r%',
        '5': '6t^',
        '6': '7y&',
        '7': '8u*',
        '8': '9i(',
        '9': '0o)',
        '0': 'p',
        'q': '12was@',
        'w': '23eds#',
        'e': '34rfd$',
        'r': '45tgf%',
        't': '56yhg^',
        'y': '67ujh&',
        'u': '78ikj*',
        'i': '89olk(',
        'o': '90pl)',
        'p': '',
        'a': 'wsxz',
        's': 'wedcx',
        'd': 'erfvc',
        'f': 'rtgbv',
        'g': 'tyhnb',
        'h': 'yujmn',
        'j': 'uikm',
        'k': 'iol',
        'l': '',
        'z': 'asx',
        'x': 'sdc',
        'c': 'dfv',
        'v': 'fgb',
        'b': 'ghn',
        'n': 'hjm',
        'm': 'j',
        '!': 'q@w',
        '@': 'w#e',
        '#': '$re',
        '$': '%rt',
        '%': '^ty',
        '^': '&yu',
        '&': '*iu',
        '*': '(io',
        '(': ')op',
        ')': 'p',
    }
    #排除这些口令
    T = ['1234', '12345', '123456', '1234567', '12345678', '123456789']
    if S in T:
        return False
    if len(S) < 4:
        return False
    for i in range(0, len(S) - 1):
        if S[i] not in keyDict.keys():
            return False
        if S[i + 1] in keyDict[S[i]]:
            i += 1
        else:
            return False
    return True
```

泄露的口令数据：csdn、7k7k、renren等一共几千万条，都比较老了。

然后将提取的键盘组合口令（未去重）保存到keyboardType.txt，进行统计：

```python
result = {}

with open("./result/keyboardType.txt", 'r') as f:
    f1 = f.readlines()
    print(len(f1))
    f2 = set(f1)
    print(len(f2))
    for i in f2:
        result[i] = f1.count(i)

result_sort = sorted(result , key=result.get)[ : :-1]

for i in result_sort:
    print(i.strip(), result[i])
```
结果大概这样：
```
#去重前的行数
204415
#去重后的行数
2249
#按频率排序结果：
q123456 40747
w2w2w2 34627
1234567890 29363
1q2w3e4r 10102
zxcvbnm 8641
qwertyuiop 6376
asdfghjkl 5879
···
```
最后提取出top100、top500丰富一下字典，留着以后爆破用。