title: SpringSecurity源码分析-初始化FilterChainProx
date: 2016-04-04 14:53:57
tags: Spring Security
---

闲来无事，看了一下SpringSecurity源码记录这里。

## 入口

使用SpringSecurity首先会在web.xml中配置一个Filter。
```xml
<filter>  
    <filter-name>springSecurityFilterChain</filter-name>  
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>  
</filter>  
<filter-mapping>  
    <filter-name>springSecurityFilterChain</filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>
```

 这个Filter是spring-web包下的，所以这个Filter其实并不是secrutiy自己的。看下这个类的
```java
public class DelegatingFilterProxy extends GenericFilterBean
```

因为它的父类实现了Filter接口，所以它本身就是一个Filter，看它的doFilter 
```java
@Override
public void doFilter(ServletRequest request, ServletResponse response, FilterChain                    filterChain)
	throws ServletException, IOException {
// Lazily initialize the delegate if necessary.
Filter delegateToUse = this.delegate;
if (delegateToUse == null) {
	synchronized (this.delegateMonitor) {
		if (this.delegate == null) {
			WebApplicationContext wac = findWebApplicationContext();
			if (wac == null) {
				throw new IllegalStateException("No WebApplicationContext found: " +
						"no ContextLoaderListener or DispatcherServlet registered?");
			}

			//获取filterchain
			this.delegate = initDelegate(wac);
		}
		delegateToUse = this.delegate;
	}
}
		// filterchain调用doFilter方法
		invokeDelegate(delegateToUse, request, response, filterChain);
	}
```

看this.delegate = initDelegate(wac);
```java
protected Filter initDelegate(WebApplicationContext wac) throws ServletException {
   //根据beanName获取Filter实例，这个beanName就是web中配置的springSecurityFilterChain
	Filter delegate = wac.getBean(getTargetBeanName(), Filter.class);
	if (isTargetFilterLifecycle()) {
		delegate.init(getFilterConfig());
	}
	return delegate;
}
```

那么这个id为springSecurityFilterChain的bean在哪里创建的呢，我们也没有手动在spring配置文件中配置。接下来------>

## Spring的自定义标签

spring配置文件中会有各种各样的标签，常见的`<bean> <aop> <mvc>`,security也有自己的标签如`<http> <security>`,那么这些标签到到底是由什么作用，spring是以怎么样的机制解析这些标签的。

> **Spring的xml文件中，一个命名空间，提供一个解析器**,在META-INF目录下提供spring.handlers与spring.schemas两个文件，.schemas里边存放.xsd配置，.handlers存放命名空间url与解析该标签解析器的对应关系


![http://7xire1.com1.z0.glb.clouddn.com/spring-security1.png](http://7xire1.com1.z0.glb.clouddn.com/spring-security1.png)


> Spring提供了一些接口其中一个NamespaceHandler，源码如下:

```java
public interface NamespaceHandler {
    //初始化
    void init();
    //解析
    BeanDefinition parse(Element element, ParserContext parserContext);
    //装饰
    BeanDefinitionHolder decorate(Node source, BeanDefinitionHolder definition, ParserContext           parserContext);
}
```

> Security的SecurityNamespaceHandler实现了该接口


```java
public void init() {
       //加载各解析器到map中
	  loadParsers();
	}

@SuppressWarnings("deprecation")
private void loadParsers() {
	// Parsers
	parsers.put(Elements.LDAP_PROVIDER, new LdapProviderBeanDefinitionParser());
	parsers.put(Elements.LDAP_SERVER, new LdapServerBeanDefinitionParser());
	parsers.put(Elements.LDAP_USER_SERVICE, new LdapUserServiceBeanDefinitionParser());
	parsers.put(Elements.USER_SERVICE, new UserServiceBeanDefinitionParser());
	parsers.put(Elements.JDBC_USER_SERVICE, new JdbcUserServiceBeanDefinitionParser());
	parsers.put(Elements.AUTHENTICATION_PROVIDER,
			new AuthenticationProviderBeanDefinitionParser());
	parsers.put(Elements.GLOBAL_METHOD_SECURITY,
			new GlobalMethodSecurityBeanDefinitionParser());
	parsers.put(Elements.AUTHENTICATION_MANAGER,
			new AuthenticationManagerBeanDefinitionParser());
	parsers.put(Elements.METHOD_SECURITY_METADATA_SOURCE,
			new MethodSecurityMetadataSourceBeanDefinitionParser());

	// Only load the web-namespace parsers if the web classes are available
	//查看是否有必要的类，如果没有便不加载这些解析器
	if (ClassUtils.isPresent(FILTER_CHAIN_PROXY_CLASSNAME, getClass()
			.getClassLoader())) {
		parsers.put(Elements.DEBUG, new DebugBeanDefinitionParser());
		parsers.put(Elements.HTTP, new HttpSecurityBeanDefinitionParser());
		parsers.put(Elements.HTTP_FIREWALL, new HttpFirewallBeanDefinitionParser());
		parsers.put(Elements.FILTER_SECURITY_METADATA_SOURCE,
				new FilterInvocationSecurityMetadataSourceParser());
		parsers.put(Elements.FILTER_CHAIN, new FilterChainBeanDefinitionParser());
		filterChainMapBDD = new FilterChainMapBeanDefinitionDecorator();
	}

	if (ClassUtils.isPresent(MESSAGE_CLASSNAME, getClass().getClassLoader())) {
		parsers.put(Elements.WEBSOCKET_MESSAGE_BROKER,
				new WebSocketMessageBrokerSecurityBeanDefinitionParser());
	}
}
```


> 初始化时，会将对应标签解析器加载，如http标签解析器是HttpSecurityBeanDefinitionParser


已经加载了相应解析器，接下来就是解析了：
```java
public BeanDefinition parse(Element element, ParserContext pc) {
	if (!namespaceMatchesVersion(element)) {
		pc.getReaderContext()
				.fatal("You cannot use a spring-security-2.0.xsd or spring-security-3.0.xsd or spring-security-3.1.xsd schema "
						+ "with Spring Security 3.2. Please update your schema declarations to the 3.2 schema.",
						element);
	}
	//根据标签获取，解析器map中对应key
	String name = pc.getDelegate().getLocalName(element);
	//获取对应解析器
	BeanDefinitionParser parser = parsers.get(name);

	if (parser == null) {
		// SEC-1455. Load parsers when required, not just on init().
		loadParsers();
	}

	if (parser == null) {
		if (Elements.HTTP.equals(name)
				|| Elements.FILTER_SECURITY_METADATA_SOURCE.equals(name)
				|| Elements.FILTER_CHAIN_MAP.equals(name)
				|| Elements.FILTER_CHAIN.equals(name)) {
			reportMissingWebClasses(name, pc, element);
		}
		else {
			reportUnsupportedNodeType(name, pc, element);
		}

		return null;
	}
    //调用解析器解析
	return parser.parse(element, pc);
}
```

> 以http标签为例，HttpSecurityBeanDefinitionParser解析器详情:

```java
@SuppressWarnings({ "unchecked" })
public BeanDefinition parse(Element element, ParserContext pc) {
  
	CompositeComponentDefinition compositeDef = new CompositeComponentDefinition(
			element.getTagName(), pc.extractSource(element));
	//将http组件放入到parserContext堆中
	pc.pushContainingComponent(compositeDef);
    //注册FilterChain
	registerFilterChainProxyIfNecessary(pc, pc.extractSource(element));

	// 获取其他filterchains
	BeanDefinition listFactoryBean = pc.getRegistry().getBeanDefinition(
			BeanIds.FILTER_CHAINS);
	List<BeanReference> filterChains = (List<BeanReference>) listFactoryBean
			.getPropertyValues().getPropertyValue("sourceList").getValue();
    //为filterchains添加fiterchain
	filterChains.add(createFilterChain(element, pc));
    //注册组件
	pc.popAndRegisterContainingComponent();
	return null;
}
```

> 注册filterchainProxy


```java
static void registerFilterChainProxyIfNecessary(ParserContext pc, Object source) {
    //如果已有filterchainproxy，直接返回
	if (pc.getRegistry().containsBeanDefinition(BeanIds.FILTER_CHAIN_PROXY)) {
		return;
	}
	//创建一个listFactoryBean实例
	BeanDefinition listFactoryBean = new RootBeanDefinition(ListFactoryBean.class);
	//添加一个sourceList属性
	listFactoryBean.getPropertyValues().add("sourceList", new ManagedList());
	pc.registerBeanComponent(new BeanComponentDefinition(listFactoryBean,
			BeanIds.FILTER_CHAINS));
    //创建一个filterchainproxy实例
	BeanDefinitionBuilder fcpBldr = BeanDefinitionBuilder
			.rootBeanDefinition(FilterChainProxy.class);
	fcpBldr.getRawBeanDefinition().setSource(source);
	fcpBldr.addConstructorArgReference(BeanIds.FILTER_CHAINS);
	fcpBldr.addPropertyValue("filterChainValidator", new RootBeanDefinition(
			DefaultFilterChainValidator.class));
	BeanDefinition fcpBean = fcpBldr.getBeanDefinition();
	//注册filterchainproxy bean
	pc.registerBeanComponent(new BeanComponentDefinition(fcpBean,
			BeanIds.FILTER_CHAIN_PROXY));
	//为filterchainproxy bean 注册别名 springSecurityFilterChain
	pc.getRegistry().registerAlias(BeanIds.FILTER_CHAIN_PROXY,
			BeanIds.SPRING_SECURITY_FILTER_CHAIN);
}
```

>创建filterchain


```java
/**
 * 通过http标签创建filterchain
 */
private BeanReference createFilterChain(Element element, ParserContext pc) {
	boolean secured = !OPT_SECURITY_NONE.equals(element.getAttribute(ATT_SECURED));
    //如果security != none
	if (!secured) {
	    //如果没有pattern,request-matcher-ref配置，直接抛出异常
		if (!StringUtils.hasText(element.getAttribute(ATT_PATH_PATTERN))
				&& !StringUtils.hasText(ATT_REQUEST_MATCHER_REF)) {
			pc.getReaderContext().error(
					"The '" + ATT_SECURED
							+ "' attribute must be used in combination with"
							+ " the '" + ATT_PATH_PATTERN + "' or '"
							+ ATT_REQUEST_MATCHER_REF + "' attributes.",
					pc.extractSource(element));
		}
        //如果http子标签包含http标签，抛出异常
		for (int n = 0; n < element.getChildNodes().getLength(); n++) {
			if (element.getChildNodes().item(n) instanceof Element) {
				pc.getReaderContext().error(
						"If you are using <http> to define an unsecured pattern, "
								+ "it cannot contain child elements.",
						pc.extractSource(element));
			}
		}
        // 创建并生成fiterchain
		return createSecurityFilterChainBean(element, pc, Collections.emptyList());
	}

	final BeanReference portMapper = createPortMapper(element, pc);
	final BeanReference portResolver = createPortResolver(portMapper, pc);
    
	ManagedList<BeanReference> authenticationProviders = new ManagedList<BeanReference>();
	//创建权限管理,根据authentication-manager-ref属性
	BeanReference authenticationManager = createAuthenticationManager(element, pc,
			authenticationProviders);

	boolean forceAutoConfig = isDefaultHttpConfig(element);
	/**创建http相关createCsrfFilter();
		createSecurityContextPersistenceFilter();
		createSessionManagementFilters();
		createWebAsyncManagerFilter();
		createRequestCacheFilter();
		createServletApiFilter(authenticationManager);
		createJaasApiFilter();
		createChannelProcessingFilter();
		createFilterSecurityInterceptor(authenticationManager);
		createAddHeadersFilter();*/
	HttpConfigurationBuilder httpBldr = new HttpConfigurationBuilder(element,
			forceAutoConfig, pc, portMapper, portResolver, authenticationManager);
    /**创建权限相关createAnonymousFilter();
		createRememberMeFilter(authenticationManager);
		createBasicFilter(authenticationManager);
		createFormLoginFilter(sessionStrategy, authenticationManager);
		createOpenIDLoginFilter(sessionStrategy, authenticationManager);
		createX509Filter(authenticationManager);
		createJeeFilter(authenticationManager);
		createLogoutFilter();
		createLoginPageFilterIfNeeded();
		createUserDetailsServiceFactory();
		createExceptionTranslationFilter();*/
	AuthenticationConfigBuilder authBldr = new AuthenticationConfigBuilder(element,
			forceAutoConfig, pc, httpBldr.getSessionCreationPolicy(),
			httpBldr.getRequestCache(), authenticationManager,
			httpBldr.getSessionStrategy(), portMapper, portResolver,
			httpBldr.getCsrfLogoutHandler());
    
	httpBldr.setLogoutHandlers(authBldr.getLogoutHandlers());
	httpBldr.setEntryPoint(authBldr.getEntryPointBean());
	httpBldr.setAccessDeniedHandler(authBldr.getAccessDeniedHandlerBean());

	authenticationProviders.addAll(authBldr.getProviders());
    
	List<OrderDecorator> unorderedFilterChain = new ArrayList<OrderDecorator>();
    //所有filter添加到list中，无序
	unorderedFilterChain.addAll(httpBldr.getFilters());
	unorderedFilterChain.addAll(authBldr.getFilters());
	unorderedFilterChain.addAll(buildCustomFilterList(element, pc));
    //给filterchain排序
	Collections.sort(unorderedFilterChain, new OrderComparator());
	checkFilterChainOrder(unorderedFilterChain, pc, pc.extractSource(element));

	// The list of filter beans
	List<BeanMetadataElement> filterChain = new ManagedList<BeanMetadataElement>();

	for (OrderDecorator od : unorderedFilterChain) {
		filterChain.add(od.bean);
	}
    //返回创建并生成过滤器链
	return createSecurityFilterChainBean(element, pc, filterChain);
}
```

> 现在明白springSecurityFilterChain在哪里创建的了(在解析security标签是自动创建的)



	
	





 
   


