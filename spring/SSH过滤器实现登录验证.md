# SSH过滤器实现登录验证

1. 过滤器Filter 1）编写一个过滤器的类实现Filter接口 2）实现接口中尚未实现的方法(着重实现doFilter方法) 3）在web.xml中进行配置(主要是配置要对哪些资源进行过滤

2. 过滤器代码实现 

   ```
   public class AuthFilter implements Filter{
   
       @Override
       public void destroy() {
           
       }
   
       @Override
       public void doFilter(ServletRequest request, ServletResponse response,
               FilterChain chain) throws IOException, ServletException {
           HttpServletResponse resp = (HttpServletResponse)response;
           HttpServletRequest req = (HttpServletRequest)request;
           HttpSession session = req.getSession();
           User user = (User)session.getAttribute("user");
           String uri = req.getRequestURI();
           //简单判断缓存中是否有用户
           if(user==null){//没有用户
               //判断用户是否是选择跳到登录界面
               if(uri.endsWith("login.jsp")||uri.endsWith("login.do")){
                   chain.doFilter(request, response);
               }else{
                   resp.sendRedirect(req.getContextPath()+"/login.jsp");//重定向到login.jsp
               }    
           }else{//有用户
               chain.doFilter(request, response);
           }
       }
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException {
           
       }
   
   }
   ————————————————
   版权声明：本文为CSDN博主「qq_29660549」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
   原文链接：https://blog.csdn.net/qq_29660549/article/details/90233243
   ```

3. web.xml配置 登录认证过滤器 

   ```
   <!-- 登录认证过滤器 -->
        <filter>
            <filter-name>auth</filter-name>
            <filter-class>com.demo.filter.AuthFilter</filter-class>
        </filter>
        <filter-mapping>
            <filter-name>auth</filter-name>
            <url-pattern>*.jsp</url-pattern>
        </filter-mapping>
        <filter-mapping>
            <filter-name>auth</filter-name>
            <url-pattern>*.do</url-pattern>
        </filter-mapping>
   ————————————————
   版权声明：本文为CSDN博主「qq_29660549」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
   原文链接：https://blog.csdn.net/qq_29660549/article/details/90233243
   ```

   