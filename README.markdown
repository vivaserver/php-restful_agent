# RESTful Agent

This is a simple Composer package for accessing RESTful resources using Curl. Enjoy!

## Installation

The easiest way is to install it as any [Composer](http://getcomposer.org/) package. Just add an entry to your `composer.json` file requiring the latest available version:

    ...
    "require": {
      "vivaserver/restful_agent": "dev-master"
      ...
    }
    ...

The package will be automatically installed when you execute the `composer install` command.

## Usage

Create a new instance of the RESTful Agent after requiring the Composer autoloader ans you should be ready to go.

    require 'vendor/autoload.php';
    $agent = new Resftful\Agent;

Later you should be able to use the $agent instance to perform any HTTP request.

### DELETE request

    $agent->delete('http://www.example.com/resources/37');

### GET request

    $agent->get('http://www.example.com/resources');

### POST request

Pass POST parameters as an associative array after the URL. 

    $agent->post('http://www.example.com/resources',array('id'=>$id,'name'=>$name,'description'=>$description));

### PUT request

Pass PUT parameters as an associative array after the URL. 

    $agent->put('http://www.example.com/resources/25',array('description'=>$description));

## Return values

The library expects that the RESTful resource will use HTTP response codes to acknowledge the status of it's returned message. Thus, the return values of the library methods is always an object with two properties:

* `code`

  The HTTP status code of the resource response.

* `body`

  The proper body of the returned response.

### Handling return values

Knowing the above, just take into consideration the response code to act on the correct status of the resource. For example:

    $response = $agent->get('http://www.example.com/resources/54');
    $result = $response->body;
    switch ($response->code) {
      case 200:
        return $result;
      break;

      case 404:
        return NULL;
      break;

      case 500:
        error_log($result);
      break;
    }


## Notes

As expected, the return value of all the methods is the output of the resource to the request. 

But please not that, on library failure, an Exception will be thrown. So it's advisable to use it inside a try/catch block, like so:

    try {
      $agent->delete('http://www.example.com/resource/37');
    }
    catch (Exception $e) {
      error_log($e->getMessage());
    }

&copy;2013 [Cristian R. Arroyo](mailto:cristian.arroyo@vivaserver.com)
