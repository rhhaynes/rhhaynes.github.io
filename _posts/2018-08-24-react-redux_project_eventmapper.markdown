---
layout: post
title:      "React-Redux Project:  EventMapper"
date:       2018-08-24 13:28:11 +0000
permalink:  react-redux_project_eventmapper
---

For the React-Redux project I decided to build a data-viewing application by integrating several geolocation databases with Google Maps. Without going into detail, a Rails API backend is responsible for persisting and providing earthquake, hurricane, and volcano positions (i.e., latitude and longitude coordinates) on request. A React-Redux frontend handles the user interactivity as well as the display of all geolocations. Rather than provide a comprehensive overview of the app, this blog post focuses on the integration of Google Maps via the `react-google-maps` library.

## Installing react-google-maps
As described in its documentation, `react-google-maps` provides a set of React components that wrap the underlying instances of Google Maps JavaScript API v3. For the purposes of this app, such wrappers enable the rendering of Google Maps with basic functionality (zoom, roadmap and satellite views, etc.), the overlay of geolocation data on the map in the form of polylines and markers, and the addition of pop-up, information windows to clarify the data. To actually install the library use the following command.
```javascript
npm install --save react-google-maps
```

## Using react-google-maps
Once the library is installed a [private API key](https://developers.google.com/maps/documentation/javascript/get-api-key) is required from Google before any maps can be rendered. The simplest way to include the key is by adding the following script to the header of the index.html file.
```html
<script src="https://maps.googleapis.com/maps/api/js?key=PRIVATE_API_KEY"></script>
```
At this point a blank map can be displayed using the generic map component below.
```javascript
import React, { Component } from 'react';
import { withGoogleMap, GoogleMap } from 'react-google-maps';

class Map extends Component {

   render() {
      const GoogleMapExample = withGoogleMap( props => (
         <GoogleMap
            defaultZoom={2}
            defaultCenter={{ lat: 26, lng: 0 }}
         >
         </GoogleMap>
      ));

      return(
         <div>
            <GoogleMapExample
               containerElement={<div style={{ height: `575px`, width: `1000px` }} />}
               mapElement={<div style={{ height: `100%` }} />}
            />
         </div>
      );
   }
};

export default Map;
```

## Customizing the map component
To customize the map with geolocation markers, the `Marker` component must be imported from `react-google-maps` and invoked with a `position` props as shown below. In this example the latitude and longitude coordinates correspond to the geolocation of Flatiron School. Happy coding!
```javascript
import React, { Component } from 'react';
import { withGoogleMap, GoogleMap, Marker } from 'react-google-maps';

class Map extends Component {

   render() {
      const GoogleMapExample = withGoogleMap( props => (
         <GoogleMap
            defaultZoom={2}
            defaultCenter={{ lat: 26, lng: 0 }}
         >
            <Marker position={{ lat: 40.705283, lng: -74.014025 }} />
         </GoogleMap>
      ));

      return(
         <div>
            <GoogleMapExample
               containerElement={<div style={{ height: `575px`, width: `1000px` }} />}
               mapElement={<div style={{ height: `100%` }} />}
            />
         </div>
      );
   }
};

export default Map;
```
