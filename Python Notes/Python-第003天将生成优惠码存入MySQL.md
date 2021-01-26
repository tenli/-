> **第 0002 题:** 将 0001 题生成的 200 个激活码（或者优惠券）保存到 **MySQL** 关系型数据库中。
>
> 题目来源:https://github.com/Yixiaohan/show-me-the-code

## 题解

1. 本题主要考察Python操作MySQL数据库,本例采用pymysql库进行链接
2. MySQL增删改查的基本操作
3. 券码存入MySQL库也需要考虑该券码是否已经存在

## 参考代码

- 建表

  ```mysql
  -- 创建数据库
  Create databse python-100days;
  -- 切换当前库
  user database;
  -- 建表
  Create table coupon_code(
  id int comment'优惠券ID',
  code varchar(50) comment'优惠券码'
  );
  ```

- Python封装MySQL工具类,这个日后需要好好学习一下面向对象思想

  ```python
  import pymysql
  
  
  class MysqlConnect(object):
      # 魔术方法, 初始化, 构造函数
      def __init__(self, host, user, password, port, database):
          """
          :param host: IP
          :param user: 用户名
          :param password: 密码
          :param port: 端口号
          :param database: 数据库名
          :param charset: 编码格式
          """
          self.db = pymysql.connect(host=host, user=user, password=password, port=port, database=database, charset='utf8')
          self.cursor = self.db.cursor()
  
      # 将要插入的数据写成元组传入
      def exec_data(self, sql, data=None):
          # 执行SQL语句
          self.cursor.execute(sql, data)
          # 提交到数据库执行
          self.db.commit()
  
      # sql拼接时使用repr()，将字符串原样输出
      def exec(self, sql):
          self.cursor.execute(sql)
          # 提交到数据库执行
          self.db.commit()
  
      def select(self, sql):
          self.cursor.execute(sql)
          # 获取所有记录列表
          results = self.cursor.fetchall()
          for row in results:
              print(row)
  
      # 魔术方法, 析构化 ,析构函数
      def __del__(self):
          self.cursor.close()
          self.db.close()
  
  
  if __name__ == '__main__':
      mc = MysqlConnect('192.168.5.36', 'root', 'aa111111', 3306, 'wdd_test')
      # mc.exec('insert into test(id, text) values(%s, %s)' % (1, repr('哈送到附近')))
      mc.exec_data('insert into test_user values(%s, %s)' % (1, repr('哈送到附近')))
      # mc.exec_data('insert into test(id, text) values(%s, %s)',(13, '哈送到附近'))
      mc.select('select * from test')
  
  ```

- 本例参考代码

  ```python
  """
  第 0002 题: 将 0001 题生成的 200 个激活码（或者优惠券）保存到 MySQL 关系型数据库中。
  """
  import random
  import string
  from Python日复一日.Python_Day_After_Day.MySQL_Connect import MysqlConnect
  
  
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
      # print(gen_code_list)
      return gen_code_list
  
  
  def save_into_da():
      db = MysqlConnect('192.168.5.36', 'root', 'aa111111', 3306, 'wdd_test')
      code_list = gen_codes(10, 10)
      i = 0
      while i < len(code_list):
          # 拼接SQL
          sql = 'insert into test_user(name) values("'"{0}"'")'.format(code_list[i])
          # 执行sql语句
          db.exec(sql)
          i += 1
  
  
  if __name__ == '__main__':
      save_into_da()
  
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