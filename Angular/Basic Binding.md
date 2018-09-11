

<https://egghead.io/lessons/angularjs-binding>

Binding is instantiated with double curley braces, {{ }}

```html
{{"Hello"}} 
<!--renders the string hello to the page-->
```

```html
{{2 + 2}}
<!--renders 4 to the page-->
```

```html
<!--not just for rendering to page, can also be used to create classes such as below using ng-class -->
<!DOCTYPE html>
<html ng-app> <!-- to instantiatle the angular applciation -->
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <script type="text/javascript"
      src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.2/angular.min.js">
    </script>
      <script type="text/javascript"
    src="https://cdnjs.cloudflare.com/ajax/libs/angular-ui-router/0.2.11/angular-ui-router.js">
    </script>
  </head>
  <body>

    <div ng-class="first.greeting">
      {{first.greeting}} {{"World"}}
    </div> <!-- binds this to a property called hello -->

    <input type="text" ng-model="first.greeting" /> <!-- set it to ng-model equal to hello, when changing the input field this will change the value in the h1 tag to show the angular js application is initializing and spinning up -->
  </body>
</html>
```