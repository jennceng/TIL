

How to notify your components that a collection has changed at the model

---

Problem: While editing a bookmark of a category, if another category is selected the edit bookmark form stays on the view even though it no longer reflects the current category selected

Solve: sending an event with $rootScope.broadcast to reset the currently selected bookmark

### $rootScope

* be judicious as to what you put in rootScope
* it is not a global grabbing of properties, but it works very well as an event bus as all events come from the scope object and $rootScope being the mother of them all



Before:

```js
// categories model 
class CategoriesModel {
  constructor($q) {
    'ngInject';

    this.$q = $q;
    this.currentCategory = null;
    this.categories = [
      ...
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

```js
// bookmarks controller 
class BookmarksController {
  constructor($scope, CategoriesModel, BookmarksModel) {
    'ngInject';

    this.CategoriesModel = CategoriesModel;
    this.BookmarksModel = BookmarksModel;
  }

  $onInit() {
    this.BookmarksModel.getBookmarks()
      .then((bookmarks) => {
        this.bookmarks = bookmarks;
      });

    this.getCurrentCategory = this.CategoriesModel.getCurrentCategory.bind(this.CategoriesModel);
    this.deleteBookmark = this.BookmarksModel.deleteBookmark;
  }

  editBookmark(bookmark) {
    this.currentBookmark = bookmark;
  }

  reset() {
    this.currentBookmark = null;
  }

  ...
}

export default BookmarksController;
```

After:

```js
// categories model 
class CategoriesModel {
  constructor($q, $rootScope) { // add rootScope
    'ngInject';

    this.$q = $q;
    this.$rootScope = $rootScope;
    this.currentCategory = null;
    this.categories = [
      ...
    ];
  }

  getCategories() {
    return this.$q.when(this.categories);
  }

  setCurrentCategory(category) {
    this.currentCategory = category;
    this.$rootScope.$broadcast('onCurrentCategoryUpdated'); // if a category is set it will broadcast an event to rootScope
  }

  getCurrentCategory() {
    return this.currentCategory;
  }
}

export default CategoriesModel;
```

```js
// bookmarks controller 
class BookmarksController {
  constructor($scope, CategoriesModel, BookmarksModel) { // make sure to inject dependency of scope
    'ngInject';

    this.$scope = $scope; // local reference
    this.CategoriesModel = CategoriesModel;
    this.BookmarksModel = BookmarksModel;
  }

  $onInit() {
    this.BookmarksModel.getBookmarks()
      .then((bookmarks) => {
        this.bookmarks = bookmarks;
      });

    this.$scope.$on('onCurrentCategoryUpdated', this.reset.bind(this)); // bind this to bookmarks controller, when it receives that event it will reset the current Bookmark as per below
    this.getCurrentCategory = this.CategoriesModel.getCurrentCategory.bind(this.CategoriesModel);
    this.deleteBookmark = this.BookmarksModel.deleteBookmark;
  }

  editBookmark(bookmark) {
    this.currentBookmark = bookmark;
  }

  reset() {
    this.currentBookmark = null;
  }

  ...
}

export default BookmarksController;
```