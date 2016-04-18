Module and States:

(function () {
	angular.module('appName', [
		'module1',
		'module2'
	])
	.config(function ($stateProvider, $urlRouterProvider) {
		$stateProvider
			.state('login', {
				url: '/login',
				templateUrl: 'app/login/login.html',
				controller: 'LoginCtrl',
				controllerAs: 'login'
			})
			.state('boards', {
				url: '/boards',
				templateUrl: 'app/boards/boards.html',
				controller: 'BoardsCtrl',
				controllerAs: 'boards',
				resolve: {
					currentUser: ['Auth', function (Auth) {
						return Auth.$requireAuth();
					}]
				}
			})
	})	
})();
