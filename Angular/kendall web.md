```js
import template from './template.html'

export const name = 'invitations'

const controller = class {
  static $inject = ['$stateParams', '$rootScope', 'serverUrl', 'Invitations', 'FlashMessages', 'Success', 'ErrorService', 'Organizations', '$state'];

  constructor($stateParams, $rootScope, serverUrl, Invitations, FlashMessages, Success, ErrorService, Organizations, $state) {
    var vm = this;

    Organizations.get(function(response) {
      vm.organizations = response.organizations;
    });

    vm.invitation = {};
    vm.response = null;

    vm.setResponse = function (response) {
      vm.response = response;
    };

    vm.createInvitation = function() {
      Invitations.send({invitation: vm.invitation, response: vm.response}, function (response) {
        Success(response, FlashMessages.signup);
        $state.go('app.pending');
      }, function (response) {
        ErrorService(response, FlashMessages.signup);
      });
    };

    vm.notMIT = function(organization) {
      return 'MIT' !== organization.name;
    };
  }
}

export default {
  controller,
  template,
  controllerAs: 'vm',
}

export const information = {
  template: '<new-invitation></new-invitation>'
}

```