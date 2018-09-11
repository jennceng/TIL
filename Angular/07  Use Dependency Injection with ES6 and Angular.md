

```js
class CategoriesModel {
  constructor() {
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }
}

export default CategoriesModel;
```

```js
// right now this is accessing the above service directly, we can change this to use a promise
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';
    this.categories = CategoriesModel.categories;
  }
}

export default CategoriesController;
```

---

q service

```js
class CategoriesModel {
  constructor($q) {
    this.$q = this; // people often forget this to do this assignment, must do this so it can be accessed outside of constructor
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }
  
  getCategories() {
    return this.$q.when(this.categories); // returns a promise that is resolved with this.categories
  }
}

export default CategoriesModel;
```

```js
class CategoriesController {
  constructor(CategoriesModel) {
    'ngInject';
    
    CategoriesModel.getCategories()
      .then(result => this.categories = result); 
  }
}

export default CategoriesController;
```