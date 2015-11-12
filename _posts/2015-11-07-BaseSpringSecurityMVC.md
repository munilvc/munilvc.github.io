---
layout: post
title: Proyecto base de Spring 4 - MVC and Security
comments: true
---

Hace ya varios meses que quería publicar algo sobre Spring Security, pues basado en mi experiencia, los proyectos más importantes en los que he participado en Perú como en USA lo usan para la seguridad en sus aplicaciones, además que es un módulo que tiene una amplia acogida por la comunidad Spring!

Proyecto: https://github.com/munilvc/MySpringWebSecurityApp

### Herramientas: 
* Spring MVC 4
* Spring Security 4 (Login, Logout, Usuarios en memoria)
* Algo de Bootstrap - Para que se vea en un teléfono móvil
* Java Config (Adiós xml - para containers que soporten Servlets 3 - desde Tomcat7)
* Maven

### Algunas notas sobre lo que veran en el código:

#### SpringMVC
1. Configurar Spring MVC es ahora mas facil que nunca, como no queremos XML, todo lo que necesitamos es una clase que extienda AbstractAnnotationConfigDispatcherServletInitializer, y en esta configuramos las clases @Configuration que representan a nuestros application contexts y uno de estos tiene que extender WebMvcConfigurerAdapter y tambien llevar @EnableWebMvc. Con esto estamos!  Dentro de esta clase configuration, podemos declarar las cosas que normalmente haciamos en XML como el ViewResolver y ResourceHandler.

```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = { "org.munilvc.myspringwebsecurityapp" })
public class WebApplicationContextConfig extends WebMvcConfigurerAdapter {
```

Si queremos crear @Controllers especificos los creamos en otro paquete si queremos y usamos @ComponentScan desde la clase configuration para que la aplicación sepa donde buscar los controllers. Recontra simple!

#### SpringSecurity
1. SpringSecurity funciona mediante filtros.
2. Con SpringSecurity 4, gracias a que ahora soporta JavaConfig, solo necesitamos agregar una clase que implemente WebApplicationInitializer (en el ejemplo uso una implementacion abstract de Spring) y agregar la anotacion @EnableWebSecurity en una clase @Configuration que implemente WebSecurityConfigurer (en el ejemplo uso un Adapter de spring).  Esto se encarga, entre otras cosas, de lo que haciamos antes en el web.xml, configurando el filtro DelegatingFilterProxy hacia "springSecurityFilterChain" de Spring, y lo que hace es delegar la responsabilidad de seguridad a un filtro de Spring en el spring-context (Del servlet-context al spring-context).
3. Para customizar la seguridad, ya solo tenemos que hacerle @override a los metodos configure() de la clase @Configuration

#### Otros
1. Spring4, sigue siendo bastante Convention over Configuration friendly.
2. Maven pom.xml “a la antigua”, no usé spring boot porque quería configurarlo todo a manera de refrescar la memoria.
3. Como verán no tiene nada de xml (aparte del pom), esto gracias a Java Config, por fin!

### Referencias Bibliográficas:
* <a href="http://spring.io/" target="_blank">Documentación oficial</a>
* <a href="http://redirect.viglink.com?key=6ff2274896c87f60f3d05d9937858af7&u=http%3A%2F%2Fwww.amazon.com%2FSpring-Action-Craig-Walls%2Fdp%2F161729120X" target="_blank">Libro: Spring In Action 4</a>
* <a href="https://www.youtube.com/watch?v=TjlDbIIJBi8" target="_blank">Youtube: From 0 to Spring Security 4.0 (Rob Winch)</a> 
