# Flask Google Login

Written by [Alvin Wan](https://alvinwan.com) for educational purposes. See the
accompanying tutorial on
[How third-party login works](http://alvinwan.com/how-thirdparty-login-works/).

Convenience utility for implementing Google login with Flask.

This is more useful as a template than as an installable library. Either way,
it's an easy way to get up and running with Google login without much work.

## Get Started

Install the utility with pip.

```bash
pip install flask-google-login
```

Initialize the utility's manager with the Flask app.

```python
from flask_google_login import FlaskGoogleLogin

FlaskGoogleLogin(app)
```

**Note**: This application by default expects `client_secrets.json` to be in the
same directory as the application. This is the file downloaded from the Google
developers console. You can change this by passing the `client_secrets_path`
argument to the `FlaskGoogleLogin` constructor.

Alternatively, initialize with the app after construction.

```python
manager = FlaskGoogleLogin()
manager.init_app(app)
```

Here are several customizations you can make with this utility out of the box.

## Customize Google Login

You can change any of the usual Google login configurations:

- **Client secrets path:** This should be a JSON downloaded from the OAuth2
  client in your Google developers console. The file will contain both a client
  ID and a secret.
- **Scopes:** This is the list of scopes that your application is requesting
  access to. By default, the application only requests "basic" information such
  as name and email address.
- **Redirect URI:** This is the URI that Google will send a GET request to, with
  the login code attached to it. This must be an https URL -- no IP addresses or
  custom URL protocols (i.e., deeplinks). By default, this is your application's
  login callback page.

```python
manager = FlaskGoogleLogin(
    client_secrets_path='/path/to/client_secrets.json',
    scopes=['profile', 'email'],
    redirect_uri='https://example.com/login/callback'
)
```

## Customize routes

You can rename any of the routes. For example, say you want to use `/login` for
a general login page with several options. You could then redefine these
routes to be `/google/login`, for example.

```python
manager = FlaskGoogleLogin()
manager.init_app(app, login_endpoint='/google/login')
# creates a login route at `/google/login`
```

You can add custom handlers to all of the endpoints, to customize different
behaviors. For example, open a new browser with the authorization URL, instead
of opening in the current tab.

```python
import webbrowser

def handler(authorization_url):
    webbrowser.open(authorization_url)
    return f"If redirect fails, click <a href='{authorization_url}'>here</a>."

manager = FlaskGoogleLogin(authorization_url_handler=handler)
```

Finally, you can ask the manager to completely skip an endpoint and write one
from scratch, by "naming" the endpoint `None`.

```python
manager = FlaskGoogleLogin()
manager.init_app(app, login_endpoint=None)
```

