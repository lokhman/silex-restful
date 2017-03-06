# silex-restful

[![StyleCI](https://styleci.io/repos/79907109/shield?branch=master)](https://styleci.io/repos/79907109)

RESTful middleware service provider for [**Silex 2.0+**](http://silex.sensiolabs.org/) micro-framework.

> This project is a part of [`silex-tools`](https://github.com/lokhman/silex-tools) library.

## <a name="installation"></a>Installation
You can install `silex-restful` with [Composer](http://getcomposer.org):

    composer require lokhman/silex-restful

## <a name="documentation"></a>Documentation
Registering `RestfulServiceProvider` will easily extend your application routing with JSON request/response methods,
error handing and JSON parameter acceptance. It works in the same way as Silex routing binding (`get`, `post`, etc),
supports own `controllers_factory` and `mount` functionality.

    use Lokhman\Silex\Provider\RestfulServiceProvider;

    $app->register(new RestfulServiceProvider());

    // response is transformed to JSON string
    $app['restful']->get('/api', function() {
        return ['version' => '1.0'];
    });

    // can mount controller collections
    $app['restful']->mount('/api/v2', function($api) {
        // accepts parameters from "application/json" body
        $api->post('/submit', function(Request $request) {
            return ['params' => $request->request->all()];
        });
    });

    // can mount controller providers
    class ApiBundle implements ControllerProviderInterface {

        function connect(Application $app) {
            $factory = $app['restful.controllers_factory'];

            $factory->get('/', function() {
                // will modify all exceptions to JSON compatible responses
                throw new GoneHttpException('API v3 is not supported anymore.');
            });

            return $factory->getControllerCollection();
        }

    }

    $app->mount('/api/v3', new ApiBundle());

## <a name="license"></a>License
Library is available under the MIT license. The included LICENSE file describes this in detail.
