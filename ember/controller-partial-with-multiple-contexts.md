###Controller Partial w/ Multiple Contexts

Assuming you want a shared partial for all of your pages, at the application template level, but you want the properties to reference the current controller.

First, create a mixin, that sets the controller you want to reference the properties on, to your `application` controller:

```coffee
CurrentControllerControllerMixin = Ember.Mixin.create(
  needs: ['application']
  _currentController: Ember.computed.alias('controllers.application.currentController')

  isActiveObserver: (->
    @set('_currentController',@)
  ).observes('isActive').on('init')

)
```

*Note: Using my addon, [ember-cli-controller-isactive](https://www.npmjs.com/package/ember-cli-controller-isactive), as a dependency here.*

In your `application` controller, set the property to be itself by default:

```coffee
ApplicationController = Ember.Controller.extend(
  ...
  currentController: @
  ...
)
```

So in your application template you can do the following:

```haml
render 'layouts/partials/header' currentController
```

*Note: [.emblem](https://github.com/machty/emblem.js/) syntax*

And in controllers that you want to change the header scope:

```coffee
MyController = Ember.Controller.extend(
  CurrentControllerControllerMixin,

  fullNameObserver: (->
    @set('navigationTitle',@get('model.fullName'))
  ).observes('model.fullName')

  navigationTitle: ''

)
```
