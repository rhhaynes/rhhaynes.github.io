---
layout: post
title:      "Custom Google Maps Styles"
date:       2018-09-23 17:43:03 +0000
permalink:  custom_google_maps_styles
---

For map-based applications built around Google Maps, although the default color scheme is generally preferred given most people's familiarity with it, occasionally it may be necessary to customize a map for better viewing experiences.  I recently ran into this issue while working on an app for displaying weather and geological databases.  Unfortunately the multitude of colors in the default map style made it difficult to visualize colored data markers and polylines.  As such for this blog post I've decided to share a quick and easy way to customize map styles in React applications using the `react-google-maps` library.

## Google Maps APIs Styling Wizard
To create custom map styles, Google provides an [easy-to-use app for modifying colors, roads, labels, etc](https://mapstyle.withgoogle.com/).  Additionally there are several alternate themes made available in addition to the standard one.  After creating or selecting a custom theme, one only needs to export the custom style as JSON to then be copied/pasted into your app.  For example the premade export for `Night` theme looks as follows.
```
[
  {
    "elementType": "geometry",
    "stylers": [{"color": "#242f3e" }]
  },
  {
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#746855" }]
  },
  {
    "elementType": "labels.text.stroke",
    "stylers": [{ "color": "#242f3e" }]
  },
  {
    "featureType": "administrative.locality",
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#d59563" }]
  },
  {
    "featureType": "poi",
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#d59563" }]
  },
  {
    "featureType": "poi.park",
    "elementType": "geometry",
    "stylers": [{ "color": "#263c3f" }]
  },
  {
    "featureType": "poi.park",
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#6b9a76" }]
  },
  {
    "featureType": "road",
    "elementType": "geometry",
    "stylers": [{ "color": "#38414e" }]
  },
  {
    "featureType": "road",
    "elementType": "geometry.stroke",
    "stylers": [{ "color": "#212a37" }]
  },
  {
    "featureType": "road",
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#9ca5b3" }]
  },
  {
    "featureType": "road.highway",
    "elementType": "geometry",
    "stylers": [{ "color": "#746855" }]
  },
  {
    "featureType": "road.highway",
    "elementType": "geometry.stroke",
    "stylers": [{ "color": "#1f2835" }]
  },
  {
    "featureType": "road.highway",
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#f3d19c" }]
  },
  {
    "featureType": "transit",
    "elementType": "geometry",
    "stylers": [{ "color": "#2f3948" }]
  },
  {
    "featureType": "transit.station",
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#d59563" }]
  },
  {
    "featureType": "water",
    "elementType": "geometry",
    "stylers": [{ "color": "#17263c" }]
  },
  {
    "featureType": "water",
    "elementType": "labels.text.fill",
    "stylers": [{ "color": "#515c6d" }]
  },
  {
    "featureType": "water",
    "elementType": "labels.text.stroke",
    "stylers": [{ "color": "#17263c" }]
  }
]
```

## Applying Custom Styles
Without going into detail on how to use `react-google-maps` components, to apply the custom map style one only needs to include the styling commands in the options property for `<GoogleMap />` as shown below.  Obviously for this case the JSON export has been saved in its own file `Night.js` which is being imported.  Happy coding!
```
import React, { Component } from 'react';
import { withGoogleMap, GoogleMap } from 'react-google-maps';
import { Night } from './styles/Night';

class Map extends Component {

  render() {
    const GoogleMapExample = withGoogleMap( props => (
      <GoogleMap
        options={ { styles: Night } }
      />
    ));

    return(
      <div>
        <GoogleMapExample
          containerElement={ <div style={ { height: `575px`, width: `1000px` } } /> }
          mapElement={ <div style={ { height: `100%` } } /> }
        />
      </div>
    );
  }
};

export default Map;
```
