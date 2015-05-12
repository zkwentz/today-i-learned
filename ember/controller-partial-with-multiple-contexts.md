###Controller Partial w/ Multiple Contexts

Assuming you want a shared partial for all of your pages, at the application template level, but you want the properties to reference the current controller.

First, create a mixin, that sets the controller you want to reference the properties on, to your `application` controller:

```coffee
`import Ember from 'ember'`

CurrentControllerControllerMixin = Ember.Mixin.create(
  needs: ['application']
  _currentController: Ember.computed.alias('controllers.application.currentController')

  isActiveObserver: (->
    @set('_currentController',@)
  ).observes('isActive').on('init')

)

`export default CurrentControllerControllerMixin`
```

*Note: Using my addon, [ember-cli-controller-isactive](https://www.npmjs.com/package/ember-cli-controller-isactive), as a dependency here.*

In your `application` controller, set the property to be itself by default:

```coffee
`import Ember from 'ember'`

ApplicationController = Ember.Controller.extend(
  ...
  currentController: @
  ...
)

`export default ApplicationController`
```

So in your application template you can do the following:

```coffee
render 'layouts/partials/header' currentController
```

*Note: [.emblem](https://github.com/machty/emblem.js/) syntax*

And in controllers that you want to change the header scope:

```coffee
`import Ember from 'ember'`
`import CurrentControllerControllerMixin from 'app/mixins/controllers/current-controller-controller-mixin'`

MyController = Ember.Controller.extend(
  CurrentControllerControllerMixin,

  fullNameObserver: (->
    @set('navigationTitle',@get('model.fullName'))
  ).observes('model.fullName')

  navigationTitle: ''

)

`export default MyController`
```
