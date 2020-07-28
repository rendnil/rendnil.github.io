---
layout: post
title: RSpec Testing - How to Stub REST Requests
---
Whether you're writing basic unit tests or more robust integration specs, using stubs for REST requests is generally considered good practice. Since you want to focus on the functionality of your specific application, at this level of testing it's useful to just set expectations for how an external service will respond. Further, it can be frustrating to rely on the performance and availability of an application that you do not have control over.

When I write RSpec tests for Rails applications, I often use the <a href="https://github.com/bblimke/webmock" target="_blank">WebMock</a> Ruby gem. Overall, this library is easy to set up and configure to stub out any REST calls that my application performs.

---

## INSTALLATION
For RSpec test suites, the installation and set up is pretty lightweight.

You can install the gem from the command line
```
gem install webmock
```

Or, you can add it to your `Gemfile` and run `bundle install`

Once it's installed, just add this line to your `spec_helper` file

```
require 'webmock/rspec'
```

---

## USE CASES

The simplest example is to just stub out a standard `GET` request and set it to return some json

```ruby
stub_request(:get, "someurl.com")
  .to_return(status: 200, body: some_json)
```

You can also set query parameters. In this case, the stub will only work for requests that match the url and the specified query params.

```ruby
stub_request(:get, "someurl.com")
  .with(query: {search_key: value})
  .to_return(status: 200, body: some_json)
```

WebMock also works with other HTTP verbs, and you can use it to test how your application handles varying HTTP status codes.

```ruby
stub_request(:post, "someurl.com")
  .with(body: {})
  .to_return(status: 404, body: some_json)
```

This quick guide will help you get started, but you should definitely review the <a href="https://github.com/bblimke/webmock" target="_blank">documentation</a> to learn about the different customization options.
