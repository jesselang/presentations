# JSON API
Jesse Lang | jesselang.com

---

## Aren't most APIs "JSON APIs"?

Well, sort of.
<!-- .element: class="fragment" data-fragment-index="1" -->

Like snowflakes,
<!-- .element: class="fragment" data-fragment-index="2" -->

No two are alike.
<!-- .element: class="fragment" data-fragment-index="3" -->

---

## JSON API to the rescue!

"A Specification For Building APIs in JSON"

http://jsonapi.org/

Note:
JSON API is designed to help you avoid spending countless hours
bike-shedding over how your API should be organized and formatted.

---

### Highlights

JSON API offers:

* Conventions to describe
    * Resources
    * Relationships between resources
    * Errors

* Other optional niceties
    * Inclusion of related resources
    * Sorting
    * Pagination

Note:
The spec is terse about filtering.

---

### Fetch a collection of articles

```
GET /articles HTTP/1.1
Accept: application/vnd.api+json
```
```
{
  "links": {
    "self": "http://example.com/articles"
  },
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "JSON API paints my bikeshed!"
    }
  }, {
    "type": "articles",
    "id": "2",
    "attributes": {
      "title": "Rails is Omakase"
    }
  }]
}
```

---

### Fetch a single article

```
GET /articles/1 HTTP/1.1
Accept: application/vnd.api+json
```
```
{
  "type": "articles",
  "id": "1",
  "attributes": {
    "title": "Rails is Omakase"
  },
  "relationships": {
    "author": {
      "links": {
        "self": "/articles/1/relationships/author",
        "related": "/articles/1/author"
      },
      "data": { "type": "people", "id": "9" }
    }
  }
}
```

---

### CRUD your article

```
POST /articles HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```
```
GET /articles/1 HTTP/1.1
Accept: application/vnd.api+json
```
```
PATCH /articles/1 HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```
```
DELETE /articles/1 HTTP/1.1
Accept: application/vnd.api+json
```

---

### Fetch the article's author

```
GET /articles/1/author HTTP/1.1
Accept: application/vnd.api+json
```
```
{
  "type": "people",
  "id": "9",
  "attributes": {
    // ...
  },
  "relationships": {
    // ...
  }
}
```

---

### Fetch the *relationship* to the author

```
GET /articles/1/relationships/author HTTP/1.1
Accept: application/vnd.api+json
```
```
{
  "links": {
    "self": "/articles/1/relationships/author",
    "related": "/articles/1/author"
  },
  "data": {
    "type": "people",
    "id": "9"
  }
}
```

Note:
Relationships can be single, or multiple.

---

### Update the relationship to the author

```
PATCH /articles/1/relationships/author HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "data": { "type": "people", "id": "12" }
}
```
Or remove the relationship:
```
PATCH /articles/1/relationships/author HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "data": null
}
```

Note:
The first request updates the author to a different person.
The second request clears the author.

---

### Inclusion of related resources

```
GET /articles/1?include=author HTTP/1.1
Accept: application/vnd.api+json
```
```
{
  "data": {
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "Rails is Omakase"
    },
    // ...
  },
  "included": {
    "type": "people",
    "id": "9",
    "attributes": {
      "first-name": "Jodi",
      "last-name": "Jones"
    }
  }
}
```

Note:
You can include single relationships like the article's author, or to-many
relationships like the article's comments, and you can include many
relationships into one request.

---

### Want to use JSON API?

The spec is considered stable but...
<!-- .element: class="fragment" data-fragment-index="1" -->

Most implementations have limited support
<!-- .element: class="fragment" data-fragment-index="2" -->


Good news for Django users!
<!-- .element: class="fragment" data-fragment-index="3" -->

https://django-rest-framework-json-api.rtfd.org/
<!-- .element: class="fragment" data-fragment-index="3" -->

Note:
I spent a few weeks in March 2016 trying several implementations
targeted toward SQLAlchemy and Flask for a project I was starting.
In the end, I used Django REST Framework. Beware, the docs are
far from complete.

---

# THANK YOU!
Jesse Lang | jesselang.com
