---
layout: post
title: AngularJS Refresher
comments: true
---

<div class="message">
Notice:
This is just my personal AngularJS refresher. In other words these are some of my notes while taking the AngularJS course at codeschool.com.  
So not all of this is my stuff but it can help you as a refresher too, if you are looking for a great intro to AngularJS I highly encourage you to follow the course itself at codeschool.com.
</div>

# Some theory:
- Directives: HTML annotations that trigger js behavior. Many useful directives are provided by angularJS.
- Modules: Where app components live. Help to organize and encapsulate code.
- Controllers: Where we define app behavior by defining functions and values.
- Expressions:  {{ "{{ ... " }}}} stuff.

## Angular claims to be MVW:  
- Model (ClientSide Model shared by Model and Whatever)
- View (Where we use HTML, Directives, Modules, Expressions) 
- Whatever (Where we define Controllers, Modules, Directives)

# Directives and "whatever"

<hr>

Directive: `ng-app` - Attach the application module to the page.

{% highlight js %}
<html ng-app="gemStore">

{% endhighlight %}

Related JS (Whatever):

{% highlight js %}
var app = angular.module('gemStore', []);
{% endhighlight %}

<hr>

Directive: `ng-controller` - Attach a controller function to the body element (since it is inside the 
<body> tag, the scope is within the <body>. For example it could also be a div).

{% highlight js %}
<body ng-controller="StoreController as store">
{% endhighlight %}

Related JS (Whatever):

{% highlight js %}
app.controller('StoreController', function(){
  this.products=gems; //Put data into a “model”
});
{% endhighlight %}

Note: A controller is attached to an app.

<hr>

Directive: `ng-repeat` - Loop, repeats the element for each item in the array.

{% highlight js %}
<div class="product row" ng-repeat="product in store.products">
      <h3>
        {{product.name}}
        <em class="pull-right">${{product.price}}</em>
      </h3>
 </div>
{% endhighlight %}
 
Related JS (Whatever):

{% highlight js %}
  var gems = [
    { name: 'Azurite', price: 2.95 },
    { name: 'Bloodstone', price: 5.95 },
    { name: 'Zircon', price: 3.95 },
  ];
{% endhighlight %}
  
<hr>

Directive: `ng-show` / `ng-hide` - Show or display the element based in the boolean represented by the property value.

{% highlight js %}
<div class="product row" ng-hide='store.product.soldOut'>
{% endhighlight %}

Related JS (Whatever):

{% highlight js %}
 var gem = {
    name: 'Azurite',
    price: 110.50,
    canPurchase: false,
    soldOut: true
  };
{% endhighlight %}

<hr>

