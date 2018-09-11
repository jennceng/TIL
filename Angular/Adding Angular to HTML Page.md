<https://egghead.io/lessons/first-step-adding-to-project>

added via cdn

```html
<!DOCTYPE html>
<html ng-app> <!-- to instantiatle the angular applciation -->
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <h1>{{hello}}</h1> <!-- binds this to a property called hello -->
    <input type="text" ng-model="hello" /> <!-- set it to ng-model equal to hello, when changing the input field this will change the value in the h1 tag to show the angular js application is initializing and spinning up -->
  </body>
  <script type="text/javascript"
    src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.2/angular.min.js">
  </script>
</html>
```