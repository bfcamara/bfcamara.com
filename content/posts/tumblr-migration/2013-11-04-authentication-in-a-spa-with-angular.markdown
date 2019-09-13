---
layout: post
template: post
date: 2013-11-04
tags:
- angularjs
- spa
title: "Authentication in a SPA with Angular"
slug: /post/66001429506/authentication-in-a-spa-with-angular
description: "Authentication in a SPA with Angular"
---
<p>There is no doubt that<strong> Single Page Applications (SPA) is a trend that is growing more and more each day. &nbsp;However, developing a SPA is harder than the traditional approach</strong>, in which the pages are loaded, maybe there is&nbsp;some ajax on the page with jQuery, and when you navigate away to another page a full reload occurs.</p>
<p>When I started my journey developing SPAs, I had to <strong>rethink again on some aspects of the application</strong>, and&nbsp;check what is the best approach to solve some plumbing problems, such as <strong>authentication, authorization, </strong><strong>error handling,logging, i18n,</strong> etc.</p>
<p>Regarding authentication, I think there are two major approaches:</p>
<ol>
<li><span>consider authentication as a separate page, and we only load our SPA, when the user&nbsp;</span>is authenticated</li>
<li>I have a <strong>pure SPA</strong>, and authentication occurs inside the app, which means there is no need to reload, since the SPA is already loaded.</li>
</ol>
<p>In both approaches I am assuming and taking as granted that<strong> the server always check for access-control&nbsp;</strong>(all access control in the client side can be overcome) and that we have an http interceptor&nbsp;that checks the HTTP status response, and if we get an Unauthorized, we redirect the user to the login screen.</p>
<p>In my current application I am working on, I opted for option 2, and I would like to show you how I implemented with Angular, which is my MV* web framework of choice.</p>
<p>Fist of all, I have an Angular service, AuthService, which is responsible to provide helper authentication methods to the app.</p>

```js
(function() {    
    'use strict';

    angular.module('app').factory('authService' , ['$rootScope', 'Restangular', function ($rootScope, Restangular) {

        var user = {
            isAuthenticated: false,
            name: ''
        };

        $rootScope.user = user;

        var authService = {};

        authService.init = function (isAuthenticated, userName) {
            user.isAuthenticated = isAuthenticated;
            user.name = userName;
        };

        authService.isAuthenticated = function () {
            return user.isAuthenticated;
        };

        authService.login = function (loginModel) {

            var loginResult = Restangular.all('accounts').customPOST(loginModel, 'login');

            loginResult.then(function (result) {
                user.isAuthenticated = result.loginOk;
                if (result.loginOk)
                    user.name = loginModel.userName;
            });

            return loginResult;
        };

        authService.logout = function () {
            return Restangular.all('accounts').customPOST(null, 'logout')
            .then(function (result) {
                user.isAuthenticated = false;
                user.name = '';
            });

        };

        authService.register = function (registerModel) {
            return Restangular.all('accounts').customPOST(registerModel, 'register');
        };

        authService.changePassword = function (changePasswordModel) {
            return Restangular.all('accounts').customPUT(changePasswordModel, 'changePassword');
        };

        return authService;
    }]);
})();    
```

<p></p>
<p>My fist implementation consisted of marking routes that need authentication, and detect when one of this routes are in place. The Angular route service provides an event, $routeChangeStart, every time a route changes. Here is the first implementation, that check routes with based on field allowAnonymous</p>

```js
(function () {
    'use strict';

    angular.module('app')
    .constant('APP_ROOT', angular.element('#linkAppRoot').attr('href'))
    .config(['$routeProvider', '$httpProvider', function ($routeProvider, $httpProvider) {

        $routeProvider
            .when('/', { redirectTo: '/integrations' })
            .when('/login', { templateUrl: 'app/views/login.html', controller: 'LoginCtrl', allowAnonymous: true })
            .when('/register', { templateUrl: 'app/views/register.html', controller: 'RegisterCtrl', allowAnonymous: true })
            .when('/integrations', { templateUrl: 'app/views/integrations.html', controller: 'IntegrationsCtrl' })
            .when('/integration/new', { templateUrl: 'app/views/editIntegration.html', controller: 'EditIntegrationCtrl' })
            .when('/integration/:integrationId', { templateUrl: 'app/views/editIntegration.html', controller: 'EditIntegrationCtrl' })
            .when('/activity', { templateUrl: 'app/views/activity.html', controller: 'ActivityCtrl' })
            .when('/changePassword', { templateUrl: 'app/views/changePassword.html', controller: 'ChangePasswordCtrl' })
            //.when('/settings', { templateUrl: 'app/views/settings.html', controller: 'SettingsCtrl' })
            .when('/externalaccounts', { templateUrl: 'app/views/externalAccounts.html', controller: 'ExternalAccountsCtrl' })
            .when('/404', { templateUrl: 'app/views/404.html' })
            .otherwise({ redirectTo: '/404' });

        $httpProvider.interceptors.push('processErrorHttpInterceptor');
    }]);


    angular.module('app').run(['$location', '$rootScope', '$log', 'authService', '$route',
        function ($location, $rootScope, $log, authService, $route) {

            function handleRouteChangeStart(event, next, current) {

                if (!next.allowAnonymous &amp;&amp; !authService.isAuthenticated()) {
                    $log.log('authentication required. redirect to login');

                    var returnUrl = $location.url();
                    $log.log('returnUrl=' + returnUrl);

                    //TODO: BUG -&gt; THIS IS NOT PREVENTING THE CURRENT ROUTE
                    //This has a side effect, which is load of the controller/view configured to the route
                    //The redirect to login occurs later.
                    //Possible solutions: 
                    // 1 - use $locationChangeStart (it is hard to get the route being used)
                    // 2 - Use a resolver in controller, returning a promise, and reject when needs auth
                    event.preventDefault();

                    $location.path('/login').search({ returnUrl: returnUrl })

                }
            }

            $rootScope.$on('$routeChangeStart', handleRouteChangeStart);

        }]);
})();
```

<p></p>
<p>However, there is a problem with this solution, since the code <strong>e.preventDefault() it does nothing. Basically the route is not prevented, which means that the destination controller is loaded, and eventually some API calls to the server are being done</strong>. In functional terms the user is redirected to the login screen, however we can have API calls that are not necessary due the fact that the controller is instantiated.</p>
<p>So,<strong> the correct approach consists to prevent the route to succeed</strong>. How can we do that in Angular? Well, in Angular, we can participate in the resolving process&nbsp; before the route succeeds. Here is the Angular documentation for the when() method of <a href="http://docs.angularjs.org/api/ngRoute.$routeProvider">$routeProvider</a>:</p>
<blockquote>
<p><em><strong>resolve</strong> - {Object.&lt;string, function&gt;=} - An optional map of dependencies which should be injected into the controller. If any of these dependencies are promises, the router will wait for them all to be resolved or one to be rejected</em> before the controller is instantiated. If all the promises are resolved successfully, the values of the resolved promises are injected and $routeChangeSuccess event is fired. If any of the promises are rejected the $routeChangeError event is fired.</p>
</blockquote>
<p>So,<strong> if we have a promise that is rejected when the user tries to achieve a route that needs authentication and the user is not authenticated, then the route is aborted and the event $routeChangeError is fired</strong>.</p>
<p>Based on this fact, here is the final implementation</p>

```js
(function () {
    'use strict';

    angular.module('app')
    .constant('APP_ROOT', angular.element('#linkAppRoot').attr('href'))
    .config(['$routeProvider', '$httpProvider', '$locationProvider', function ($routeProvider, $httpProvider, $locationProvider) {

        function checkLoggedIn($q, $log, authService) {
            var deferred = $q.defer();

            if (!authService.isAuthenticated()) {
                $log.log('authentication required. redirect to login');
                deferred.reject({ needsAuthentication: true });
            } else {
                deferred.resolve();
            }

            return deferred.promise;
        }

        $routeProvider.whenAuthenticated = function (path, route) {
            route.resolve = route.resolve || {};
            angular.extend(route.resolve, { isLoggedIn: ['$q', '$log', 'authService', checkLoggedIn] });
            return $routeProvider.when(path, route);
        }


        $routeProvider
            .when('/', { redirectTo: '/integrations' })
            .when('/login', { templateUrl: 'app/views/login.html', controller: 'LoginCtrl' })
            .when('/register', { templateUrl: 'app/views/register.html', controller: 'RegisterCtrl' })
            .whenAuthenticated('/integrations', { templateUrl: 'app/views/integrations.html', controller: 'IntegrationsCtrl' })
            .whenAuthenticated('/integration/new/:integrationType', { templateUrl: 'app/views/editIntegration.html', controller: 'EditIntegrationCtrl' })
            .whenAuthenticated('/integration/:integrationType/:integrationId', { templateUrl: 'app/views/editIntegration.html', controller: 'EditIntegrationCtrl' })
            .whenAuthenticated('/activity', { templateUrl: 'app/views/activity.html', controller: 'ActivityCtrl' })
            .whenAuthenticated('/changePassword', { templateUrl: 'app/views/changePassword.html', controller: 'ChangePasswordCtrl' })
            //.whenAuthenticated('/settings', { templateUrl: 'app/views/settings.html', controller: 'SettingsCtrl' })
            .whenAuthenticated('/externalaccounts', { templateUrl: 'app/views/externalAccounts.html', controller: 'ExternalAccountsCtrl' })
            .when('/404', { templateUrl: 'app/views/404.html', controller: 'NotFoundErrorCtrl' })
            .when('/apierror', {templateUrl: 'app/views/apierror.html', controller: 'ApiErrorCtrl' })
            .otherwise({ redirectTo: '/404' });

        $httpProvider.interceptors.push('processErrorHttpInterceptor');
    }]);

    angular.module('app').run(['$location', '$rootScope', '$log', 'authService', '$route',
        function ($location, $rootScope, $log, authService, $route,) {

            $rootScope.$on('$routeChangeError', function (ev, current, previous, rejection) {
                if (rejection &amp;&amp; rejection.needsAuthentication === true) {
                    var returnUrl = $location.url();
                    $log.log('returnUrl=' + returnUrl);
                    $location.path('/login').search({ returnUrl: returnUrl });
                }
            });

        }]);

})();
```

<p>Basically the method whenAuthenticated adds the checkLoggedIn resolver to the route resolve object, which asks to the authService if the current user is authenticated. If not, it rejects the promise, and the $routeChangeError is raised, which in turn is handled redirecting to login route.</p>
<p>I hope it helps</p>
<p></p>
<p>&nbsp;</p>
<p>&nbsp;</p>