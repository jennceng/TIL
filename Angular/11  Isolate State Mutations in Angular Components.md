Managing state in Angular 2 uses reactive **unidirectional data flow **that favors **immutable operations**

How do we manage mutable operations like forms in Angular?

Mutable state in itself is not a bad thing as long as you’re explicit about the boundaries around it

---

Problem: 

Let’s say you add functionality to edit an existing bookmark. If you mess with the title of the bookmark then click ‘cancel’, the edited title persists.

Solve:

One simple technique is to make a local copy of, for example, the bookmark you’re editing with Object.assign. If you decide to cancel the editing operation, the original bookmark is unchanged, and if you decide to save it, you can pass the new object up to the parent/smart component to be saved.



Before:

```js
let saveBookmarkComponent = {
  bindings: {
    bookmark: '<',
    save: '&',
    cancel: '&'
  },
  template,
  controllerAs: 'saveBookmarkCtrl'
};
```

```js
// savebookmark.html form is bound directly to the book mark object, we need a way to get around that
<div class="save-bookmark">
    <h4 ng-if="!saveBookmarkCtrl.bookmark.id">Create a bookmark in
        <span class="text-muted">{{saveBookmarkCtrl.bookmark.category}}</span>
    </h4>
    <h4 ng-if="saveBookmarkCtrl.bookmark.id">Editing {{saveBookmarkCtrl.bookmark.title}}</h4>

    <form class="edit-form" role="form" novalidate
        ng-submit="saveBookmarkCtrl.save({bookmark:saveBookmarkCtrl.bookmark})" >
        <div class="form-group">
            <label>Bookmark Title</label>
            <input type="text" class="form-control" ng-model="saveBookmarkCtrl.bookmark.title" placeholder="Enter title">
        </div>
        <div class="form-group">
            <label>Bookmark URL</label>
            <input type="text" class="form-control" ng-model="saveBookmarkCtrl.bookmark.url" placeholder="Enter URL">
        </div>
        <button type="submit" class="btn btn-info btn-lg">Save</button>
        <button type="button" class="btn btn-default btn-lg pull-right" ng-click="saveBookmarkCtrl.cancel()">Cancel</button>
    </form>
</div>
```



After: Add a SaveBookMarkController that uses an onchanges hook

```js
class SaveBookmarkController{
  $onchanges() {
    this.editedBookmark = Object.assign({}, this.bookmark);
  }
}
```

```js
let saveBookmarkComponent = {
  bindings: {
    bookmark: '<',
    save: '&',
    cancel: '&'
  },
  template,
  controller,
  controllerAs: 'saveBookmarkCtrl'
};
```

```js
// savebookmark.html
// saveBookmarkCtrl.bookmark.whatever -> saveBookmarkCtrl.editedBookmark.whatever, no longer bound directly to bookmark
<div class="save-bookmark">
    <h4 ng-if="!saveBookmarkCtrl.bookmark.id">Create a bookmark in
        <span class="text-muted">{{saveBookmarkCtrl.editedBookmark.category}}</span>
    </h4>
    <h4 ng-if="saveBookmarkCtrl.bookmark.id">Editing {{saveBookmarkCtrl.bookmark.title}}</h4>

    <form class="edit-form" role="form" novalidate
        ng-submit="saveBookmarkCtrl.save({bookmark:saveBookmarkCtrl.editedBookmark})" >
        <div class="form-group">
            <label>Bookmark Title</label>
            <input type="text" class="form-control" ng-model="saveBookmarkCtrl.editedBookmark.title" placeholder="Enter title">
        </div>
        <div class="form-group">
            <label>Bookmark URL</label>
            <input type="text" class="form-control" ng-model="saveBookmarkCtrl.editedBookmark.url" placeholder="Enter URL">
        </div>
        <button type="submit" class="btn btn-info btn-lg">Save</button>
        <button type="button" class="btn btn-default btn-lg pull-right" ng-click="saveBookmarkCtrl.cancel()">Cancel</button>
    </form>
</div>
```

