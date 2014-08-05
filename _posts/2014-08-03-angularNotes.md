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

**Directive**: `ng-app` - Attach the application module to the page.

{% highlight js %}
<html ng-app="gemStore">

{% endhighlight %}

Related JS (Whatever):

{% highlight js %}
var app = angular.module('gemStore', []);
{% endhighlight %}

<hr>

**Directive**: `ng-controller` - Attach a controller function to the body element (since it is inside the 
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

**Directive**: `ng-repeat` - Loop, repeats the element for each item in the array.

{% highlight js %}
<div class="product row" ng-repeat="product in store.products">
      <h3> {{ "{{ product.price " }}}} </h3>
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

**Directive**: `ng-show` / `ng-hide` - Show or display the element based in the boolean represented by the property value.

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

# Angular Filters 

Use a pipe ‘|’ to say “output ‘product.price’ into the ‘currency’ filter”.

{% highlight js %}
{{ "{{ product.price | currency " }}}} // Currency
{{ "{{ '1388123412323' | date:'MM/dd/yyyy @ h:mma' " }}}} // Date
this.review.createdOn = Date.now(); // If we use this in controller
{{ "{{ review.createdOn | date " }}}} // Date as Aug 3, 2014
{{ "{{ 'anyString' | uppercase " }}}} // Uppercase, lowercase
{{ "{{ 'My Description' | limitTo:8 "}}}} // Number of characters
<li ng-repeat="product in store.products | limitTo:3"> //Limit iterations
<li ng-repeat="product in store.products | orderBy:'-price'"> // Order by, minus(-) means desc, none means asc.
{% endhighlight %}

# Working with images

{% highlight js %}
<img ng-src="{{ "{{product.images[0].full " }}}}"/>

<li class="small-image pull-left thumbnail" ng-repeat="image in product.images"> // Iterate/Repeat over an array of images.

<div class="gallery" ng-show="product.images.length"> // Show only if the array is not empty.
{% endhighlight js %}

# Working with a panel controller 

To extract logic to javascript and not inside html.

{% highlight js %}
// A new controller...
...
app.controller('TabController', function(){
  this.tab=1;
  this.setTab = function(setTabValue){
      this.tab = setTabValue;
  };
	  
  this.isSet = function(tabValue){
      return this.tab == tabValue;
  };
});
...

// Respective directives in html code
...
<section class="tab" ng-controller="TabController as tab">
        <ul class="nav nav-pills">
          <li ng-class="{active: tab.isSet(1)}">
            <a href ng-click="tab.setTab(1)">Description</a></li>
<div ng-show="tab.isSet(2)">
          <h4>Specs</h4>
          <blockquote>{{ "{{ product.shine " }}}}</blockquote>
</div>
...
{% endhighlight js %}

Other example of controller and directives:

{% highlight js %}
// Now for setting a default
...
  app.controller('GalleryController', function(){
    this.current = 0;
    this.setCurrent = function(newGallery){
      this.current = newGallery || 0;
    };
  });
…

// Respective directives
// Note we use properties from different controllers - where scope is intercepted:
 
  <body class="list-group" ng-controller="StoreController as store">
    <header>
      <h1 class="text-center">Flatlander Crafted Gems</h1>
      <h2 class="text-center">– an Angular store –</h2>
    </header>
    <div class="list-group-item" ng-repeat="product in store.products">
      <h3>
        {{ "{{ product.name "}}}}
        <em class="pull-right">{{ "{{ product.price | currency " }}}}</em>
      </h3>

      <!-- Image Gallery  -->
      <div class='gallery' ng-show="product.images.length" ng-controller='GalleryController as gallery'>
        <img ng-src="{{ "{{product.images[gallery.current] " }}}}" />
...
{% endhighlight js %}