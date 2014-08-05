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

## Some theory:
- Directives: HTML annotations that trigger js behavior. Many useful directives are provided by angularJS.
- Modules: Where app components live. Help to organize and encapsulate code.
- Controllers: Where we define app behavior by defining functions and values.
- Expressions:  {{...}} stuff.

# Angular claims to be MVW:  
- Model (ClientSide Model shared by Model and Whatever)
- View (Where we use HTML, Directives, Modules, Expressions) 
- Whatever (Where we define Controllers, Modules, Directives)


## Directives and "whatever"

Directive: 
`ng-app` - Attach the application module to the page.

<html ng-app="gemStore">

Whatever:
var app = angular.module('gemStore', []);

<hr>

Directive: 
`ng-controller` - Attach a controller function to the body element (since it is inside the 
<body> tag, the scope is within the <body>. For example it could also be a div).

<body ng-controller="StoreController as store">

Whatever:
app.controller('StoreController', function(){
  this.products=gems; //Put data into a “model”
});

Note: A controller is attached to an app.

<hr>

Directive: 
`ng-repeat` - Loop, repeats the element for each item in the array.

<div class="product row" ng-repeat="product in store.products">
      <h3>
        {{product.name}}
        <em class="pull-right">${{product.price}}</em>
      </h3>
 </div>

Whatever:
  var gems = [
    { name: 'Azurite', price: 2.95 },
    { name: 'Bloodstone', price: 5.95 },
    { name: 'Zircon', price: 3.95 },
  ];

<hr>

Directive: 
`ng-show` / `ng-hide` - Show or display the element based in the boolean represented by the property value.

<div class="product row" ng-hide='store.product.soldOut'>

Whatever:
 var gem = {
    name: 'Azurite',
    price: 110.50,
    canPurchase: false,
    soldOut: true
  };

<hr>
  
## Filters: 

Use a pipe ‘|’ to say “output ‘product.price’ into the ‘currency’ filter”

{{product.price | currency }} -> Currency
{{'1388123412323' | date:'MM/dd/yyyy @ h:mma'}} -> Date
this.review.createdOn = Date.now(); -> If we use this in controller
{{review.createdOn | date}} -> Aug 3, 2014
{{'octagon gem' | uppercase}} -> Uppercase, lowercase
{{'My Description' | limitTo:8}} ->Number of characters
<li ng-repeat="product in store.products | limitTo:3"> ->Limit iterations

<li ng-repeat="product in store.products | orderBy:'-price'"> ->Order by, ‘-’ means desc, none means asc.

Working with images:
<img ng-src="{{product.images[0].full}}"/>

<li class="small-image pull-left thumbnail" ng-repeat="image in product.images"> -> Iterate/Repeat over an array of images.

<div class="gallery" ng-show="product.images.length"> ->Show only if the array is not empty.

Working with a panel controller:
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

Respective directives:
...
<section class="tab" ng-controller="TabController as tab">
        <ul class="nav nav-pills">
          <li ng-class="{active: tab.isSet(1)}">
            <a href ng-click="tab.setTab(1)">Description</a></li>
<div ng-show="tab.isSet(2)">
          <h4>Specs</h4>
          <blockquote>{{"Shine:" + product.shine}}</blockquote>
        </div>
...

Other example of controller and directives:

...
  app.controller('GalleryController', function(){
    this.current = 0;
    this.setCurrent = function(newGallery){
      this.current = newGallery || 0;
    };
  });
…

related directives

…// usando properties de varios controllers - donde se intercepta su scope:
  <body class="list-group" ng-controller="StoreController as store">
    <header>
      <h1 class="text-center">Flatlander Crafted Gems</h1>
      <h2 class="text-center">– an Angular store –</h2>
    </header>
    <div class="list-group-item" ng-repeat="product in store.products">
      <h3>
        {{product.name}}
        <em class="pull-right">{{product.price | currency}}</em>
      </h3>

      <!-- Image Gallery  -->
      <div class='gallery' ng-show="product.images.length" ng-controller='GalleryController as gallery'>
        <img ng-src="{{product.images[gallery.current]}}" />
...

Working with a Form

Adding a review to a list of reviews - new controller! :
...
 app.controller('ReviewController', function(){
    this.review = {};
    this.addReview = function(productReviewed) {
    	productReviewed.reviews.push(this.review);  // Add an element to the array
      this.review = {};
    };
  });
...

The directives and html related.  Note the preview and it relation with ng-model.  (double directional binding)

<!--  Review Form -->
            <form name="reviewForm" ng-controller="ReviewController as reviewCtrl" ng-submit="reviewCtrl.addReview(product)">
...
              <!--  Live Preview -->
              <blockquote>
                <strong>{{reviewCtrl.review.stars}} Stars</strong>
   ...

              <!--  Review Form -->
                <select ng-model="reviewCtrl.review.stars" class="form-control" ng-options="stars for stars in [5,4,3,2,1]" title="Stars">
                  <option value="">Rate the Product</option>
                </select>
...      
          <input type="submit" class="btn btn-primary pull-right" value="Submit Review" />

            </form>
...

Form Validations

novalidate: <form name=”reviewForm” …. novalidate>  -> Turns off default validation by some browsers
required: <input name=”author” ng-model=”reviewCtrl.revire.author” type=”email” required/> -> Sets the form element as required, handled and defined by angular. Can validate URLs, emails, 
{{reviewForm.$valid}}  -> evaluates to true or false on validations implemented by angular.

class="ng-pristine ng-invalid" -> style in code when form loads
class="ng-dirty ng-invalid" -> style when typing and invalid
class="ng-dirty ng-valid" ->style when typing and valid
=> we can play with styles:
.ng-invalid.ng-dirty{
border-color:#FA787E;
}
.ng-valid.ng-dirty{
border-color: #78FA89;
}

ng-submit="reviewForm.$valid && reviewCtrl.addReview(product)"  -> Submit form only if form is valid.


Custom Directives: Let you write “expressive” html, easier to read and to understand its behavior.

ng-include: 
          <div ng-show="tab.isSet(1)" ng-include="'product-description.html'">
          </div>

moved this to a separate file “product-description.html”:
<h4>Description</h4>
<blockquote>{{product.description}}</blockquote>
 
Example:
js code defining the directive:
app.directive("productDescription", function(){
    return {
      restrict: 'E',   // E=Element, A=Attribute
      templateUrl: "product-description.html"
    };
  });

related html code when element:
 <div>
            <product-description ng-show="tab.isSet(1)"></product-description>
</div>

similar html code when attribute:
  <div product-specs ng-show="tab.isSet(2)" >



Now putting the controller into the directive:

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

product-tabs.html:
...<section >
          <ul class="nav nav-pills">
            <li ng-class="{ active:tab.isSet(1) }">
              <a href ng-click="tab.setTab(1)">Description</a>
            </li>
            <li ng-class="{ active:tab.isSet(2) }">
              <a href ng-click="tab.setTab(2)">Specs</a>
…

index.html
<product-tabs></product-tabs>



Modules and dependencies:

(function() {
  var app = angular.module('gemStore', ['store-directives']); ->Add store-directives as dependency to getStore.

…
})();

(function() {
  var app = angular.module('store-directives', []);  
…
})();



Angular Built in services!

  app.controller('StoreController', ['$http', function($http){
    var store = this;
    store.products = [];
    $http.get('/store-products.json').success(function(data){
    	store.products = data;
    });
  }]);
