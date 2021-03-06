所谓SQL注入式攻击，就是攻击者把SQL命令插入到Web表单的输入域或页面请求的查询字符串，欺骗服务器执行恶意的SQL命令。
在某些表单中，用户输入的内容直接用来构造（或者影响）动态SQL命令，或作为存储过程的输入参数，这类表单特别容易受到SQL注入式攻击。

如何防止SQL注入？
---
### 对于动态构造SQL查询的场合，可以使用下面的技术：

第一：替换单引号，即把所有单独出现的单引号改成两个单引号，防止攻击者修改SQL命令的含义。再来看前面的例子，"SELECT * from Users WHERE login = ''' or ''1''=''1' AND password = ''' or ''1''=''1'"显然会得到与"SELECT * from Users WHERE login = '' or '1'='1' AND password = '' or '1'='1'"不同的结果。

第二：删除用户输入内容中的所有连字符，防止攻击者构造出类如"SELECT * from Users WHERE login = 'mas' -- AND password =''"之类的查询，因为这类查询的后半部分已经被注释掉，不再有效，攻击者只要知道一个合法的用户登录名称，根本不需要知道用户的密码就可以顺利获得访问权限。

第三：对于用来执行查询的数据库帐户，限制其权限。用不同的用户帐户执行查询、插入、更新、删除操作。由于隔离了不同帐户可执行的操作，因而也就防止了原本用于执行SELECT命令的地方却被用于执行INSERT、UPDATE或DELETE命令。

### 用存储过程来执行所有的查询。
SQL参数的传递方式将防止攻击者利用单引号和连字符实施攻击。此外，它还使得数据库权限可以限制到只允许特定的存储过程执行，所有的用户输入必须遵从被调用的存储过程的安全上下文，这样就很难再发生注入式攻击了。

### 限制表单或查询字符串输入的长度。
如果用户的登录名字最多只有10个字符，那么不要认可表单中输入的10个以上的字符，这将大大增加攻击者在SQL命令中插入有害代码的难度。

### 检查用户输入的合法性，确信输入的内容只包含合法的数据。
数据检查应当在客户端和服务器端都执行——之所以要执行服务器端验证，是为了弥补客户端验证机制脆弱的安全性。

