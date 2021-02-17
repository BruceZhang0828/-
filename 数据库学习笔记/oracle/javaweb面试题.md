1. JSP 和 servlet 有什么区别？
JSP 是 servlet 技术的扩展，本质上就是 servlet 的简易方式。servlet 和 JSP 最主要的不同点在于，servlet 的应用逻辑是在 Java 文件中，并且完全从表示层中的 html 里分离开来，而 JSP 的情况是 Java 和 html 可以组合成一个扩展名为 JSP 的文件。JSP 侧重于视图，servlet 主要用于控制逻辑。
2. JSP 有哪些内置对象？作用分别是什么？
JSP 有 9 大内置对象：
· request：封装客户端的请求，其中包含来自 get 或 post 请求的参数；
· response：封装服务器对客户端的响应；
· pageContext：通过该对象可以获取其他对象；
· session：封装用户会话的对象；
· application：封装服务器运行环境的对象；
· out：输出服务器响应的输出流对象；
· config：Web 应用的配置对象；
· page：JSP 页面本身（相当于 Java 程序中的 this）；
· exception：封装页面抛出异常的对象。
3. 说一下 JSP 的 4 种作用域？
· page：代表与一个页面相关的对象和属性。
· request：代表与客户端发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个 Web 组件；需要在页面显示的临时数据可以置于此作用域。
· session：代表与某个用户与服务器建立的一次会话相关的对象和属性。跟某个用户相关的数据应该放在用户自己的 session 中。
· application：代表与整个 Web 应用程序相关的对象和属性，它实质上是跨越整个 Web 应用程序，包括多个页面、请求和会话的一个全局作用域。
4. session 和 cookie 有什么区别？
· 存储位置不同：session 存储在服务器端；cookie 存储在浏览器端。
· 安全性不同：cookie 安全性一般，在浏览器存储，可以被伪造和修改。
· 容量和个数限制：cookie 有容量限制，每个站点下的 cookie 也有个数限制。
· 存储的多样性：session 可以存储在 Redis 中、数据库中、应用程序中；而 cookie 只能存储在浏览器中。
5. 说一下 session 的工作原理？
session 的工作原理是客户端登录完成之后，服务器会创建对应的 session，session 创建完之后，会把 session 的 id 发送给客户端，客户端再存储到浏览器中。这样客户端每次访问服务器时，都会带着 sessionid，服务器拿到 sessionid 之后，在内存找到与之对应的 session 这样就可以正常工作了。