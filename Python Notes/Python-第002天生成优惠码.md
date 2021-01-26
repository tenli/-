> 第 0001 题： 做为 Apple Store App 独立开发者，你要搞限时促销，为你的应用生成激活码（或者优惠券），使用 Python 如何生成 200 个激活码（或者优惠券）？
>
> 题目来源:https://github.com/Yixiaohan/show-me-the-code

## 题解

1. 我的理解本题主要考察的是随机函数和字符串函数的使用,当然如果仅仅是生成一定数量的激活码而不考虑激活码的复杂度,也可以使用数字代替
2. 激活码一般需要是随机生成,导入random和string库,使用string库中的ascii_letters和digits组合成字符串,然后使用random随机生成
3. 封装生成激活码的方法,暂时使用列表存储激活码,也是为了查询是否重复(虽然几率很小)
4. 也可以对激活码再次加密处理,本次暂时不做加密了

## 参考代码

```Python
import random
import string


# 定义方法,code_long:激活码长度,code_num
def gen_codes(code_long, code_num):
    # code_long:激活码长度
    # code_num:激活码数量
    # 定义列表,用以存储激活码
    gen_code_list = []
    i = 1
    while i <= code_num:
        # 随机字符串
        gen_code = ''.join(random.sample(string.ascii_letters + string.digits, code_long))
        # 判断是否已经存在
        if gen_code in gen_code_list:
            continue
        else:
            gen_code_list.append(gen_code)
            i += 1


if __name__ == '__main__':
    gen_codes(20, 10)
```

## 生成的激活码

```python
['BJOawkDN6GFLq0jhrYCX', 'pFNyoRA8UqfHiJCY60PZ', '3QDRvof1xKBLt8Z0XhTS', 'TSbcQvjduw4hDZeFask3', 'AZMlWt8SRo6XOqnad51U', 'C6ZEtX5QmnzbaB1Uv4j3', 'smldgc4TpwB0JEqSXrYL', 'op3zjCElPATiVmGRtcZa', '0NntvuQlZ7HRKYT1BCxF', '5FnhmZbVdu0cpvMqJ8HT']
```

## 代码仓库

[我的GitHub地址](https://github.com/tenli/Python_Day_After_Day):https://github.com/tenli/Python_Day_After_Day

## 寄语

种一棵树最好的时间是十年前,其次是现在。

那么,从现在开始,方圆十里的树归我种了。



## 需要学习的

string和random库

https://www.cnblogs.com/zqifa/p/python-random-1.html