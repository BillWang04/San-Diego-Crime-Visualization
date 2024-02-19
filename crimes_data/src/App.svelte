<script>
    mapboxgl.accessToken = "pk.eyJ1IjoiYmx3MDAyIiwiYSI6ImNsc21kam9xcTBtaHQycXBkNGVudTZmNXAifQ.Zxv8Wui2RBVxFDRB0mbvMw";
	const map = new mapboxgl.Map({
		container: "map",
		style: "mapbox://styles/mapbox/dark-v11", 
		center: [-117.1611, 32.7157], 
		zoom: 12, // starting zoom level
		minZoom: 11,
		maxZoom: 20,
	});
	//test data
	const dataPoints = [
      { coordinates: [-117.1611, 32.7157], title: "Point 1", type: "Theft", date: "2024-02-17", description: "Details about the crime" },
      { coordinates: [-117.1701, 32.7105], title: "Point 2", type: "Assault", date: "2024-02-18", description: "Details about the crime" },
    ];


	//colors of categories
	const crimeColors = {
      Theft: 'blue',
      Assault: 'red',
    };

	dataPoints.forEach(point => {
      const marker = new mapboxgl.Marker({
        color: crimeColors[point.type] || 'gray', // Default to gray if no color is defined
      })
        .setLngLat(point.coordinates)
        .setPopup(new mapboxgl.Popup().setHTML(`
          <h3>${point.title}</h3>
          <p><strong>Type:</strong> ${point.type}</p>
          <p><strong>Date:</strong> ${point.date}</p>
          <p><strong>Description:</strong> ${point.description}</p>
        `))
        .addTo(map);
    });



</script>

<main>

</main>

<style>
	#map {
	  height: 400px; /* Set the desired height for your map */
	  width: 100%;
	}
  

  </style>


