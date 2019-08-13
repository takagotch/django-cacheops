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

{% cached_as <queryset> [, timeout=<timeout> [, extra=<key addition>]]%}
{% endcached_as %}

{% cached [timeout=<timeout>] [, extra=<key addition>] %}
{% endcached %}
```

```sh
pip install django-cacheops
git clone git://github.com/Suor/django-cacheops.git
pip install -e django-cacheops
```

```py
Article.objects.filter(tag=2).cache()

qs = Article.objects.filter(tag=2).cache(ops=['count'])
paginator = Paginator(objects, ipp)
articles = list(pager.page(page_num))

qs = Article.objects.filter(visible=True).nocache()
qs1 = qs.filter(tag=2)
qs2 = qs.filter(category=3)

from cacheops import cached_as

@cached_as(Article, timeout=120)
def article_stats():
  return {
    'tags': list(Article.objects.value('tag').annotate(Count('id')))
    'categories': list(Article.objects.values('category').annotate(Count('id')))
  }

def articles_block(category, count=5):
  qs =Article.objects.filter(category=category)
  
  @cached_as(qs, extra=count)
  def _articles_block():
    articles = list(qs.filter(photo=True)[:count])
    if len(articles) < count:
      articles += list(qs.filter(photo=False)[:count-len(articles)])
    return articles
    
  return _articles_block()


@cached_as(Article.objects.filter(public=True), Tag)
def article_stats():
  return {...}

from cacheops import cached_view_as

@cached_view_as(News)
def new_index(request):
  return HttpResponse(...)

class NewsIndex(ListView):
  model = News

news_index = cached_view_as(News)(NewsIndex.as_view())


from cacheops import invalidate_obj, invalidate_model, invalidate_all
invalidate_obj(some_article)
invalidate_model(Article)
invalidate_all()



from cacheops import no_invalidation
with no_invalidation:
  obj.save()
  
@no_invalidation
def some_work(...):
  obj.save()
  
try:
  with no_invalidation:
finally:
  invalidate_obj(...)

qs.invalidated_update(...)

from cacheops import cached, cached_view

@cached(timeout-number_of_seconds)
def top_articles(category):
  return ...
  
@cached_view(timeout=number_of_seconds)
def top_articles(request, category=None):
  return HttpResponse(...)

@property
def articles_json(self):
  @cached(timeout=10*60, extra=self.category_id)
  def _articles_json():
    return json.dumps(...)
    
  return _articles_json()

top_articles.invalidate(some_category)
top_articles.key(some_category).set(new_value)
  
top_articles.invalidate('http://example.com/page', some_category)

from cacheops import cache
cache.set(cache_key, data, timeout=None)
cache.get(cache_key)
cache.delete(cache_key)

from cacheops import cache, CacheMiss
try:
  result = cache.get(key)
except CacheMiss:


from cacheops import file_cache
@file_cache.cached(timeout=number_of_seconds)
def top_articles(category):
  return ...
  
@file_cache.cached_view(timeout=number_of_seconds)
def top_articles(request, category):
  return HttpResponse(...)

top_articles.invalidate(some_category)
top_articles.key(some_category).set(some_value)
file_cache.set(cache_key, data, timeout=None)
file_cache.get(cache_key)
file_cache.delete(cache_key)

from cacheops import invalidate_fragment
invalidate_fragment(fragment_name, extra1, ...)

from cacheops import cached_as, CacheopsLibrary
register = CacheopsLibrary()
@register.decorator_tag(takes_context=True)
def cache_menu(context, menu_name):
  from django.utils import transaction
  from myapp.models import Flag, MenuItem
  
  request = context.get('request')
  if request and request.user.is_staff():
    return lambda func: func
    
  return cache_as(
    MenuItem,
    Flag.objects.filter(name='menu'),
    extra=("menu", menu_name, transaction.get_language()),
    timeout=24 * 60 * 60
  )

@cached_as(qs, lock=True)
def heavy_func(...):
for item in qs.cache(lock=True):

CACHEOPS = {
  'some.model': {'ops': 'get', 'db_agnostic': False, 'timeout': ...}
}

CACHEOPS_PREFIX = lambda query ...
CACHEOPS_PREFIX = 'some.module.cacheops_prefix'

cacheops_prefix = lambda _: get_request().get_host()

def cacheops_prefix(query):
  query.dbs
  query.tables
  
  if set(query.tables) <= HELPER_TABLES:
  if query.tables == ['blog_post']:
    return 'blog:'
  if query.tables == ['blog_post']:
    return 'blog:'

from cacheops.signals import cache_read
from statsd.defaults.django import statsd
def stats_collector(sender, func, hit, **kwargs):
  event = 'hit' if hit else 'miss'
  statsd.incr('cacheops.%s' % event)

cache_read.connect(stats_collector)


@cache_as(qs, lock=True)
def heavy_func(...):
for item in qs.cache(lock=True):
```


