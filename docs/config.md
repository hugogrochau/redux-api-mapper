# Config

This section will provide you the shape of the config object. Here we will list all properties supported. We recommend using the ``stateToAction`` helper function when defining your actions in your config.

```js
var config = {
  host : 'http://somehost.com/api',
  headers : {
    'Authorization' : () => window.localStorage.getItem('token'),
    'Content-type' : 'application/json'
  },
  options : { //these options are supported by the default http-layer.
    beforeRequest : (request) => {},
    afterResponse : (response) => JSON.parse(response.data),
    bodyParse     : (body) => JSON.stringify(body),
    responseParse : (response) => JSON.parse(response.data)
  },
  resources : [
    {
      name : 'Users',
      path : '/users',
      headers : {
        'Some-Fancy-Header' : 'some-fancy-header'
      },
      options : { //these options are supported by the default http-layer.
        beforeRequest : (request) => {},
        afterResponse : (response) => JSON.parse(response.data),
        bodyParse     : (body) => JSON.stringify(body),
        responseParse : (response) => JSON.parse(response.data)
      },
      endPoints : [
        {
          name : 'getUsers',
          path : '/',
          method : 'get',
          action : stateToProps(actionOnFetch, actionOnComplete, actionOnError, actionOnCancelled),
          headers : {
            'Another-Fancy-Header' : 'fancy-header'
          },
          options : { //these options are supported by the default http-layer.
            beforeRequest : (request) => {},
            afterResponse : (response) => JSON.parse(response.data),
            bodyParse     : (body) => JSON.stringify(body),
            responseParse : (response) => JSON.parse(response.data)
          }
        },
        {
          name : 'getUser',
          path : '/{id}', //{param} -> will be handled by the library
          method : 'get', //'post', 'put', 'patch', 'delete'...
          action : stateToProps(actionOnFetch, actionOnComplete),
          headers : {
            'Another-Fancy-Header' : 'Fancy-Header'
          }
        }
      ]
    }
  ]
}
```

### Caveats

* Inner headers will prevail
* Every path parameter inside brackets are handled by the library. This mapper will try to match the param name with a param provided by you when you do the request call
* ``actionOnFetch and others`` can be an action creator or an action object containing the type. In case of a function(action creator), it will call the function passing the response as a parameter and it will expect an object with a type and the payload as return.

### Tips

* You can (and should) put each resource in a separated file
* Always use stateToAction helper unless you have a good reason to do it by hand
* Add optional parameters as comments in your end-points, so you'll know what are the possible parameters

<b>Thats it! Go to the next section to learn how to create an Http-Layer and to start doing your requests</b>
