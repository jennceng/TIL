

learn how to use inputs / outputs to create smart and dumb components, property data goes in and user events come out

* dumb components have no logic, only display data and relay user events back to parent component



```js
// /Users/Jenn/sandbox/learn_angular/eggly-es6/app/components/categories/category-item/category-item.component.js
import template from './category-item.html';
import './category-item.styl';

const categoryItemComponent = {
  bindings: {
    category: '<' // binds from parent to child, change detection is only on the parent, not the child
  }
  template,
  controllerAs: 'categoryItemCtrl' // for components if you dont explicitly define a controller it will implicityly create one for you
};

export default categoryItemComponent;
```



Update categories.html

```js
<a>
	<img class="logo" src="assets/img/eggly-logo.png">
</a>
<ul class="nav nav-sidebar">
	<li class="category-item"
		ng-repeat="category in categoriesListCtrl.categories">{{category.name}}</li>
</ul>
```

```js
<ul>
  <li ng-repeat="category in categoriesListctrl.categories">
    <category-item category="category"></category-item>
  </li>
</ul>
// to use the category item component
```

