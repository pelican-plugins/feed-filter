# Feed Filter

[![Build Status](https://img.shields.io/github/actions/workflow/status/pelican-plugins/feed-filter/main.yml?branch=master)](https://github.com/pelican-plugins/feed-filter/actions)
[![PyPI Version](https://img.shields.io/pypi/v/pelican-feed-filter)](https://pypi.org/project/pelican-feed-filter/)

Feed Filter is a Pelican plugin that filters elements from feeds.

Installation
------------

This plugin can be installed via:

    python -m pip install pelican-feed-filter

Usage
-----

This plugin is configured through your Pelican configuration file, by setting the following variable:

`FEED_FILTER = {}`

Define feed paths and include/exclude filters to apply to matching feeds. Both feed paths and filters are matched using [Unix shell-stye wildcards][1].

Filters are defined as:
* `include.item attribute`
* `exclude.item_attribute`

where `item_attribute` can be any [feed item attribute][2], ie. `title`, `link`, `author_name`, `categories`, ...

You can also match `pubdate` and `updateddate` item attributes as strings formatted with the following format: `%a, %d %b %Y %H:%M:%S` (e.g. 'Thu, 28 Jun 2001 14:17:15')

**Filter priorities**

If an inclusion filter is defined, only feed elements that match the filter will be included in the feed.

If an exclusion filter is defined, all feed elements except those which match the filter will be included in the feed.

If both include and exclude filters are defined, all feed elements except those which match some exclusion filter but no inclusion filter, will be included in the feed.

If multiple inclusion/exclusion filters are defined for the same feed path, a single match is enough to include the item in the feed.

Usage examples
--------------

* Include only posts in some categories into the global feed:
```
FEED_ATOM = 'feed/atom'
FEED_RSS = 'feed/rss'
FEED_FILTER = {
    'feed/*': {
        'include.categories': ['software-*', 'programming']
    }
}
```

* Exclude an author from a category feed:
```
CATEGORY_FEED_ATOM = 'feed/{slug}.atom'
CATEGORY_FEED_RSS = 'feed/{slug}.rss'
FEED_FILTER = {
    'feed/a-category-slug.*': {
        'exclude.author_name': 'An Author name'
    }
}
```

* Exclude an author from all category feeds:
```
CATEGORY_FEED_ATOM = 'feed/{slug}.atom'
CATEGORY_FEED_RSS = 'feed/{slug}.rss'
FEED_FILTER = {
    'feed/*.*': {
        'exclude.author_name': 'An Author name'
    }
}
```

* In the global feed, exclude all posts in a category, except if written by a given author:
```
FEED_ATOM = 'feed/atom'
FEED_RSS = 'feed/rss'
FEED_FILTER = {
    'feed/*': {
        'include.author_name': 'An Author name',
        'exclude.category': 'software-development'
    }
}
```

* In the global feed, exclude all posts whose title starts with "Review":
```
FEED_ATOM = 'feed/atom'
FEED_RSS = 'feed/rss'
FEED_FILTER = {
    'feed/*': {
        'exclude.title': 'Review*'
    }
}
```

* In the global feed, include all posts written by a given author OR in a certain category, except if the title starts with "Review":
```
FEED_ATOM = 'feed/atom'
FEED_RSS = 'feed/rss'
FEED_FILTER = {
    'feed/*': {
        'include.author_name': 'An Author name',
        'include.category': 'software-development'
        'exclude.title': 'Review*'
    }
}
```

Contributing
------------

Contributions are welcome and much appreciated. Every little bit helps. You can contribute by improving the documentation, adding missing features, and fixing bugs. You can also help out by reviewing and commenting on [existing issues][].

To start contributing to this plugin, review the [Contributing to Pelican][] documentation, beginning with the **Contributing Code** section.

[1]: https://docs.python.org/3/library/fnmatch.html "Fnmatch Python module"
[2]: https://github.com/getpelican/feedgenerator/blob/master/feedgenerator/django/utils/feedgenerator.py#L132 "Feed item attributes"
[existing issues]: https://github.com/pelican-plugins/feed-filter/issues
[Contributing to Pelican]: https://docs.getpelican.com/en/latest/contribute.html
