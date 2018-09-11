Best practice to move initialization logic out of constructor

Especially important when you have a subcomponent that has a dependency on a parent component that has properties that may not exist because they’re part of an asyncronous operation

before:

```js
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';

    CategoriesModel.getCategories()
      .then(categories => this.categories = categories);
  }
}

export default CategoriesController;
```

after:

```js
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';

    this.CategoriesModel = CategoriesModel;
  }

  $onInit() {
    this.CategoriesModel.getCategories()
      .then(categories => this.categories = categories);
  }
}

export default CategoriesController;
```