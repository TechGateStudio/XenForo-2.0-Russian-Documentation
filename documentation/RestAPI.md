# REST API
In XenForo 2.1, a REST API was added. This allows you to programmatically interact with many areas of a XenForo installation.

Accessing the API requires generating a key via the admin control panel. There is no unauthenticated access to the API and users cannot generate their own keys to access the API at this time.

The API for a specific XenForo installation is accessible at `<XenForo base URL>/api/`. All endpoints are prefixed by this URL. For example, if XenForo is installed at `https://example.com/community/`, then the API URLs will start with `https://example.com/community/api/`. In this example, accessing a list of threads would be done via `https://example.com/community/api/threads/`.

The API is enabled by default. If necessary, all API access can quickly be disabled by adding the following to src/config.php:

```php
$config['enableApi'] = false;
```

### API keys
API keys are created via the admin control panel by going to **Setup > API keys**. As generating API keys can allow access to highly privileged data, only super administrators may access this system. All super admins will receive an email when an API key is generated to ensure that the request is valid.

When a key is created, a random string will be generated and this will be used to authenticate yourself with the API. It is important that this key is kept secret. If you believe an API key has been compromised, you should immediately regenerate the key and update any code using the old key.