## Help me respond

Hello there :) 👋🏼

Thank you for your interest in *help-me-respond*!

In case you run into problems please feel free to create an issue for me at [the Issues Tab](https://github.com/piggydoughnut/help-me-respond/issues)

Your feedback is highly appreciated! 🌸

[About](Readme.md#about)

[Prerequisites for usage](Readme.md#prerequisites-for-usage)

[Installation](Readme.md#installation)

[Localization](Readme.md#localization)
   
[Pass arguments to your messages](Readme.md#pass-arguments-to-your-messages)

## About

This is a simple response helper which should make your life a bit easier. 

It supports localization and friendly messages for users.

`Help-me-respond` preconfigures HTTP responses for you by setting the status code, processing the message and setting headers. You only need to call one of the API functions and pass the message in a form of a string or an object to it.

[Release info](https://github.com/piggydoughnut/help-me-respond/releases)

## Prerequisites for usage

Your project is a `nodejs` server based on `Express` or something similar. Help-me-respond uses the `res` object from Express, which represents an HTTP response.


## Installation

```
npm i help-me-respond --save
````

2. Create a *config/* folder in the root of your project. That is where all the config for the library lives.

3. Create *config/default.json* for the general library config

```
{
  "logging": false,
  "prefixNone": false,  # when you server returns any data, a prefix "data" is added to the response
  "disableJsonHeader": false # remove the default 'application/json' header
  "friendlyMessages": [] # array of message keys which are defined in message.json or locales.json
}
```

4. Create *config/messages.json* for the response messages and friendly messages setup.

```
{
    "welcome": "Welcome to the platform!",
    "notFound": "Unfortunately the given record was not found. Please check your input details."
}
```


### Friendly Messages
   
Some messages returned from the server are too technical for users. We want to differentiate between those messages and user friendly messages. See example below.
    
To enable Friendly messages you just need to edit the *config/default.json* and specify which messages are the friendly ones. The messages itself have to be defined in the *config/messages.json* (or *config/locales.json* if you are using localization).

```
config/default.json

{
    "friendlyMessages": ["welcome", "notFound"]
}
```
    
Once the message name is in the above array, the response object will have a key *friendlyMessage* which makes it easy for front-end to differentiate between messages.


## Localization

Sometimes you need to support more than one language.

1. This is a basic setup for the [i18n-nodejs](https://github.com/eslam-mahmoud/i18n-nodejs).
```
config/default.json

{
    "lang": "en",
    "langFile": "../../config/locales.json" 
}
```

`lang` is the default fallback language of your application

`langFile` path is used in the library therefore, the path is relative to the *index.js* file from *help-me-respond* folder

2. Create `config/locales.json` file and add some messages there.

```
config/locales.json

{
    "SHARING_ERROR": {
	"en": "You cannot share the link with yourself."
    },
    "NOT_OWNER": {
	"en": "You are not the owner."
    }
}
```

### Pass arguments to your messages

This works only if you are using localization.

```
config/locales.json

{
    "welcome": "Welcome dear {{name}}"
}
```

```
  http200(res, JSON.stringify({
    msg: 'welcome',
    args: {
      name: 'Mike'
    }
  }))
 ```

## API

**res** - Express res object

**msg** - string message, Error object, any other object

**headers** - array with http headers. If none provided the only header set by the library will be *Content-type*: *application/json*



#### http400(res, msg, headers)
returns HTTP response with 400 error code
If no message is specified will return message - BAD REQUEST


#### http404(res, msg, headers)
returns HTTP response with 404 error code
If no message is specified will return message - NOT FOUND


#### http403(res, msg, headers)
returns HTTP response with 403 error code
If no message is specified will return message - FORBIDDEN


#### http401(res, msg, headers)
returns HTTP response with 403 error code
If no message is specified will return message - UNAUTHENTICATED

#### http200(res, msg, headers)
returns HTTP response with 201 success code


#### http201(res, msg, headers)
returns HTTP response with 201 success code


#### http204(res, msg, headers)
returns HTTP response with 204 code


#### rCode(code, res, msg, headers)
general function to return any HTTP code you want


## Response examples

### Success messages
```
{
   "friendlyMessage": "I am a friendly successfull message"
}
```

```
{
  "message": "I am a very technical success message that users do not want to see"
}
```

### Error messages
```
{
  "error": {
    "friendlyMessage": "I am a friendly error message"
  }
}
```

```
{
  "error": {
    "message": "I am a very technical error message that users do not want to see",
    "stack": [
      'at /Users/XX/Projects/help-me-respond/test/responseErrorTest.js:28:9',
      'at DUMMY_ERROR_OBJECT (/Users/Dasha/Projects/help-me-respond/test/responseErrorTest.js:27:9)',
      'at Context.<anonymous> (/Users/Dasha/Projects/help-me-respond/test/responseErrorTest.js:72:3)',
      'at callFn (/usr/local/Cellar/node/0.12.7/libexec/npm/lib/node_modules/mocha/lib/runnable.js:334:21)'
    ]    
  }
}
```

### Data response


```
{
  "data": {
    "item1": "name",
    "item2": "name"
  }
}
```
