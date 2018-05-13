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
                    model.addAttribute("student",s);
                    return "index";
                }
            }  
