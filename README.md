<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Map with Markers</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <style>
        #map { /* Define map container size */
            height: 600px;
        }
    </style>
</head>
<body>
    <div id="map"></div> <!-- Map container -->
    
    <!-- Include Leaflet.js library -->
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    
    <script>
        var map = L.map('map').setView([51.505, -0.09], 13); // Set initial map view and zoom level

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 19,
            attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);

        var markers = []; // Array to store markers

        // Function to handle click on the map
        function onMapClick(e) {
            var marker = L.marker(e.latlng, { draggable: true }).addTo(map); // Add draggable marker at clicked location
            marker.bindPopup("Marker added at " + e.latlng.toString() + ". Click again to remove.").openPopup(); // Display popup with coordinates
            markers.push(marker); // Add marker to array

            // Remove marker on click
            marker.on('click', function() {
                map.removeLayer(marker); // Remove marker from map
                markers = markers.filter(function(item) {
                    return item !== marker; // Remove marker from array
                });
            });
        }

        map.on('click', onMapClick); // Event listener for map click

        // Function to clear all markers
        function clearMarkers() {
            markers.forEach(function(marker) {
                map.removeLayer(marker); // Remove each marker from map
            });
            markers = []; // Empty the array
        }

        // Example button to clear markers
        var clearButton = L.Control.extend({
            options: {
                position: 'topright' // Position of the button on the map
            },
            onAdd: function() {
                var button = L.DomUtil.create('button', 'leaflet-control-layers leaflet-control');
                button.innerHTML = 'Clear Markers';
                button.onclick = function() {
                    clearMarkers(); // Call function to clear markers
                };
                return button;
            }
        });

        map.addControl(new clearButton()); // Add clear markers button to the map
    </script>
</body>
</html>

