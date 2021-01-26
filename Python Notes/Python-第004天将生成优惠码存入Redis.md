> **第 0002 题:** 将 0001 题生成的 200 个激活码（或者优惠券）保存到 **Redis** 非关系型数据库中。
>
> 题目来源:https://github.com/Yixiaohan/show-me-the-code

## 题解

1. 本题主要考察Python操作Redis数据库,本例采用redis库进行链接
2. redis的增删改查等基本操作
3. 券码存入redis库也需要考虑该券码是否已经存在

## 参考代码

- redis基本命令

  ```python
  -- 安装redis库
   pip install redis
  import redis   # 导入redis 模块
  
  r = redis.Redis(host='localhost', port=6379, decode_responses=True)  
  r.set('name', 'ceshi')  # 设置name对应的值
  print(r['name'])
  print(r.get('name'))  # 取出键name对应的值
  print(type(r.get('name')))  # 查看类型
  ```

- Python封装Redis工具类

  ```python
  class MyRedisUtil():
      def __init__(self):
          self.conn = None
      # 链接redis
      def connect_redis(self):
          self.conn = redise.Redis(host='127.0.01', port=6379)
      # 添加list数据,从左边新增加lpush(),从右边新增rpush()
      def lpush_redis(self, k_list):
          for key in k_list:
              self.conn.lpush('key',key)
      # 添加集合set数据
      def sadd_redis(self,k_list):
          for key in k_list:
              self.conn.sadd('key',key)
  
      # 读取list数据
      def get_range_redis(self):
          k_list = self.conn.getrange('key',0,-1) # 取所有的字节
          for key in k_list:
              print(key.decode())
      # 读取集合数据
      def smembers(self):
          smembers = self.conn.smembers('key') # 取所有的字节
          for key in smembers:
              print(key.decode())
      # 清空数据
      def flushdb_redis(self):
          self.conn.flushdb()
  
  ```
  
- 本例参考代码

  ```python
  
  ```
  
  

## 存入数据库的数据

![image-20210123214029592](E:\markdown_pic\markdown-pic\typora_pic\image-20210123214029592.png)



## 代码仓库

[我的GitHub地址](https://github.com/tenli/Python_Day_After_Day):https://github.com/tenli/Python_Day_After_Day

## 寄语

种一棵树最好的时间是十年前,其次是现在。

那么,从现在开始,方圆十里的树归我种了。

## 需要学习的

1. MySQL封装需要再熟悉一下
2. 关于优惠券码唯一性和插入数据失败的处理逻辑还没有做