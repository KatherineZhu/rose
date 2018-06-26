# Angular

- When Angular take over the webapp? `ContentLoaded` event
- What is the context of Angular? `ng-app`
- Angular `module` two phases: config and run
- Indepedency Injection
    - https://github.com/angular/angular.js/wiki/Understanding-Dependency-Injection
    - why use it ? the key to making easily testable components
    - It's worth noting that the injector will only create an instance of a service once
- Scope
    - `vm = this`, why not `vm = $scope`?
    - A scope (prototypically) inherits properties from its parent scope.
    - https://github.com/angular/angular.js/wiki/Understanding-Scopes
- UI Router
    - http://slides.com/timkindberg/ui-router#/
- digest cycle and watch
    - difference, when do digest cycle and what will be check?
    - All watch should in Angular scope (jquery, setTimeout... will not work)
- [Anti patterns](https://github.com/angular/angular.js/wiki/Anti-Patterns)
