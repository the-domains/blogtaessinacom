---
inFeed: false
description: >-
  In this article, we’re going to add minitest and guard for more fun when
  testing in Ruby on Rails. This is a summary of Chapter 3.6 from
  https://www.railstutorial.org/
dateModified: '2017-09-17T10:32:11.901Z'
datePublished: '2017-09-17T10:32:12.628Z'
title: 'Journey with RoR #2 - Setting up for automated testing'
author: []
publisher: {}
via: {}
hasPage: true
sourcePath: _posts/2017-01-06-journey-with-ror-2-setting-up-dev-env-for-testing.md
starred: false
datePublishedOriginal: '2017-01-06T14:52:19.785Z'
url: journey-with-ror-2-setting-up-for-automated-testing/index.html
_type: Blurb

---
# Journey with RoR \#2 - Setting up for automated testing

In this article, we're going to add minitest and guard for _more fun_ when testing in Ruby on Rails. This is a summary of Chapter 3.6 from [https://www.railstutorial.org/][0]

---

Goals:

* Enhance test reporter by using visual
* Automatically run tests whenever file changes

Let's start off by adding these into your Gemfile, then run _bundle._

    # Gemfile
    
    group :test do
      gem 'rails-controller-testing', '0.1.1'
      gem 'minitest-reporters',       '1.1.9'
      gem 'guard',                    '2.13.0'
      gem 'guard-minitest',           '2.4.4'
    end

To get Rails tests report to format nicely as shown in the figure, add the following code block in test/test\_helper.rb.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/a090dd0b-4c1a-4c38-8fb5-89b1f80ba4f3.png)

    # test/test_helper.rb
    
    require "minitest/reporters"
    Minitest::Reporters.use!

---

Next up, we will use Guard to _guard_ (whatever that means xD) our files and run tests automatically whenever file changes are detected.

Run _bundle exec guard init, _this will create a Guardfile in your project root directory, which already populated with basic setups. Look into Guardfile and uncomment the Rails 4 section, as shown below. These settings tell Guard which file/folder to watch for file changes. At the time of this writing, it works on Rails 5 too.

    # Guardfile
    
    # Rails 4
    watch(%r{^app/(.+)\.rb$})                               { |m| "test/#{m[1]}_test.rb" }
    watch(%r{^app/controllers/application_controller\.rb$}) { 'test/controllers' }
    watch(%r{^app/controllers/(.+)_controller\.rb$})        { |m| "test/integration/#{m[1]}_test.rb" }
    watch(%r{^app/views/(.+)_mailer/.+})                   { |m| "test/mailers/#{m[1]}_mailer_test.rb" }
    watch(%r{^lib/(.+)\.rb$})                               { |m| "test/lib/#{m[1]}_test.rb" }
    watch(%r{^test/.+_test\.rb$})
    watch(%r{^test/test_helper\.rb$}) { 'test' }

Now run _bundle exec guard _and you will see something like the figure below.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/ebc9cdb5-c852-48c5-8ad2-4844cb9ab3c9.png)

Try to edit some files and save, tests that are related to the files you edited will automatically run, giving you a visual feedback. With this, we can run TDD efficiently, no more excuses for writing tests ;)

Lastly, append these into your .gitignore. Guard uses Spring server to speed up loading time, however it is known for its unexpected behaviour, such as sucking up system resources. In our journey, it rarely occurs to us, if somehow you're caught in that mess, refer to [Box3.4 of the book][1].

    # .gitignore
    
    # Ignore Spring files.
    /spring/*.pid

Alright we're done! Now start writing test and become a better programmer!

> Let's talk about unit tests. Unit tests are code that tests and makes assertions about code. In unit testing, we take a little part of code, say a method of a model, and test its inputs and outputs. Unit tests are your friend. The sooner you make peace with the fact that your quality of life will drastically increase when you unit test your code, the better. Seriously. 

---

[Here's a summary of what happened][2].

[0]: https://www.railstutorial.org/ "The Ruby On Rails Tutorial"
[1]: https://www.railstutorial.org/book/_single-page#aside-processes "The Ruby On Rails Tutorial"
[2]: https://gist.github.com/alvinthen/e19337e06cdf0c4a3034e2fbf81a90a5