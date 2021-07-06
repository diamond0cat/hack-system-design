
![20210705171418](https://raw.githubusercontent.com/corykingsf/hack-system-design-pixel/main/pictures/20210705171418.png)


session一般用于识别用户身份：用户登录以后再session里面为用户创建authentication token又叫sessin token,用于识别用户登录了没有，如果登录了识别是哪个用户, session table(存user_id和authentication token可以在session table通过authenticaiton token查user id)

cookie存在客户端，容易被篡改。cookie长度有限制一般存根身份信息有关的内容, cookie是key-value存的

csrf token:在所有的post请求里带一个csrf token,用于对付csrf攻击

https://zhuanlan.zhihu.com/p/22521378

![20210705172842](https://raw.githubusercontent.com/corykingsf/hack-system-design-pixel/main/pictures/20210705172842.png)


