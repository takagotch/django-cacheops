### django-cacheops
---
https://github.com/Suor/django-cacheops

```py
CACHEOPS_REDIS = {
  'host': 'localhost',
  'port': 6379,
  'db': 1,

  'socket_timeout': 3,
  'password': '...',
  'unix_socket_path': ''
}

CACHEOPS_REDIS = "redis://localhost:6379/1"

CACHEOPS_REDIS = "unix://path/to/@localhost:6379/1"

CACHEOPS_REDIS = "redis://:password@localhost:6379/1"

CACHEOPS_SENTINEL = {
  'locations': [('localhost', 26379)],
  'service_name': 'mymaster',
  'socket_timeout': 0.1,
  'db': 0
}

CACHEOPS_CLIENT_CLASS = 'your.redis.ClientClass'

CACHEOPS = {
  'auth.*': {'ops': 'get', 'timeout': 60*15},
  
  'auth.*': {'ops': {'fetch', 'get'}, 'timeout': 60*60},
  
  'auth.permission': {'ops': 'all', 'timeout': 60*60},
  
  '*.*': {'timeout': 60*60},
  
  '*.*': {'timeout': 60*60},
  
  'some_app.*': None,
}

CACHEOPS_DEFAULTS = {
  'timeout': 60*60
}
CACHEOPS = {
  'auth.user': {'ops': 'get', 'timeout': 60*15},
  'auth.*': {'ops': ('fetch', 'get')},
  'auth.permission': {'fetch', 'get'},
  '*.*': {},
}

CACHEOPS_DEGRADE_ON_FAILURE = True


from django.test import override_settings

@override_settings(CACHEOPS_ENABLED=False)
def test_something():
  assert cond
```

```sh
pip install django-cacheops
git clone git://github.com/Suor/django-cacheops.git
pip install -e django-cacheops
```

```py
Article.objects.filter(tag=2).cache()













```


