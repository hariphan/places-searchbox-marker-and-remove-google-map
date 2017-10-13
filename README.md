### places_searchbox_marker-and_remove
Places Searchbox
# 
    var markers = [];
    function initAutocomplete() {
        var map = new google.maps.Map(document.getElementById('map'), {
            center: {lat: -33.8688, lng: 151.2195},
            zoom: 13,
            mapTypeId: 'roadmap'
        });

        map.addListener('click', function(even) {
            placeMarkerAndPanTo(even.latLng, map);
        });

        var input = document.getElementById('pac-input');
        var searchBox = new google.maps.places.SearchBox(input);
        map.controls[google.maps.ControlPosition.TOP_LEFT].push(input);
  
        map.addListener('bounds_changed', function() {
            searchBox.setBounds(map.getBounds());
        });
  
        searchBox.addListener('places_changed', function() {
            deleteMarkers();
            var places = searchBox.getPlaces();
  
            if (places.length == 0) {
              return;
            }
  
            // Clear out the old markers.
            markers.forEach(function(marker) {
              marker.setMap(null);
            });
            markers = [];
  
            // For each place, get the icon, name and location.
            var bounds = new google.maps.LatLngBounds();
            places.forEach(function(place) {
            if (!place.geometry) {
                console.log("Returned place contains no geometry");
                return;
            }
            var icon = {
                url: place.icon,
                size: new google.maps.Size(71, 71),
                origin: new google.maps.Point(0, 0),
                anchor: new google.maps.Point(17, 34),
                scaledSize: new google.maps.Size(25, 25)
            };
  
              // Create a marker for each place.
            markers.push(new google.maps.Marker({
                map: map,
                title: place.name,
                position: place.geometry.location
            }));
  
            if (place.geometry.viewport) {
                // Only geocodes have viewport.
                bounds.union(place.geometry.viewport);
            } else {
                bounds.extend(place.geometry.location);
            }
        });
        map.fitBounds(bounds);
        });
    }
```
    map.addListener('click', function(even) {
        placeMarkerAndPanTo(even.latLng, map);
    });
```

Markers Location

    function placeMarkerAndPanTo(latLng, map) {
        deleteMarkers();
        var marker = new google.maps.Marker({
        position: latLng,
            map: map
        });
        markers.push(marker);
    }
```
deleteMarkers();
```
Delete Markers

    function deleteMarkers() {
        if(markers){
            for(i in markers){
                markers[i].setMap(null);
            }
            markers.length = 0;
        }
    }