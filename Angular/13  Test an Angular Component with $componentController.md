

```js
// categories/categories.spec.js

let component, $componentController, CategoriesModel;

beforeEach(() => {
  // because we're testing a component, we need to import the categories module and then mock out the categories model
  window.module('categories');
  
  window.module($provide => {
    $provide.value('CategoriesModel', {
      getCategories: () => {
        return {
          then: () => {} // simulate a fake promise
        };
      }
    });
  });
});

describe('Controller', () => {
    it('calls CategoriesModel.getCategories immediately', () => {
      spyOn(CategoriesModel, 'getCategories').and.callThrough();

      component = $componentController('categories', {
        CategoriesModel
      });
      component.$onInit(); // need to provoke the lifecycle method since we're not actually adding it to the DOM

      expect(CategoriesModel.getCategories).toHaveBeenCalled();
    });
  });
```