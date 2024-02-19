<script>
    import { onMount } from "svelte";
	import "../node_modules/mapbox-gl/dist/mapbox-gl.css"

	let mapContainer;
	let lng, lat, zoom;

	lng = -117.1611;
	lat = 32.7157;
	zoom = 11;
	//test data
	let csvData
	d3.csv('/data/ARJIS_Public_Crime_Data_w__Day_of_Week_20240215.csv').then(data => {
		csvData = data
	});
	console.log(csvData)
	
	const dataPoints = [
      { coordinates: [-117.1611, 32.7157], title: "Point 1", type: "Theft", date: "2024-02-17", description: "Details about the crime" },
      { coordinates: [-117.1701, 32.7105], title: "Point 2", type: "Assault", date: "2024-02-18", description: "Details about the crime" },
    ];

	let crimeFilter = {};

	//colors of categories
	const crimeColors = {
      	Theft: 'blue',
     	Assault: 'red',
    };

	let markers = [];
	onMount(() =>{
		mapboxgl.accessToken = "pk.eyJ1IjoiYmx3MDAyIiwiYSI6ImNsc21kam9xcTBtaHQycXBkNGVudTZmNXAifQ.Zxv8Wui2RBVxFDRB0mbvMw";

		const map = new mapboxgl.Map({
			container: "map",
			style: "mapbox://styles/mapbox/dark-v11", 
			center: [lng, lat], 
			zoom: zoom, // starting zoom level
			minZoom: 11,
			maxZoom: 18,
		});

		csvData.forEach(point => {
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
	});




</script>

<main>
	  

</main>

<style>



</style>


