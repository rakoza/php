### Kako Laravel razrjesava type-hinted dependesije?

https://laravel.com/docs/9.x/container#zero-configuration-resolution
https://laravel.com/docs/9.x/container#automatic-injection

Check `Illuminate\Routing\Route` for method:

```php

    /**
     * Run the route action and return the response.
     *
     * @return mixed
     *
     * @throws \Symfony\Component\HttpKernel\Exception\NotFoundHttpException
     */
    protected function runController()
    {
        return $this->controllerDispatcher()->dispatch(
            $this, $this->getController(), $this->getControllerMethod()
        );
    }
```

Check `Illuminate\Routing\ControllerDispatcher` for method `dispatch`

```php

    /**
     * Dispatch a request to a given controller and method.
     *
     * @param  \Illuminate\Routing\Route  $route
     * @param  mixed  $controller
     * @param  string  $method
     * @return mixed
     */
    public function dispatch(Route $route, $controller, $method)
    {
        $parameters = $this->resolveClassMethodDependencies(
            $route->parametersWithoutNulls(), $controller, $method
        );

        if (method_exists($controller, 'callAction')) {
            return $controller->callAction($method, $parameters);
        }

        return $controller->{$method}(...array_values($parameters));
    }
```

Check `resolveClassMethodDependencies` method from `Illuminate\Routing\RouteDependencyResolverTrait` trait

```php
    /**
     * Resolve the object method's type-hinted dependencies.
     *
     * @param  array  $parameters
     * @param  object  $instance
     * @param  string  $method
     * @return array
     */
    protected function resolveClassMethodDependencies(array $parameters, $instance, $method)
    {
        if (! method_exists($instance, $method)) {
            return $parameters;
        }

        return $this->resolveMethodDependencies(
            $parameters, new ReflectionMethod($instance, $method)
        );
    }
```

> `resolveClassMethodDependencies` initialize all parameters you placed as function arguments in your controller



