# SpringMVC笔记

**1. SpringMVC工作原理（重要）**

客户端发送庆祝-->前端控制器DispaterServlet接受客户端请求-->找到处理器映射HandlerMapping解析请求对应的Handler-->HandlerAdapter会根据Handler来调用真正的处理器开处理请求，并处理相应的业务逻辑-->处理器返回一个模型视图ModelAndView-->视图解析器开始解析-->返回一个视图对象-->前端控制器DispatcherServlet渲染数据（Moder）-->将得到视图对象返回给用户。

**2. 流程说明（重要）**

（1）客户端（浏览器）发送求情，直接请求到DispatcherServlet。

（2）DispatcherServlet根据请求信息调用HandlerMapping，解析请求对应的Handler。

（3）解析到对应的Handler（即controller控制器）后，开始由HandlerAdapter适配器处理。

（4）HandlerAdapter会根据Handler来调用真正的处理器开处理请求，并处理相应的业务逻辑。

（5）处理器处理完业务后，会返回一个 ModelAndView 对象，Model 是返回的数据对象，View 是个逻辑上的 View。

（6）ViewResolver 会根据逻辑 View 查找实际的 View。

（7）DispaterServlet 把返回的 Model 传给 View（视图渲染）。

（8）把 View 返回给请求者（浏览器）

**SpringMVC重要组件说明**

1. **前端控制器DispatcherServlet（不需要工程师开发），由框架提供。**

   作用：SpringMVC的入口函数。接受请求，响应结果，相当于转发器，中央处理器。有了DispatcherServlet减少了其他组件之间的耦合度。用户请求到达前端控制器，它就相当于MVC模式中的C，DispatcherServlet是整个流程控制的中心，由它调用其他组件处理用户的请求，DispatcherServlet的存在降低了组件之间的耦合性。

2. **处理器映射器HandlerMapping（不需要工程师开发），由框架提供。**

   作用：根据请求的url查找Handler。HandlerMapping负责根据用户请求找到Handler即处理器（controller），SpringMVC提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式。

3. **处理器适配器HandlerAdapter**

   作用：按照特定规则（HandlerAdapter要求的规则）去执行Handler。通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

4. **处理器Handler（需要工程师开发）**

   编写Handler时按照HandlerAdapter的要求去做，这样适配器才可以去正确执行Handler。Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。 由于Handler涉及到具体的用户业务请求，所以一般情况需要工程师根据业务需求开发Handler。

5. **视图解析器View resolver（不需要工程师开发），由框架提供**

   作用：进行视图解析，根据逻辑视图名解析成真正的视图（view） View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。 springmvc框架提供了很多的View视图类型，包括：jstlView、freemarkerView、pdfView等。 一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由工程师根据业务需求开发具体的页面。

6. **视图View（需要工程师开发）**

   