---
layout: post
title:      "Serializing Deeply Nested Associations"
date:       2018-09-17 03:41:33 +0000
permalink:  serializing_deeply_nested_associations
---

While it's generally preferred to keep data layers as shallow as possible, at some point when building APIs it may be necessary to include attributes spanning multiple relationships and levels.  Unfortunately the default nesting for `json` with `active_model_serializers` in `Rails` is only one layer.  This blog post aims to address this issue by exploring a couple ways to serialize deeply nested associations.

## Single-layer nesting by default
To illustrate the problem, imagine an API that provides geolocation data (i.e., latitutde and longitude values) for hurricanes.  In addition to actual hurricane positions, a user may be interested in the geolocations of all associated forecasts, or *spaghetti models*.  Behind the scenes, one way to persist and associate such information might be as follows.
```ruby
class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true
end
```
```ruby
class Geolocation < ApplicationRecord
  belongs_to :latlng, :polymorphic => true
end
```
```ruby
class Hurricane < ApplicationRecord
  has_many :geolocations, :as => :latlng
  has_many :spaghetti_models
end
```
```ruby
class SpaghettiModel < ApplicationRecord
  belongs_to :hurricane
  has_many :geolocations, :as => :latlng
end
```

Likewise the corresponding serializers using `active_model_serializers` are shown below.
```ruby
class GeolocationSerializer < ActiveModel::Serializer
  attributes :lat, :lng
  belongs_to :latlng, :polymorphic => true
end
```
```ruby
class HurricaneSerializer < ActiveModel::Serializer
  attributes :name, :category
  has_many :geolocations, :as => :latlng
  has_many :spaghetti_models
end
```
```ruby
class SpaghettiModelSerializer < ActiveModel::Serializer
	belongs_to :hurricane
	has_many :geolocations, :as => :latlng
end
```

Unfortunately using this setup our API output looks as follows for the 2018 Atlantic hurricane season.  As you can see, no geolocation data is contained under `spaghetti_models`.
```json
[
  {
    "name": "Hurricane Florence",
    "category": 4,
    "geolocations": [ {"lat": 12.9, "lng": -18.4}, {"lat": 12.9, "lng": -19.0}, ... ],
    "spaghetti_models": [ {}, {}, ... ]
  },
  ...
]
```

## Deeply nested serializations
To achieve the desired API output shown below, one of the following two approaches can be taken.
```json
[
  {
    "name": "Hurricane Florence",
    "category": 4,
    "geolocations": [ {"lat": 12.9, "lng": -18.4}, {"lat": 12.9, "lng": -19.0}, ... ],
    "spaghetti_models": [
      {
        "name": "forecast_1",
        "geolocations": [ {"lat": 12.9, "lng": -18.4}, {"lat": 13.2, "lng": -20.1}, ... ]
      },
      {
        "name": "forecast_2",
        "geolocations": [ {"lat": 12.9, "lng": -19.4}, {"lat": 13.1, "lng": -20.5}, ... ]
      },
      ...
    ]
  },
  ...
]
```

### Deep-nesting by default
To change the default nesting level, a configuration property can be set in an initializer for `active_model_serializers`.  Simply create `config/initializers/active_model_serializer.rb` and reset the `default_includes` for `ActiveModel::Serializer`.
```ruby
ActiveModel::Serializer.config.default_includes = '**' # (default '*')
```
It should be noted that problems with infinite recursion can occur using this approach unless care is taken.

### Customizing serializer behavior
A safer approach to achieving the desired result involves disregarding the `has_many`, `belongs_to` associations in the serializer and defining the collection of nested attributes directly.  In this way the serializer behavior can be customized to provide select attributes.
```ruby
class SpaghettiModelSerializer < ActiveModel::Serializer
  attributes :name, :geolocations

  def geolocations
    object.geolocations.collect do |geolocation|
      { :lat => geolocation.lat, :lng => geolocation.lng }
    end
  end
end
```
