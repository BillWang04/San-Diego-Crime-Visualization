<script>
	import { onMount } from "svelte";
	import "../node_modules/mapbox-gl/dist/mapbox-gl.css";
	import * as d3 from "d3";
  
	let lng, lat, zoom;
	lng = -117.1611;
	lat = 32.7157;
	zoom = 11;
  
	// Test data
	let csvData;
  
	const dataPoints = [
	  { coordinates: [-117.1611, 32.7157], title: "Point 1", type: "Theft", date: "2024-02-17", description: "Details about the crime" },
	  { coordinates: [-117.1701, 32.7105], title: "Point 2", type: "Assault", date: "2024-02-18", description: "Details about the crime" },
	];
  
	// Colors of categories
	const crimeColors = {
	  Theft: 'blue',
	  Assault: 'red',
	};
  
	async function fetchData() {
		try {
			let csv_data = await d3.csv('/data/ARJIS_Public_Crime_Data_w__Day_of_Week_20240215.csv');
			return csv_data
			.filter(row => {
          		return row.geometry && row.geometry.match(/POINT \((-?\d+\.\d+) (-?\d+\.\d+)\)/);
        	})
			.map(row => {
				const matches = row.geometry.match(/POINT \(([-0-9.]+) ([-0-9.]+)\)/);
				if (matches && matches.length === 3) {
					const [_, longitude, latitude] = matches;
					row.coordinates = [parseFloat(longitude), parseFloat(latitude)];
				}
				return row;
			})
			.slice(0,100);
	  } catch (error) {
		console.error('Error loading CSV:', error);
	  }
	}
  
	// Use await within an async context
	onMount(async () => {
	  // Assign the result of fetchData to csvData
	  csvData = await fetchData();
	  console.log(csvData);
  
	  // Continue with the rest of your map initialization and data processing
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
		  color: crimeColors[point.Crime] || 'gray', // Default to gray if no color is defined
		})
		  .setLngLat(point.coordinates)
		  .setPopup(new mapboxgl.Popup().setHTML(`
		  <h3>${point.Agency}</h3>
		  <p><strong>Type:</strong> ${point.Crime_Category}</p>
		  <p><strong>Date:</strong> ${point.Date}</p>
		  <p><strong>Description:</strong> ${point.Charge_Description}</p>
		  `))
		  .addTo(map);
	  });
	});
  </script>
  
  <main>


  </main>
  
  <style>
  </style>
  