---
layout: post
title:      "March Madness Table Relations"
date:       2018-04-03 00:50:01 +0000
permalink:  march_madness_table_relations
---

Given that it's March/April I decided to base my Sinatra portfolio project on the NCAA Men’s Division I Basketball Tournament:  March Madness.  To simplify things I focused on allowing a user to create an account and fill out a bracket as opposed to implementing logic that compares a user’s bracket to the actual tournament results.  Not surprisingly one aspect that made my programming life significantly easier was table relations.  Through these relations I was able to create a powerful application using only a minimal amount of code.

As an overview I built my application using the following models (classes) and associations.

Application Models:
* User
* Bracket
* Game
* Team

Model Associations / Table Relations:
* a User ```has_one``` Bracket and a Bracket ```belongs_to``` a User
* a Bracket ```has_many``` Games and a Game ```belongs_to``` a Bracket
* a Game ```has_and_belongs_to_many``` Teams
* a Game ```belongs_to``` a winner and also a loser
* a Team ```has_and_belongs_to_many``` Games
* a Team ```has_many``` games_won and also games_lost

Considering the first few relations between a User, Bracket and Game, it should be apparent that a user can have only one bracket and that each bracket will contain several games — 63 to be exact.  Such ```has/belongs_to``` relations work by requiring a foreign key to exist in the table of the model declaring the ```belongs_to``` association.  For example because a Game ```belongs_to``` a Bracket, each game should contain a bracket_id foreign key that indicates which bracket it associates with.  By utilizing these types of relationships one can find, create, and destroy instances of each model type and implicitly trigger their associations.

To achieve similar functionality between a Game and Team which have a many-to-many relationship, the ```has_and_belongs_to_many``` method is used in conjunction with a join table.  The join table holds nothing more than foreign keys of each model type to signify their associations.  Using this type of relationship allows each game (specific to only 1 bracket) to contain 2 teams and for each team to keep track of every game (across all brackets) that it is involved.  What’s more, including the custom ```has_many/belongs_to``` relationship between a Game and Team enables tracking of the winners and losers.  By including custom foreign keys corresponding to the winning and losing team IDs in each game, a game will have exactly one winning and losing team, and each team will store all games for which it was the winner and loser.
