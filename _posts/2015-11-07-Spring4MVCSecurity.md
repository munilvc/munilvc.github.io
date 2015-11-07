---
layout: post
title: Proyecto base de Spring 4: MVC + Security
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

### Algunas notas:
1. Maven pom.xml “a la antigua”, no usé spring boot porque quería configurarlo todo a manera de refrescar la memoria.
2. Como verán no tiene nada de xml (aparte del pom), esto gracias a Java Config.

### Referencias Bibliográficas:
* <a href="http://spring.io/" target="_blank">Documentación oficial</a>
* <a href="http://redirect.viglink.com?key=6ff2274896c87f60f3d05d9937858af7&u=http%3A%2F%2Fwww.amazon.com%2FSpring-Action-Craig-Walls%2Fdp%2F161729120X" target="_blank">Libro: Spring In Action 4</a>
* <a href="https://www.youtube.com/watch?v=TjlDbIIJBi8" target="_blank">Youtube: From 0 to Spring Security 4.0 (Rob Winch)</a> 
