---
layout: post
title:      "Understanding ```has_many :through``` MyTravels"
date:       2018-05-18 15:36:58 +0000
permalink:  understanding_has_many_through_mytravels
---

For my Rails portfolio project I decided to create a content management system called MyTravels for making, organizing, and documenting travel plans.  Based on user experiences, the application provides insight into the world's most popular destinations and the features that make them unique.  Without going into detail, after creating an account users have the ability to make new travel plans.  For existing plans users can submit journal entries or notes in the form of travel logs to detail their upcoming itineraries or past experiences.

## MyTravels table relations
Ignoring travel log functionality for now, behind the scenes the MyTravels application is built upon three Active Record models with the following  associations.

```ruby
User
has_many :travels
has_many :destinations, :through => :travels
```

```ruby
Destination
has_many :travels
has_many :users, :through => :travels
```

```ruby
Travel
belongs_to :user
belongs_to :destination
```

Given this setup it should be apparent that a *many-to-many* relationship exists between the User and Destination models.  But why did we choose to define this relationship using a ```has_many :through``` approach which requires a separate join model (i.e., Travel)?  Especially when Rails offers a second and much simpler approach, ```has_and_belongs_to_many```, which only relies on a join table?

## Declaring many-to-many relationships
To understand this decision we need only reference the helpful [Rails Guide](http://guides.rubyonrails.org/association_basics.html#choosing-between-has-many-through-and-has-and-belongs-to-many) which states:

> The simplest rule of thumb is that you should set up a **has_many :through** relationship if you need to work with the relationship model as an independent entity.  If you don't need to do anything with the relationship model, it may be simpler to set up a **has_and_belongs_to_many** relationship (though you'll need to remember to create the joining table in the database).

So what does this mean?  Well in the context of MyTravels it means that we should use a ```has_many :through``` approach if the relationship model, in our case Travel, requires its own data or attributes.  And as it turns out, this is exactly our situation.

## Defining the join model
In the real world when one wishes to describe travel, it usually involves answering the following questions.

* Where are you going?
* Why are you going?
* When are you leaving?
* How long will you be there?

And because we've defined a Destination model to handle the *where* aspect of traveling, the question of which *many-to-many* relationship to use really comes down to whose responsibility is it to address the other questions.  Since it doesn't make sense to assign travel dates or purposes to a User, a relationship model acting as an independent entity works perfectly here.  By setting up a ```has_many :through``` relationship and creating a Travel model, we now have a very intuitive place to define travel attributes such as purpose, start date, and end date.

It should be noted that even if we could reasonably handle the *why*, *when*, and *for how long* questions with our User model — thus allowing us to leverage the simpler ```has_and_belongs_to_many``` approach — most experienced developers would still recommend using the ```has_many :through``` relationship.  Why?  Because it's hard to be 100% certain that you'll never need to work with join data over the course of a project.

## Safe travels
In summary one should only use the ```has_and_belongs_to_many``` approach when they are absolutely certain that the join data in a *many-to-many* association will serve no purpose...  almost never.  So if you're looking for a golden rule on the topic, stick with ```has_many :through``` for defining all *many-to-many* relationships.  Happy coding and safe travels!
