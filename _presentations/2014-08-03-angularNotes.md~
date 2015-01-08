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
  var products = [
    { name: 'Azurite', price: 1.34 },
    { name: 'Bloodstone', price: 2.67 },
    { name: 'Zircon', price: 2.38 },
  ];
{% endhighlight %}
  
<hr>

**Directive**: `ng-show` / `ng-hide` - Show or display the element based in the boolean represented by the property value.

{% highlight js %}
<div class="product row" ng-hide='store.product.soldOut'>
{% endhighlight %}

Related JS (Whatever):

{% highlight js %}
 var product = {
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

<li ng-repeat="image in product.images"> // Iterate/Repeat over an array of images.

<div ng-show="product.images.length"> // Show only if the array is not empty.
{% endhighlight js %}

{% highlight css %}
//json array of images
images: [
  "images/gem-01.gif",
  "images/gem-03.gif",
  "images/gem-04.gif"
]
//empty array
images: [ ]
{% endhighlight css %}    


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
<section ng-controller="TabController as tab">
  <ul>
    <li ng-class="{active: tab.isSet(1)}">
      <a href ng-click="tab.setTab(1)"> Description
      </a>
    </li>
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
...

// Respective directives
// Note we use properties from different controllers
... 
<body ng-controller="StoreController as store">
  <div ng-repeat="product in store.products">
    <h3>
      {{ "{{ product.name "}}}}
      ({{ "{{ product.price | currency " }}}})
    </h3>

// Image Gallery
<div class='gallery' ng-show="product.images.length" ng-controller='GalleryController as gallery'>
<img ng-src="{{ "{{product.images[gallery.current] " }}}}" />
...
{% endhighlight js %}

# Working with a Form

{% highlight js %}
// Adding a review to a list of reviews - new controller!
...
 app.controller('ReviewController', function(){
    this.review = {};
    this.addReview = function(productReviewed) {
    	productReviewed.reviews.push(this.review);  // Add an element to the array
      this.review = {};
    };
  });
...

// The directives and html related.  Note the preview and its relation with ng-model.  (double directional binding)

<form name="reviewForm" ng-controller="ReviewController as reviewCtrl" ng-submit="reviewCtrl.addReview(product)">
...
  <strong>{{ "{{reviewCtrl.review.stars " }}}} Stars</strong>
...
  <select ng-model="reviewCtrl.review.stars" ng-options="stars for stars in [5,4,3,2,1]" title="Stars">
    <option value="">Rate the Product</option>
  </select>
...      
  <input type="submit" value="Submit Review" />
</form>

{% endhighlight js %}

# Form Validations

{% highlight html %}
// novalidate: Turns off default validation by some browsers
<form name=”reviewForm” novalidate> 

// required: Sets the form element as required, handled and defined by angular. Can validate URLs, emails,
<input name=”author” ng-model=”reviewCtrl.revire.author” type=”email” required/>  

// evaluates to true or false on validations implemented by angular.
{{ "{{ reviewForm.$valid " }}}} 
{% endhighlight html %}

{% highlight html %}
// Styles used by Angular
class="ng-pristine ng-invalid" // style in code when form loads
class="ng-dirty ng-invalid" // style when typing and invalid
class="ng-dirty ng-valid" // style when typing and valid
{% endhighlight html %}

{% highlight css %}
// knowing previous Angular styles let us play with css:
.ng-invalid.ng-dirty{
border-color:#FA787E;
}
.ng-valid.ng-dirty{
border-color: #78FA89;
}
{% endhighlight css %}

{% highlight html %}
// Submit form only if form is valid.
ng-submit="reviewForm.$valid && reviewCtrl.addReview(product)" 
{% endhighlight html %}

# Custom Directives

Why? Let you write “expressive” html, easier to read and to understand its behavior.
We use: `ng-include`: 

{% highlight js %}
// In the html file
<div ng-show="tab.isSet(1)" ng-include="'product-description.html'">
</div>

// In a separate file “product-description.html”:
<h4>Description</h4>
<blockquote>{{ "{{ product.description "}}}}</blockquote>
{% endhighlight js %}

Example: Defining the directive
{% highlight js %}
app.directive("productDescription", function(){
  return {
    restrict: 'E',   // E=Element, A=Attribute
      templateUrl: "product-description.html"
    };
});

// related html code when directive is of type "element":
<div>
  <product-description ng-show="tab.isSet(1)"></product-description>
</div>

// similar html code when directive is of type "attribute":
<div product-specs ng-show="tab.isSet(2)" >
{% endhighlight js %}

# Defining a controller in the directive:

{% highlight js %}
...
app.directive("productTabs", function(){
  return {
    restrict:'E',
    templateUrl: "product-tabs.html",
    controller: function(){
      this.tab = 1;

      this.isSet = function(checkTab) {
	return this.tab === checkTab;
      };

      this.setTab = function(setTab) {
	this.tab = setTab;
      };
    },
    controllerAs:'tab'
  };
});
...
  
// product-tabs.html:
...
<ul class="nav nav-pills">
  <li ng-class="{ active:tab.isSet(1) }">
    <a href ng-click="tab.setTab(1)">Description</a>
  </li>
  <li ng-class="{ active:tab.isSet(2) }">
    <a href ng-click="tab.setTab(2)">Specs</a>
  </li>
</ul>  
...

// Now to use in index.html
<product-tabs></product-tabs>

{% endhighlight js %}

# Modules and dependencies:

{% highlight js %}
// Add store-directives as dependency of getStore

(function() {
  var app = angular.module('gemStore', ['store-directives']); ->
  ...
})();

(function() {
  var app = angular.module('store-directives', []);  
  ...
})();
{% endhighlight js %}

# Angular Built in services!

{% highlight js %}
// Calling a REST Service
app.controller('StoreController', ['$http', function($http){
  var store = this;
  store.products = [];
  $http.get('/store-products.json').success(function(data){
    store.products = data;
  });
}]);

// It also provides logger, etc...
{% endhighlight js %}

Final note not related to Angular: It has been really painful to write this post with Jekyll since the Angular {{ "{{ ... "}}}} syntax requires especial handling to display the  :s