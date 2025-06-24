# 示例代码

[https://gitee.com/fangrentian/basic](https://gitee.com/fangrentian/basic)


# springboot使用SSE提供服务时, 未知原因导致返回的响应头中 `Content-Length` 的值为0,导致客户端无法链接

> 问题是：通过在controller或者filter中都无法删除`Content-Length`, 最终通过`HttpServletResponseWrapper`顺利删除


```java

package hdb.ares.ssedemo.config;

import jakarta.servlet.*;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpServletResponseWrapper;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.io.IOException;

@Configuration
public class SseConfig {

    @Bean
    public FilterRegistrationBean<SseHeaderFilter> sseHeaderFilter() {
        FilterRegistrationBean<SseHeaderFilter> registrationBean = new FilterRegistrationBean<>();

        registrationBean.setFilter(new SseHeaderFilter());
        registrationBean.addUrlPatterns("/wechat/sse");
        registrationBean.setOrder(1); // 设置过滤器顺序

        return registrationBean;
    }


    public static class SseHeaderFilter implements Filter {

        @Override
        public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
                throws IOException, ServletException {

            HttpServletResponse httpResponse = (HttpServletResponse) response;
            // 创建自定义响应包装器
            HttpServletResponseWrapper wrapper = new HttpServletResponseWrapper(httpResponse) {
                @Override
                public void setHeader(String name, String value) {
                    if ("Content-Length".equalsIgnoreCase(name)) {
                        // 拦截并丢弃Content-Length头
                        return;
                    }
                    super.setHeader(name, value);
                }

                @Override
                public void addHeader(String name, String value) {
                    if ("Content-Length".equalsIgnoreCase(name)) {
                        return;
                    }
                    super.addHeader(name, value);
                }
            };

            // 继续执行过滤器链
            chain.doFilter(request, wrapper);
        }
    }
}

```
