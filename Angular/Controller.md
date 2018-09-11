

```html
<!--not just for rendering to page, can also be used to create classes such as below using ng-class -->
<!DOCTYPE html>
<html ng-app="app"> <!-- to instantiatle the angular applciation, need to set it to something to register controller -->
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.3.3/angular.min.js"> </script>
    <script>
    angular.module("app", []) // second argument is list of dependencies
      .controller("FirstCtrl", function FirstCtrl() {
        var first = this;

        first.greeting = "First"
      })
    </script>
  </head>
  <body ng-controller="FirstCtrl as first"> <!-- says the controller will be referred to as first (ie first.greeting is actually FirstCtrl.greeting) -->
    <input type="text" ng-model="first.greeting" /> <!-- set it to ng-model equal to hello, when changing the input field this will change the value in the h1 tag to show the angular js application is initializing and spinning up -->

    <div ng-class="first.greeting">
      {{first.greeting}} {{"World"}}
    </div> <!-- binds this to a property called first.greeting -->

  </body>
</html>

```

upon first rendering hte controller will set first.greeting to “First”, initial page will say “First World” and input will be “First"