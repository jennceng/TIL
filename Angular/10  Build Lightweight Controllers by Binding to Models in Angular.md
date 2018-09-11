In Angular we want to keep our controllers thin and focused on the view that it is controlling. We accomplish this by moving as much logic and state as we can into the models.

```js
// categories.html
<span class="logo" ng-click="categoriesListCtrl.onCategorySelected(null)"> //clicking on logo resets to no selected category
	<img class="logo" src="assets/img/eggly-logo.png">
</span>
<ul class="nav nav-sidebar">
	<li class="category-item"
		ng-repeat="category in categoriesListCtrl.categories">
		<category-item
			category="category"
			selected="categoriesListCtrl.onCategorySelected(category)"> // triggers onCategorySelected, which delegates it to the CategoryModel
		</category-item>
	</li>
</ul>
```

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

  onCategorySelected(category) {
    this.CategoriesModel.setCurrentCategory(category);
  }
}

export default CategoriesController;
```

```js
class CategoriesModel {
  constructor($q) {
    'ngInject';

    this.$q = $q;
    this.currentCategory = null;
    this.categories = [
      {"id": 0, "name": "Development"},
      {"id": 1, "name": "Design"},
      {"id": 2, "name": "Exercise"},
      {"id": 3, "name": "Humor"}
    ];
  }

  getCategories() {
    return this.$q.when(this.categories);
  }

  setCurrentCategory(category) {
    this.currentCategory = category;
  }

  getCurrentCategory() {
    return this.currentCategory;
  }
}

export default CategoriesModel;
```

Depending on the category selected, only the bookmarks that match it will be shown

```js
class BookmarksController {
  constructor(CategoriesModel, BookmarksModel) {  // we need CategoriesModel here because we will be utlizing CategoriesModel.getCurrentCategory to determine what bookmarks should be shown based on what category is selected (see html below)
    'ngInject';

    this.CategoriesModel = CategoriesModel;
    this.BookmarksModel = BookmarksModel;
  }

  $onInit() {
    this.BookmarksModel.getBookmarks()
      .then((bookmarks) => {
        this.bookmarks = bookmarks;
      });

    this.getCurrentCategory = this.CategoriesModel.getCurrentCategory.bind(this.CategoriesModel); // without binding, 'this' will refer to bookmarks controller, we want to to refer to CategoriesModel so we bind it
  }
}

export default BookmarksController;
```

```js
// bookmarks.html
<div class="bookmarks">
	<div ng-repeat="bookmark in bookmarksListCtrl.bookmarks | filter:{category:bookmarksListCtrl.getCurrentCategory().name}">  // filters bookmarks to be shown based on current category, which is delegated to CategoriesModel
		<button type="button" class="close">&times;</button>
		<button type="button" class="btn btn-link">
			<span class="glyphicon glyphicon-pencil"></span>
		</button>
		<a href="{{bookmark.url}}" target="_blank">{{bookmark.title}}</a>
	</div>
</div>
```

