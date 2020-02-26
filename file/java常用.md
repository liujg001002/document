###将jar包打入本地仓库
>mvn install:install-file -DgroupId=org.xl.brave   -DartifactId=brave-axis2 -Dversion=0.0.1  -Dfile=C:\Users\sy\Desktop\brave-axis2-0.0.1-SNAPSHOT.jar    -Dpackaging=jar

###多线程远程feign调用请求丢失解决办法
###开启新线程之前，添加代码：
###将RequestAttributes对象设置为子线程共享
>ServletRequestAttributes sra = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
RequestContextHolder.setRequestAttributes(sra, true);

###entity常用注解(字段如果为空是不会走@length,@patten等验证,判断 空需要组合使用)
>@Null  被注释的元素必须为null  
@NotNull  被注释的元素不能为null  
@AssertTrue  被注释的元素必须为true  
@AssertFalse  被注释的元素必须为false  
@Min(value)  被注释的元素必须是一个数字，其值必须大于等于指定的最小值  
@Max(value)  被注释的元素必须是一个数字，其值必须小于等于指定的最大值  
@DecimalMin(value)  被注释的元素必须是一个数字，其值必须大于等于指定的最小值  
@DecimalMax(value)  被注释的元素必须是一个数字，其值必须小于等于指定的最大值  
@Size(max,min)  被注释的元素的大小必须在指定的范围内。  
@Digits(integer,fraction)  被注释的元素必须是一个数字，其值必须在可接受的范围内  
@Past  被注释的元素必须是一个过去的日期  
@Future  被注释的元素必须是一个将来的日期  
@Pattern(value) 被注释的元素必须符合指定的正则表达式。  
@Email 被注释的元素必须是电子邮件地址  
@Length 被注释的字符串的大小必须在指定的范围内  
@NotEmpty  被注释的字符串必须非空  
@Range  被注释的元素必须在合适的范围内  
@Valid 用于注释元素为自定义对象(controller 参数 dto中实体)  
@Validated  用于controller 实体对象上（提示改实体需要进行校验）

###解决feign 调用失败
>@RequestMapping(value = "/generate/password", method = RequestMethod.POST, consumes = MediaType.APPLICATION_JSON_VALUE)
###long返回前端转string
>@JSONField(serializeUsing= ToStringSerializer.class)

###增强的controller注解@ControllerAdvice
* [地址](https://www.cnblogs.com/lenve/p/10748453.html)
* 全局异常处理
* 全局数据绑定
* 全局数据预处理

###Date 日期格式化传递
>@JsonFormat(timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")  
@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")

#获取handlerUrl
>private String getHandlerUrl(HttpServletRequest request) {  
    String servletPath = request.getServletPath();  
    Map pathVariables = (Map)request.getAttribute(HandlerMapping.URI_TEMPLATE_VARIABLES_ATTRIBUTE);  
    for (Object key : pathVariables.keySet()) {  
        String pKey = new StringBuffer("/").append(pathVariables.get(key)).toString();  
        String pValue = new StringBuffer("/{").append(key).append("}").toString();  
        servletPath = servletPath.replace(pKey, pValue);  
    }  
    log.info("--------------------> handler: {}", servletPath);  
    return servletPath;  
}
#获取项目中controller uri
>@Resource  
private BeanFactory beanFactory;  
private String getAllRequestMappingInfo() {  
        AbstractHandlerMethodMapping<RequestMappingInfo> objHandlerMethodMapping = (AbstractHandlerMethodMapping<RequestMappingInfo>) beanFactory.getBean("requestMappingHandlerMapping");  
        Map<RequestMappingInfo, HandlerMethod> mapRet = objHandlerMethodMapping.getHandlerMethods();  
        Set<RequestMappingInfo> requestMappingInfos = mapRet.keySet();  
        for (RequestMappingInfo requestMappingInfo : requestMappingInfos) {  
            PatternsRequestCondition patternsCondition = requestMappingInfo.getPatternsCondition();  
            Set<String> patterns = patternsCondition.getPatterns();  
            RequestMethodsRequestCondition methodsCondition = requestMappingInfo.getMethodsCondition();  
            Set<RequestMethod> methods = methodsCondition.getMethods();  
            System.out.println(patterns.toString() +" --  "+ methods.toString());  
        }  
        return "test ok";  
}