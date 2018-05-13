# pring-boot使用thymeleaf模板

1. 在pom.xml加入依赖, 更新:

            <dependency>  
                     <groupId>org.springframework.boot</groupId>  
                     <artifactId>spring-boot-starter-thymeleaf</artifactId>  
            </dependency>
      
2. 在开发的时候把缓存关闭，只需要在application.properties进行配置即可：

          spring.thymeleaf.prefix=classpath:/templates/  
          spring.thymeleaf.suffix=.html  
          spring.thymeleaf.mode=HTML5  
          spring.thymeleaf.encoding=UTF-8  
          spring.thymeleaf.content-type=text/html  
          # set to false for hot refresh  
          spring.thymeleaf.cache=false  
    
 3. 编写模板文件resouces/templates/index.html
 
             <!DOCTYPE HTML>
            <html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org" >
            <head>
                <meta content="text/html;charset=UTF-8"/>
                <meta name="viewport" content="width=device-width,initial-scale=1"/>
            </head>
            <body>
            <div >
                <div>
                    <span th:text="${student.name}">unknow</span>
                </div>
            </div>
            </body>
            </html>
   
 4. 编写Controller
 
 
                   /**
                   这里使用@Controller而不用@RESTController是因为这里返回一个页面而不是一个值，
                   如果只是使用@RestController注解Controller，则Controller中的方法无法返回jsp页面，
                   配置的视图解析器InternalResourceViewResolver不起作用，
                   返回的内容就是Return 里的内容。
                   */
                           @Controller  
                          publicclass ViewController {  

                           @RequestMapping(value = "/index")
                            public String index(Model model)
                            {
                                Students s  = new Students(20, "roger", "man");
                                 // 加入一个属性，用来在模板中读取
                                model.addAttribute("student",s);
                                return "index";
                            }
                        }  

总结:
一、资源文件的约定目录结构 

            Maven的资源文件目录：/src/java/resources 
            spring-boot项目静态文件目录：/src/java/resources/static 
            spring-boot项目模板文件目录：/src/java/resources/templates 
            spring-boot静态首页的支持，即index.html放在以下目录结构会直接映射到应用的根目录下：
                        classpath:/META-INF/resources/index.html    
                        classpath:/resources/index.html    
                        classpath:/static/index.html    
                        calsspath:/public/index.html   

在spring-boot下，默认约定了Controller试图跳转中thymeleaf模板文件的的前缀prefix是”classpath:/templates/”,后缀suffix是”.html” 
这个在application.properties配置文件中是可以修改的。 如下配置可以修改试图跳转的前缀和后缀

            spring.thymeleaf.prefix: /templates/    
            spring.thymeleaf.suffix: .html    
 
有关thymeleaf中的默认这是可以查看org.springframework.boot.autoconfigure.thymeleaf.ThymeleafProperties这个类的属性 

二、在html页面中引入thymeleaf命名空间，即<html xmlns:th=http://www.thymeleaf.org></html>，此时在html模板文件中动态的属性使用th:命名空间修饰 

三、引用静态资源文件，比如CSS和JS文件，语法格式为“@{}”，如@{/js/blog/blog.js}会引入/static目录下的/js/blog/blog.js文件 

四、访问spring-mvc中model的属性，语法格式为“${}”，如${user.id}可以获取model里的user对象的id属性 

五、循环，在html的标签中，加入th:each=“value:${list}”形式的属性，如"<span th:each=”user:${users}”></span>"可以迭代users的数据 


ref: [usingthymeleaf.html](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#what-is-thymeleaf)
