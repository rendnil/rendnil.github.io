---
layout: post
title: Rails - Handling Record Insertion Race Conditions
---

## Classic Technique

New Rails programmers typically learn the `.find_or_create_by` method to avoid inserting duplicate records in the database. When invoking this method, Rails will search the database for a record with the specified attributes, and if it does not return one, it will create a new record.

For example, given a `Book` class with attributes of `title` and `author`. If we run the below, Rails will look in the Book table for a record with title of 'Dune' and author 'Frank Herbert.' When it finds a record with the below attributes, it will return the record, and if not, it will insert a record into the database.

```ruby
Book.find_or_create_by(title: 'Dune', author: 'Frank Herbert')
```

Peering into the Rails source code, the method first evaluates `.find_by` and then the `.create` method.

```ruby
def find_or_create_by(attributes, &block)
  find_by(attributes) || create(attributes, &block)
end
```

## Issues

Most of the time, this implementation will work fine, however, I have encountered situations where the method leads to a race condition.

The Rails documentation actually cautions against such cases:

>
Please note *this method is not atomic*, it runs first a SELECT, and if there are no results an INSERT is attempted. If there are other threads or processes there is a race condition between both calls and it could be the case that you end up with two similar records.
>

So, for example, let's say you have 2 instances of a service running and both use the repeatable read isolation level. If both services attempt to insert the record at the same time, they will not find an existing record in the database and will actually insert duplicate copies.

## Solution

The primary way to avoid this race condition is to use the similarly named `.create_or_find_by` method. This method first attempts to create the record in the database, and if it already exists, it returns the existing record with those attributes. By necessity, for this solution to work, the table needs to contain unique constraints on one or more of these attribute columns.

```ruby
Book.create_or_find_by(title: 'Dune', author: 'Frank Herbert')
```

Peering under the hood, Rails attempts to insert the record into the database, and rescues the `ActiveRecord::RecordNotUnique` error thrown when the unique database constraint is violated.

```ruby
def create_or_find_by(attributes, &block)
  transaction(requires_new: true) { create(attributes, &block) }
rescue ActiveRecord::RecordNotUnique
  find_by!(attributes)
end
```

The above implementation should resolve the race conditions caused by the classic `.find_or_create_by` method.

---
## SOURCES
<a href="https://apidock.com/rails/v4.0.2/ActiveRecord/Relation/find_or_create_by" target="_blank">find_or_create_by</a>
<br/>
<br/>
<a href="https://apidock.com/rails/v6.0.0/ActiveRecord/Relation/create_or_find_by" target="_blank">create_or_find_by</a>
