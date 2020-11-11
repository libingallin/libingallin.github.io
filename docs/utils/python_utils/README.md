# Efficient Python Code Snippets

## 1. `pd.DataFrame` 加速工具

- [enhancing performance doc from pandas](https://pandas.pydata.org/pandas-docs/stable/user_guide/enhancingperf.html)

- [swifter](https://github.com/jmcarpenter2/swifter)

- [pandarallel](https://github.com/nalepae/pandarallel) -- 推荐

- [modin](https://github.com/modin-project/modin)


## 2. md5 加密

```python
import hashlib

hashlib.md5('李'.encode('utf-8')).hexdigest()
```

## 3. Jupyter cell 展示所有输出

```python
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = 'all'

# cell
# show all results
df.shape
df.head()
```

## 4. Jupyter 注释/警示框

```
<div class="alert alert-block alert-info">
    <b>xxxx:</b> Blue boxes are used for tips and notes.
</div>
```

```
<div class="alert alert-block alert-warning">
    <b>xxxx:</b> Yellow boxes are generally used to include additional examples or mathematical formulas.
</div>
```

```
<div class="alert alert-block alert-success">
    <b>xxxx:</b> Use green boxes only when necessary like to display links to related content.
</div>
```

```
<div class="alert alert-block alert-danger">
    <b>xxxx:</b> It is good to avoid red boxes but can be used to alert users to not delete some important part of the code etc..
</div>
```

在代码中，`<b>xxx</b>` 用来加粗，`<code>xxx</code>` 表示代码，`<br>` 用来换行，`<font color='red' size=4>xx</font>` 更改字体颜色和大小

## 5. Jupyter 中传递/保存变量

-   `%store` -- **(看)** Show list of all variables and their current values

-   `%store xxxx` -- **(存)** Store the *current value* of the *variable xxxx* to disk（只要存储了，存储的变量 *xxxx* 与生成该变量的 notebook 状态（关闭与否）不再相关）

-   `%store -d xxxx` -- **(删)** Remove the variable *xxxx* and its value from storage （不再用到的变量及时删，自己的变量自己删）

-   `%store -z` -- **(全删跑路)** Remove all variables from storage

-   `%store -r` -- **(全读)** Refresh all variables from storage (overwrite current vals)

-   `%store -r xxxx1 xxxx2` -- **(读)** Refresh specified variables (*xxxx1* and *xxxx2*) from storage (delete current val)

-   `%store xxxx >a.txt` -- Store value of *xxxx* to new file a.txt

-   `%store xxxx >>a.txt` -- Append value of *xxxx* to file a.txt

-   `%store` -- **(看)** Show list of all variables and their current values文件

-   `%store` -- **(看)** Show list of all variables and their current values

## 6. yesterday 用在 ds 中

```python
from datetime import datetime, timedelta

yesterday = datetime.today() + timedelta(-1)
yesterday = yesterday.strftime('%Y-%m-%d')
```

## 7. 给文件/目录名加上时间戳

```python
import time

time.strftime('%Y-%m-%d')
# output: '2020-01-06'

time.strftime('./data/standard_n_%Y%m%d')
# output: './data/standard_n_20200106'

time.strftime('./data/standard_n_%Y%m%d%H%M%S.txt')
# output: './data/standard_n_20200106110949.txt'

time.strftime('./data/standard_n_%Y_%m_%d-%H_%M_%S')
# output: './data/standard_n_2020_01_06-11_10_23'
```

## 8. 提取时间字符串中的时间信息

```python
import dateutil.parser as parser

parser.parse('2020-01-06 19:20:19')
# output: datetime.datetime(2020, 1, 6, 19, 20, 19)

parser.parse('2020/01/06 19:20:19').year
# output: 2020

parser.parse('2020-01-06 19:20:19').month
# output: 1

parser.parse('2020 01 06 19:20:19').day
# output: 6
```

## 9. 时间段内随机选择时间

```python
import random
import time
from datetime import datetime

random.seed(42)


def get_random_datetime(start_date='2019-01-01', start_time='09:00:00', end_date='2019-12-31', end_time='21:00:00'):
    """现在给定日期内选择一天，再在当天给定时间段内选择一个时间。"""
    frmt_1 = '%Y-%m-%d'
    # 随机在日期段内选择一天
    start_date = time.mktime(time.strptime(start_date, frmt_1))
    end_date = time.mktime(time.strptime(end_date, frmt_1))
    date_delta = end_date - start_date
    rnd_date = datetime.fromtimestamp(random.random()*date_delta + start_date)
    rnd_date = rnd_date.strftime(frmt_1)

    # 随机在时间段内选择
    frmt_2 = '%Y-%m-%d %H:%M:%S'
    rnd_start_time = ''.join((rnd_date, ' ', start_time))
    rnd_start_time = time.mktime(time.strptime(rnd_start_time, frmt_2))
    rnd_end_time = ''.join((rnd_date, ' ', end_time))
    rnd_end_time = time.mktime(time.strptime(rnd_end_time, frmt_2))
    time_delta = rnd_end_time - rnd_start_time
    rnd_time = datetime.fromtimestamp(random.random()*time_delta + rnd_start_time)
    rnd_time = rnd_time.strftime(frmt_2)
    return rnd_date, rnd_time


get_random_datetime()  # ('2019-06-05', '2019-06-05 14:56:50')
get_random_datetime()  # ('2019-12-20', '2019-12-20 20:17:56')
```

## 10. 关于中文

```python
def is_chinese(uchar):
    """判断一个字符是否是汉字"""
    assert len(uchar) == 1, "uchar 只能是单个字符"
    if u'\u4e00' <= uchar <= u'\u9fa5':
        return True
    return False


def is_number(uchar):
    """判断一个字符是否是数字"""
    assert len(uchar) == 1, "uchar 只能是单个字符"
    if u'\u0030' <= uchar <= u'\u0039':
        return True
    return False


def is_alphabet(uchar):
    """判断一个字符是否是英文字母"""
    assert len(uchar) == 1, "uchar 只能是单个字符"
    if (u'\u0041' <= uchar <= u'\u005a') or (u'\u0061' <= uchar <= u'\u007a'):
        return True
    return False


def B2Q(uchar):
    """单字符半角转全角"""
    assert len(uchar) == 1, "uchar 只能是单个字符"
    inside_code = ord(uchar)
    if inside_code < 0x0020 or inside_code > 0x7e:
        # 不是半角字符就返回原来的字符
        return uchar
    if inside_code == 0x0020:
        # 除了空格其他的全角半角的公式为:半角=全角-0xfee0
        inside_code = 0x3000
    else:
        inside_code += 0xfee0
    return chr(inside_code)


def Q2B(uchar):
    """单字符全角转半角"""
    assert len(uchar) == 1, "uchar 只能是单个字符"
    inside_code = ord(uchar)
    if inside_code == 0x3000:
        inside_code = 0x0020
    else:
        inside_code -= 0xfee0
    if inside_code < 0x0020 or inside_code > 0x7e:
        # 转完之后不是半角字符返回原来的字符
        return uchar
    return chr(inside_code)
```

## 11. 提取年-月信息

假设 dataframe 中的 date 列是 datetime64[ns] 类型，提前其中的年月信息，可以这样

```python
data['year'] = data['date'].dt.year
tmp_months = data['date'].dt.month
data['month'] = [str(x).zfill(2) for x in tmp_months]
data['year_month'] = (data['year'].astype(str)
                      + '_'
                      + data['month'])
data.drop(columns=['year', 'month'], inplace=True)
```
这样出来的结果是 `2020_09`，而不是 `2020_9`。

## 12. `pd.concat` 可能会遇到的问题

```python
for loop:
    ...
    a = xxxx   # pd.DataFrame
    a.reset_index(drop=True, inplace=True) 

    b = yyyy   # pd.DataFrame
    b.reset_index(drop=True, inplace=True)

    # pd.concat 之前必须要 reset_index
    a_b = pd.concat([a, b], axis=1)
```

## 13. 判断字符串是否是车辆 vin 码

```python
def check_vin(vin: str) -> bool:
    """检验一个字符串是否是一个标准的vin."""
    # vin码长度为 17
    if vin is None or len(vin) != 17:
        return False
    # I O Q 这 3 个英文字母不应该出现在 vin 中
    if 'I' in vin or 'O' in vin or 'Q' in vin:
        return False

    # vin 中各个字符的对应值
    mapping = {
        'A': 1, 'B': 2, 'C': 3, 'D': 4, 'E': 5, 'F': 6, 'G': 7, 'H': 8, 'I': 0,
        'J': 1, 'K': 2, 'L': 3, 'M': 4, 'N': 5, 'O': 0, 'P': 7, 'Q': 8, 'R': 9,
        'S': 2, 'T': 3, 'U': 4, 'V': 5, 'W': 6, 'X': 7, 'Y': 8, 'Z': 9, '0': 0,
        '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9
    }
    values = [mapping.get(v, 100) for v in vin]
    if 100 in values:  # 防止未知字符出现在vin中
        return False
    
    # vin中每个位置的权重
    weights = [8, 7, 6, 5, 4, 3, 2, 10, 0, 9, 8, 7, 6, 5, 4, 3, 2]

    check_sum = sum([v * w for v, w in zip(values, weights)])
    check_value = check_sum % 11

    v_9th = vin[8]
    if (str(check_value) == v_9th) or (check_value == 10 and v_9th == 'X'):
        return True
    else:
        return False
```
