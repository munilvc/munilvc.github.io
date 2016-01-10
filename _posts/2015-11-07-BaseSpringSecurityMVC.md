---
layout: post
title: Proyecto base de Spring 4 - MVC and Security
comments: true
---

Hace ya varios meses que quería publicar algo sobre Spring Security, pues basado en mi experiencia, los proyectos más importantes en los que he participado en Perú como en USA lo usan para la seguridad en sus aplicaciones, además que es un módulo que tiene una amplia acogida por la comunidad Spring!

* Proyecto: https://github.com/munilvc/MySpringWebSecurityApp
* Descripción: Es una aplicacion simple que muestra como hacer login y logout con SpringMVC y SpringSecurity, sin usar XML, y con algo de Bootstrap para hacerla responsive. 

### Herramientas: 
* Spring MVC 4
* Spring Security 4 (Login, Logout, Usuarios en memoria)
* Algo de Bootstrap - Para que se vea en un teléfono móvil
* Java Config (Adiós xml - para containers que soporten Servlets 3 - desde Tomcat7)
* Maven

### Algunas notas sobre lo que veran en el código:

#### SpringMVC

* Configurar Spring MVC es ahora mas facil que nunca, como no queremos XML, todo lo que necesitamos es una clase que extienda AbstractAnnotationConfigDispatcherServletInitializer, y en esta configuramos las clases @Configuration que representan a nuestros application contexts.

    {% highlight java linenos %}    
    public class MvcInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    
        @Override
        protected Class<?>[] getServletConfigClasses() {
            return new Class[] { WebApplicationContextConfig.class };
        }
        ...
    {% endhighlight %}
    
* Y luego en un application context tenemos que extender WebMvcConfigurerAdapter y tambien declarar @EnableWebMvc. Con esto estamos!  Dentro de esta clase configuration, podemos declarar las cosas que normalmente haciamos en XML como el ViewResolver y ResourceHandler.
    
    {% highlight java linenos %}
    @Configuration
    @EnableWebMvc
    @ComponentScan(basePackages = { "org.munilvc.myspringwebsecurityapp" })
    public class WebApplicationContextConfig extends WebMvcConfigurerAdapter {
    
        @Bean(name = "viewResolver")
        public InternalResourceViewResolver getViewResolver() {
            InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
            viewResolver.setPrefix("/WEB-INF/views/");
            viewResolver.setSuffix(".jsp");
            return viewResolver;
        }
        ...
    {% endhighlight %}
    
* Si queremos crear @Controllers especificos los creamos en otro paquete si queremos y usamos @ComponentScan desde la clase @Configuration para que la aplicación sepa donde buscar los controllers. Recontra simple!

* Nota - XML: para usar xml lo que se hace es declarar un "FrontController", se declara un servlet (de una clase spring DispatcherServlet) junto un path base de accion para este servlet en el archivo web.xml.  Esto hace que el request si viene de este path se redireccione a Spring y este levanta un contexto propio basado en un archivo con el nombre del servlet + "-servlet.xml", dentro de este archivo se configuran las cosas como view resolver, scanners para anotaciones, etc.  Tambien podemos configurar archivos de contexto adicionales usando un listener (ContextLoaderListener de Srping) y el tag context-Param.

    {% highlight xml linenos %}
    <servlet>
		<servlet-name>MuniApp</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>MuniApp</servlet-name>
		<!-- / means that it will handle all requests comming to the application. Apps default servlet. -->
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	
	<!-- Listener and context-param, used to create addional context files. -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/muniapp-application-context.xml</param-value>
	</context-param>
    {% endhighlight %}

#### SpringSecurity

* SpringSecurity funciona mediante filtros.
* Con SpringSecurity 4, gracias a que ahora soporta JavaConfig, solo necesitamos agregar una clase que implemente WebApplicationInitializer (en el ejemplo uso una implementacion abstract de Spring).
    
    {% highlight java linenos %}
    public class SecurityInitializer extends AbstractSecurityWebApplicationInitializer {
        ...
    {% endhighlight %}
    
* Luego agregar la anotacion @EnableWebSecurity en una clase @Configuration que implemente WebSecurityConfigurer (en el ejemplo uso un Adapter de spring).  Esto se encarga, entre otras cosas, de lo que haciamos antes en el web.xml, configurando el filtro DelegatingFilterProxy hacia "springSecurityFilterChain" de Spring, y lo que hace es delegar la responsabilidad de seguridad a un filtro de Spring en el spring-context (Del servlet-context al spring-context).

    {% highlight java linenos %}
    @Configuration
    @EnableWebSecurity
    public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.authorizeRequests()
                .antMatchers("/resources/**").permitAll()
                .anyRequest().authenticated()
                .and().formLogin().loginPage("/login").permitAll()
                .and().logout().permitAll();
        }
      ...
    {% endhighlight %}

* Para customizar la seguridad, ya solo tenemos que hacerle @Override a los metodos configure() de la clase @Configuration.

#### Otros
* Spring4, sigue siendo bastante Convention over Configuration friendly.
* Maven pom.xml “a la antigua”, no usé spring boot porque quería configurarlo todo a manera de refrescar la memoria.
* Como verán no tiene nada de xml (aparte del pom), esto gracias a Java Config, por fin!

### Referencias Bibliográficas:
* <a href="http://spring.io/" target="_blank">Documentación oficial</a>
* <a href="http://redirect.viglink.com?key=6ff2274896c87f60f3d05d9937858af7&u=http%3A%2F%2Fwww.amazon.com%2FSpring-Action-Craig-Walls%2Fdp%2F161729120X" target="_blank">Libro: Spring In Action 4</a>
* <a href="https://www.youtube.com/watch?v=TjlDbIIJBi8" target="_blank">Youtube: From 0 to Spring Security 4.0 (Rob Winch)</a> 
