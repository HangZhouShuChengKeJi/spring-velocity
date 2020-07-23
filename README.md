# spring-velocity
spring velocity 2.2 版本支持

# 使用
## 引入依赖
```xml
<dependency>
    <groupId>org.yg.spring</groupId>
    <artifactId>springframework-velocity</artifactId>
    <version>0.0.0.1</version>
</dependency>
```

## spring config 配置
```java
package org.yg.spring.springboot.velocity.demo.config;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.velocity.VelocityConfigurer;
import org.springframework.web.servlet.view.velocity.VelocityLayoutViewResolver;

/**
 * @author 小天
 * @date 2020/6/14 14:32
 */
@Configuration
public class WebmvcConfig implements WebMvcConfigurer {

    @Bean
    public VelocityConfigurer velocityConfigurer(ApplicationContext context) {
        VelocityConfigurer velocityConfigurer = new VelocityConfigurer();
        // 模板加载路径（最后的斜杠可有可无）
        velocityConfigurer.setResourceLoaderPath("classpath:templates/");
        // 设置文件系统加载器 "不" 优先（即 spring 的资源加载器优先）
        velocityConfigurer.setPreferFileSystemAccess(false);
        // 使用 spring 的资源加载器
        velocityConfigurer.setResourceLoader(context);
        return velocityConfigurer;
    }

    @Override
    public void configureViewResolvers(ViewResolverRegistry registry) {
        VelocityLayoutViewResolver velocityLayoutViewResolver = new VelocityLayoutViewResolver();
        // 模板文件后缀
        velocityLayoutViewResolver.setSuffix(".vm");
        // 模板文件前缀
        velocityLayoutViewResolver.setPrefix("");
        // 布局模板文件的 key
        velocityLayoutViewResolver.setLayoutKey("layout");
//        // 默认布局模板文件
//        velocityLayoutViewResolver.setLayoutUrl("layout.vm");
        // 工具集合配置
        velocityLayoutViewResolver.setToolboxConfigLocation("toolbox.xml");
        velocityLayoutViewResolver.setContentType("text/html;charset=UTF-8");
        registry.viewResolver(velocityLayoutViewResolver);
    }
}
```

# TODO
+ [BUG] 视图名称 或 layout 或 #parse 以 '/' 开头时，在 centos 上找不到视图

# License
The Spring Framework is released under version 2.0 of the [Apache License](https://www.apache.org/licenses/LICENSE-2.0).
