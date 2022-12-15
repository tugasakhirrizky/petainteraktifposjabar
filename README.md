# petainteraktifposjabar
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Peta Interaktif</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://api.tiles.mapbox.com/mapbox-gl-js/v2.9.2/mapbox-gl.js"></script>
    <link
      href="https://api.tiles.mapbox.com/mapbox-gl-js/v2.9.2/mapbox-gl.css"
      rel="stylesheet"
    />
    <style>
      body {
        margin: 0;
        padding: 0;
      }
      h2,
      h3 {
        margin: 10px;
        font-size: 18px;
      }
      h3 {
        font-size: 16px;
      }
      p {
        margin: 10px;
      }
      .map-overlay {
        position: absolute;
        bottom: 0;
        right: 0;
        background: #fff;
        margin-right: 20px;
        font-family: Arial, sans-serif;
        overflow: auto;
        border-radius: 3px;
      }
      #map {
        position: absolute;
        top: 0;
        bottom: 0;
        width: 100%;
      }
      #features {
        top: 0;
        height: 100px;
        margin-top: 20px;
        width: 250px;
      }
      #legend {
        padding: 10px;
        box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
        line-height: 18px;
        height: 150px;
        margin-bottom: 40px;
        width: 100px;
      }
      .legend-key {
        display: inline-block;
        border-radius: 20%;
        width: 10px;
        height: 10px;
        margin-right: 5px;
      }
    </style>
  </head>
  <body>
    <div id="map"></div>
    <div class="map-overlay" id="features">
      <h2>Jangkauan Area Layanan Pos Provinsi Jawa Barat</h2>
    <div id="pd"><p> Radius 10.000 meter </p></div>
    </div>
    <div class="map-overlay" id="legend"></div>

    <script>
      // define access token
      mapboxgl.accessToken = 'pk.eyJ1IjoicmV6ZWtpcmV6ZWtpMSIsImEiOiJjbGJqaTNyNXMwZHBlM29ucmxvN2w0dWR3In0.YBdWa-MnwxkH-9LNtqnZtQ';

      // create map
      const map = new mapboxgl.Map({
        container: 'map', // container id
        style: 'mapbox://styles/rezekirezeki1/clboisgnf000e15rrnhftgkct' // map style URL from Mapbox Studio
      });

      // wait for map to load before adjusting it
      map.on('load', () => {
        // make a pointer cursor
        map.getCanvas().style.cursor = 'default';

        // set map bounds to the continental US
        map.fitBounds([
          [107.609968, -6.889852],
          [107.610000, -6.89000]
        ]);

        // define layer names
        const layers = [
          'Titik Layanan Pos Universal (Pos Indonesia)',
          'Titik Layanan Pos Komersial (Terkategorikan)',
          'Area Terlayani Pos',
          'Area Tidak Terlayani Pos',
          'Area Kawasan Pemukiman',
          'Area Bangunan'
        ];
        const colors = [
          '#130292',
          '#E09600',
          '#1916ca',
          '#fd0d0d',
          '#f5009b',
          '#fafafa'
        ];

        // create legend
        const legend = document.getElementById('legend');

        layers.forEach((layer, i) => {
          const color = colors[i];
          const item = document.createElement('div');
          const key = document.createElement('span');
          key.className = 'legend-key';
          key.style.backgroundColor = color;

          const value = document.createElement('span');
          value.innerHTML = `${layer}`;
          item.appendChild(key);
          item.appendChild(value);
          legend.appendChild(item);
        });

        // change info window on hover
        map.on('mousemove', (event) => {
          const states = map.queryRenderedFeatures(event.point, {
            layers: ['statedata']
          });
          document.getElementById('pd').innerHTML = states.length
            ? `<h3>${states[0].properties.name}</h3><p><strong><em>${states[0].properties.density}</strong> people per square mile</em></p>`
            : `<p> Radius 10.000 meter </p>`;
        });
      });
    </script>
  </body>
</html>
