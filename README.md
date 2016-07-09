# Documentation

*Note: More documentation and examples to come. This is a work in progress as code is generalized from another application. This library is not yet on pypi, but will be soon.*

django-lti-provider provides LTI functionality for the Django web framework. This
work began as a port of MIT's [LTI Flask Sample](https://github.com/mitodl/mit_lti_flask_sample),
which demonstrates a sample LTI provider for the Flask Framework based on 
the [Python LTI library, PyLTI](https://github.com/mitodl/pylti).

Additional work was completed to provide fuller functionality and support the idiosyncrasies of various LMS systems
such as Canvas, Blackboard, Moodle and EdEx.

django-lti-provider offers:
* an authentication backend to complete an oAuth handshake (optional)
* a templated view for config.xml generation
* a templated landing page view for those LMS who do not have a 'launch in new tab' option, i.e. Canvas
* support for Canvas' [embedded tool extensions](https://canvas.instructure.com/doc/api/file.editor_button_tools.html)

The library is used at Columbia University's [Center for Teaching And Learning](http://ctl.columbia.edu) in [Mediathread](http://www.github.com/ccnmtl/mediathread) and the RedirectPlus tool (upcoming).


## Installation

You can install ```django-lti-provider``` through ```pip```::
```python
$ pip install django-lti-provider
```
In your project, add ```django-lti-provider``` to your ```requirements.txt```.

Add to ```INSTALLED_APPS``` in your ```settings.py```::
```python
  'lti_provider',
```

## Dependencies

Django==1.9.7
six==1.10.0
nameparser==0.4.0
httplib2==0.9.2
oauth==1.0.1
oauth2==1.9rc1
oauthlib==0.6.3
pylti>=0.1.3

## Configuration

Add the URL route::
```python
(r'^lti/', include('lti_provider.urls'))
```
The ``LTI_TOOL_CONFIGURATION`` variable in your ``settings.py`` allows you to
configure your application's config.xml. ([Edu Apps](https://www.edu-apps.org/code.html) has good documentation
on configuring an lti provider through xml.)
```
LTI_TOOL_CONFIGURATION = {
    'title': '<your lti provider title>',
    'description': '<your description>'
    'launch_url': 'lti/',
    'embed_url': '<the view endpoint for an embed tool>' or '',
    'embed_icon_url': '<the icon url to use for an embed tool>' or '',
    'embed_tool_id': '<the embed tool id>' or '',
    'access': '<public|anonymous>'
}
```

The ```LTI_AUTHENTICATE``` variable in your ```settings.py``` turns on authentication for all LTI flows.
```python
LTI_AUTHENTICATE = True
```

Also add the LTIBackend to your AUTHENTICATION_BACKENDS:
```python
AUTHENTICATION_BACKENDS = [
  'django.contrib.auth.backends.ModelBackend',
  'lti_auth.auth.LTIBackend',
]
```

To pass through extra LTI parameters to your provider, populate the ```LTI_EXTRA_PARAMETERS``` variable
in your ```settings.py```
```
LTI_EXTRA_PARAMETERS = ['lti_version']
```

