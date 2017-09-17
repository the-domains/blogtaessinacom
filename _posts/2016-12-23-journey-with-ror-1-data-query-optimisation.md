---
datePublished: '2017-09-17T10:18:28.648Z'
inFeed: true
hasPage: true
author: []
via: {}
dateModified: '2017-09-17T10:18:27.577Z'
title: 'Journey with RoR #1 - Data Query Optimisation'
publisher: {}
description: >-
  Ruby on Rails has provided a very convenient method to do interactions with
  databases. Relationships between Models can be easy established via simple
  setup,
sourcePath: _posts/2016-12-23-journey-with-ror-1-data-query-optimisation.md
starred: false
datePublishedOriginal: '2016-12-23T18:39:14.443Z'
url: journey-with-ror-1-data-query-optimisation/index.html
_type: Article

---
# Journey with RoR \#1 - Data Query Optimisation
![](https://imgflo.herokuapp.com/graph/2b2431f8e7ba7b0/1291aad9a9147a87a3d9303acdd455b2/croprotate.png?cropheight=133&cropwidth=376&degrees=0&input=https%3A%2F%2Fthe-grid-user-content.s3-us-west-2.amazonaws.com%2F3fbfbcd8-a0a8-4fc4-861e-e2498422fa33.png&x=2&y=0)

Ruby on Rails has provided a very convenient method to do interactions with databases. Relationships between Models can be easy established via simple setup,

1. adding references to the child
![evaluation has_one report](https://the-grid-user-content.s3-us-west-2.amazonaws.com/40d5e167-1242-49f2-b948-3ec91c890225.png)

---

    $ rails g migration addReferencesToChild parent:references

1. Define relationships at Models

    # app/models/parent.rb
      
    class Parent < ApplicationRecord
     has_many :childs
    end
    
    class Child < ApplicationRecord
     belongs_to :parent
    end

With this, we can now easily query for Child that associated with Parent.

    # at rails console
    >  parent.childs

---

With the convenience provided by RoR, we could easily perform queries whenever needed. However, it is not always efficient in terms of data query.

In the case study below, we have setup a simple relationship model to test out the efficiency of the query stated.

To be able to generate the view as above, the command below was used.

    @evaluations = current_user.evaluations.order('created_at DESC')

The log of the output is recorded and is shown in the image below.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/3c4b851b-1049-45d2-81a6-200eb2e5d5ea.png)

As shown in the image, for each and every loan evaluations in the view, there will be another additional query to get the report information.

In the effort to improve the performance of the site, the method of query is updated.

    @evaluations = current_user.evaluations.includes(:report).order('created_at DESC')

This time, "report" was included as the query is performed.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/08eb8a71-4ddb-45b4-bc12-0bab7418b2ba.png)

As shown in the above, the number of queries is greatly reduced, resulting in the improvement of performance during the query.

As a summary,

    .includes()

The use of include while querying parent/child object will help optimize data query