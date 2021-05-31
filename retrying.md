# retrying

### 思维导图

![](images\retrying.png)

### 安装

```python
pip install retrying
```

### 特征

- 通用装饰器 API
- 指定停止条件（重试次数限制）
- 指定等待条件（重试之间睡眠）
- 自定义异常重试
- 根据预期的返回结果自定义重试

### 使用

#### 装饰：

装饰重试函数：`@retry`

```python
from retrying import retry

@retry
def do_something_unreliable():
    if random.randint(0,10)>1:
        raise IOError('Broken sauce, everything is hosed!!111one')
    else:
        return 'Awesime sauce!'
```

#### 参数：

- ***stop_max_attempt_number***：最大重试次数（default=5）。超出次数抛出异常
- ***stop_max_delay***：最大重试时间。超出时间抛出异常
- ***stop_func***：自定义停止条件。需传递 *previous_attempt_number*、*delay_since_first_attempt_ms* 参数，并返回布尔值
- ***wait_fixed***：两次重试之间等待间隔
- ***wait_random_min*** / ***wait_random_max***：随机等待
- ***wait_incrementing_start*** / ***wait_incrementing_increment***：增量等待。等待时间为 `wait = start + incre*(attempt_number-1)`
- ***wait_exponential_multiplier*** / ***wait_exponential_max***：指数等待。等待时间 `wait = mult*(2**attempt_number)`
- ***wait_func***：自定义等待时间。需传递 *previous_attempt_number*、*delay_since_first_attempt_ms* 参数，并返回数值
- ***wait_jitter_max***：抖动等待最大值。在上述 wait 的基础上随机等待0-1倍 *wait_jitter_max* 时间，等待时间为 `wait+(0,1)*jitter`
- ***retry_on_exception***：指定自定义异常进行重试，否则直接抛出异常
- ***wrap_exception***：根据需求是否使用 `RetryError` 包裹异常
- ***retry_on_result***：根据预期的结果自定义重试。返回 True 时进行重试，否则返回结果

#### 解析：

##### 1、***stop_max_attempt_number***

- 最大重试次数，不传时默认值为 5，超出次数则抛出异常。
- *stop* 和 *stop_func* 参数存在时，该参数无效
- 与 *stop_max_delay* 同时存在时，符合一条就抛出异常
- *stop* / *stop_func* /  *stop_max_delay* / *stop_max_attempt_number* 都未设置时，会一直重试

```python
from retrying import retry

@retry(stop_max_attempt_number=3)
def stop_after_3_attempts():
    raise Exception

print(stop_after_3_attempts())
```

结果：

![image-20200427170315652](images\image-20200427170315652.png)

##### 2、***stop_max_delay***

- 最大重试时间，不传时默认值100ms，超出则抛出异常
- *stop* 和 *stop_func* 参数存在时，该参数无效
- 与 *stop_max_attempt_number* 同时存在时，符合一条就抛出异常
- *stop* / *stop_func* /  *stop_max_delay* / *stop_max_attempt_number* 都未设置时，会一直重试

```python
from retrying import retry

@retry(stop_max_delay=30)
def stop_after_30_ms():
    time.sleep(0.01)
    raise Exception

print(stop_after_30_ms())
```

结果：

![image-20200427170315652](images\image-20200427170315652.png)

##### 3、***stop_func***

- 自定义停止重试函数，参数存在时其他 stop 无效
- 须接收 *previous_attempt_number* 、*delay_since_first_attempt_ms* 参数
- 返回值为布尔值
- *stop* / *stop_func* /  *stop_max_delay* / *stop_max_attempt_number* 都未设置时，会一直重试

 ```python
from retrying import retry

def stop_func(attempt, delay):
    if attempt ==2:
        return True
    
@retry(stop_func=stop_func)
def stop_after_func():
    raise Exception

print(stop_after_func())
 ```

结果：

![image-20200427170958764](images\image-20200427170958764.png)

##### 4、***wait_fixed***

- 重试间隔，未设置时默认为1000ms
- *wait* 和 *wait_func* 参数存在时，该参数无效
- 多个等待时，等待时间取最大值 max

```python
import time
import random
from retrying import retry

@retry(wait_fixed=2000)
def wait_2_s():
    print(time.time())
    if random.randint(0,10)>2:
        raise Exception('随机数不能大于2')
    else:
        return 2

print(wait_2_s())
```

结果：

![image-20200427171736318](images\image-20200427171736318.png)

##### 5、***wait_random_min*** / ***wait_random_max***

- 随即等待 `random.randint(min, max)`，未设置时默认`0/1000`

- *wait* 和 *wait_func* 参数存在时，该参数无效
- 多个等待时，等待时间取最大值 max

```python
import time
import random
from retrying import retry

@retry(wait_random_min=0, wait_random_max=1000)
def wait_random():
    print(time.time())
    if random.randint(0,10)>2:
        raise Exception('随机数不能大于2')
    else:
        return 2

print(wait_random())
```

结果：

![image-20200427172520051](images\image-20200427172520051.png)

##### 6、***wait_incrementing_start*** / ***wait_incrementing_increment***

- 增量等待，未设置时默认为 `0/100`，等待时间为 `start + incre*(attempt_number-1)`
- *wait* 和 *wait_func* 参数存在时，该参数无效
- 多个等待时，等待时间取最大值 max

```python
import time
import random
from retrying import retry

@retry(wait_incrementing_start=0, wait_incrementing_increment=1000)
def wait_increament_1000():
    print(time.time())
    if random.randint(0,10)>2:
        raise Exception('随机数不能大于2')
    else:
        return 2

print(wait_increament_1000())
```



结果：

![image-20200427174053781](images\image-20200427174053781.png)

##### 7、***wait_exponential_multiplier*** / ***wait_exponential_max***

- 指数等待。未设置时默认为`1/1073741823`，等待时间为`wait = mult*(2**attempt_number)`
- 超出 *wait_exponential_max* ，等待时间为 *wait_exponential_max* 

- *wait* 和 *wait_func* 参数存在时，该参数无效
- 多个等待时，等待时间取最大值 max

```python
import time
import random
from retrying import retry

@retry(wait_exponential_multiplier=100)
def wait_exponential_100():
    print(time.time())
    if random.randint(0,10)>2:
        raise Exception('随机数不能大于2')
    else:
        return 2

print(wait_exponential_100())
```

结果：

![image-20200427175049163](images\image-20200427175049163.png)

##### 8、***wait_func***

- 自定义等待函数，参数存在时，其他 wait 无效
- 须接受 *previous_attempt_number* 、*delay_since_first_attempt_ms* 参数
- 返回值为数值

```python
import time
import random
from retrying import retry

def wait_func(attempt, delay):
    return 2000 if attempt==2 else 0

@retry(wait_func=wait_func)
def wait_after_func():
    print(time.time())
    if random.randint(0,10)>2:
        raise Exception('随机数不能大于2')
    else:
        return 2

print(wait_after_func())
```

结果：

![image-20200427175948251](images\image-20200427175948251.png)

##### 9、***wait_jitter_max***

- 抖动等待最大值，未设置时默认为0
- 在 wait 的基础上随机等待 0-1 倍 *wait_jitter_max* ，等待时间为 `wait+(0,1)*wait_jitter_max`

```python
import time
import random
from retrying import retry

@retry(wait_jitter_max=2000)
def wait_jitter_max_2000():
    print(time.time())
    if random.randint(0,10)>2:
        raise Exception('随机数不能大于2')
    else:
        return 2

print(wait_jitter_max_2000())
```

结果：

![image-20200428111130970](images\image-20200428111130970.png)

##### 10、***retry_on_exception***

- 自定义异常重试
- 须接收 *exception* 参数，并且返回值需支持运算符 ”|“，否则抛出 `TypeError`异常
- 返回结果为 True，则尝试重试直到 stop 抛出异常；否则直接抛出异常

```python
import time
import random
from retrying import retry

@retry(stop_max_attempt_number=5, retry_on_exception=lambda x:isinstance(x, IOError))
def retry_on_io_except():
    print(time.time())
    raise IOError

print(retry_on_io_except())
```

结果为 True，重试直到达到停止条件，抛出异常

![image-20200428114836969](images\image-20200428114836969.png)

```python
import time
import random
from retrying import retry

@retry(stop_max_attempt_number=5, retry_on_exception=lambda x:isinstance(x, IOError))
def retry_on_io_except():
    print(time.time())
    raise IndexError

print(retry_on_io_except())
```

结果为 False，直接抛出异常

![image-20200428114603143](images\image-20200428114603143.png)

##### 11、***wrap_exception***

- 抛出异常时是否使用 `RertyError ` 类包裹异常，默认 False

```python
import time
import random
from retrying import retry

@retry(stop_max_attempt_number=2, wrap_exception=True)
def retry_wrap_exception():
    print(time.time())
    raise IOError

print(retry_wrap_exception())
```

结果：

![image-20200428115357439](images\image-20200428115357439.png)

##### 12、***retry_on_result***

- 根据返回的结果(无异常时)自定义是否重试
- 需接收 *result* 参数
- 返回 True 尝试重试直到停止，否则直接退出

```python
import time
import random
from retrying import retry

@retry(retry_on_result=lambda x:1 if x in [1,2,3,4,5] else 0)
def retry_on_12345():
    number = random.randint(0,10)
    return number

print(retry_on_12345())
```

结果：

![image-20200428133953387](images\image-20200428133953387.png)

### 源码

