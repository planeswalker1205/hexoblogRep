# Redis

Redis菜鸟教程：https://www.runoob.com/redis/redis-intro.html。

github找资源。

命名规范：搜索阿里：Ali-Check。

**修改端口号**:在redis-windows-conf中设置port端口号，然后连带redis-windows-conf重新启动服务。

```
redis-server.exe redis.windows.conf
```

再次连接服务器需要。

`redis-cli  -h 主机ip  -p  端口  `

当出现NOAUTH authentication required则表示需要密码验证。

`auth root`/`auth 123456`

如果往redis添加List元素，那么他的下标从0开始，从后往前数，

`LPUSH userlist one two three`

`LPUSH userlist four five`

那么此时`LINDEX userlist 0`的值是five.`LINDEX userlist 2`的值为three。

###使用命令行操作redis

cmd--->**netstat** 可以查看所有端口号，检查是否有被占用的端口

指令不要敲中文

1. 开启服务命令:`D:\redis>redis-server`

  `redis-server redis.windows.config`和`D:\redis>redis-server`是两个不同的进程

 <!--(要开启服务命令后重新打开一个cmd才能执行其他命令)-->

2. 连接本地redis服务:`redis-cli`
3. 清空数据库:`flushdb`
4. 查看所有redis存储的键`keys *`

**redis.windows.config可以查看配置文件**

设置redis密码:notepad打开redis.windows.config,搜索requirepass

搜索bind可以设置谁可以访问redis  

搜索port可以设置端口号（本机默认6379）

5. `redis-cli  -h 主机ip  -p  端口  `(具体连接ip和端口号的reids服务)

6. `set pz class11`设置redis的键值，可以`keys *`查看

7. `get pz`查看键为pz的值

8. `set name jack` `set name lili`对于redis来说，修改就是对同一个key值进行覆盖

9. `del name`删除一条键为name的值

10. `exists key`检查key是否存在

11. 设置过期时间`expire key second`key是要设置键，second是过期时间秒

    `ttl key` 可以查看默认过期时间，默认是-1永不过期，-2是已经失效

12. ` pttl name` 查看剩余多少毫秒

ￎￋￌￊﾽﾳ￐￀ﾹ   

13. `INCR key`将key中存储的数字值加1

### 使用Java操作redis

使用jedis

1. 打开Idea

2. 百度maven，搜索jedis

3. pom.xml中redis client依赖

   ```
   <dependency>
     <groupId>redis.clients</groupId>
     <artifactId>jedis</artifactId>
     <version>3.0.1</version>
   </dependency>
   ```

1. main方法new一个jedis

   ```
   public static void main(String[] args) {
       Jedis jedis = new Jedis();
       System.out.println("Jedis实例"+jedis);
       String s = jedis.set("name","jack");
       String s1 = jedis.set("age","18");
       System.out.println(s);
       System.out.println(s1);
       System.out.println("jack的名字是:"+jedis.get("name"));
       System.out.println("jack的年龄是:"+jedis.get("age"));
   
   }
   ```

注:在运行之前一定要先开启redis服务。cmd-----redis-server

5. cmd命令测试

### 手机号验证码三分钟有效

1. 创建web项目

2. pom.xml的redis集成进去

3. 简单的redis工具类

   ```
   public class RedisUtils{
       private static Jedis jedis;
       private static final String HOST = "127.0.0.1";//ip地址
       private static fianl Integer PORT = 6377;//端口号
       private static final String AUTH = "123456";
       static{
           jedis = new Jedis(HOST,PORT);
           jedis.auth(AUTH);
       }
       /**
       *@param key键
       *@return boolean 是否存在
       *判断key是否存在
       */
       public static boolean exists(String key){
           return jedis.exists(key);
       }
       
       public static String set(String key,String value){
            return jedis.set(key,value);
       }
       //设置有效时间的key和value
        public static String setex(String key,String value,int seconds){
            return jedis.set(key,seconds,value);
       }
       //get...
        public static String get(String key){
            return jedis.get(key);
       }
       ...
       //删除
       public static Long del(String key){
           return jedis.del(key);
       }
       
   }
   ```

   

4. 创建controller类

   ```
   @RestController
   public class CodeContro11{
   	//手机号
   	private String phone = "15435435435";
   	@Requestmapping("");
       public String createCode(){
       //六位数字的随机验证码如何创建
       //判断redis是否有该手机号所对应的随机验证码
       	boolean flag = RedisUtils.exists(phone);
       	String code = null;
       	if(!flag){
               for(int i = 0;i<6;i++){
               int random=((int)(Mathrandom()*10));
               code=sb.toString();
           }else{
               code = RedisUtils.getJedis().get(phone);
           }
           RedisUtils.setex(phone,sb.toString(),60);
       	}
       	//通过第三方的短息接口服务API，将手机号和验证码。
           return "验证码发送成功!";
       }
       public String checkCode(String sendCode){
           if(null==sendCode){
               return "请输入验证码";
           }else{
               String redisCode = RedisUtils.get(phone);
               if(sendCode.equals(redisCode)){
               //验证码有有效性，使用完成后应该删除
               RedisUtils.del(phone);
                   return "验证码输入正确";
               }else{
                   return "验证码输入错误";
               }
           }
       }
   }
   ```

   

   

   

   ## nginx

   一般是在linux系统下使用

   反向代理，负载均衡

   win10 下的nginx无法启动，闪退且进程中没有服务，出现的问题是80端口被占用，config里改写端口重启即可。

   1. 启动:

      nginx.exe双击








### SVN

---

Apache Subversion 通常被缩写成 SVN，是一个开放源代码的版本控制系统，Subversion 在 2000 年由 CollabNet Inc 开发，现在发展成为 Apache 软件基金会的一个项目，同样是一个丰富的开发者和用户社区的一部分。

SVN相对于的RCS、CVS，采用了分支管理系统，它的设计目标就是取代CVS。互联网上免费的版本控制服务多基于Subversion。**先更新，再提交。不然会出现版本冲突**

1. 安装一个SVN服务端。所有源代码，项目文档手册资料等等提交svn服务器。---visual SVN，图形界面（服务端），------tortoise svn(客户端)
2. 局域网安装一个svn服务端
3. 安装svn客户端

