<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Trip Records</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      background: #f5f5f5;
    }
    .header-container {
      background: linear-gradient(to right, #4CAF50, white);
      padding: 10px 0;
      margin-bottom: 15px;
      border-bottom: 2px solid #4CAF50;
    }
    .header {
      text-align: center;
      margin: 0 auto;
      max-width: 800px;
    }
    .header h1 {
      margin: 0;
      font-size: 22px;
      font-weight: bold;
      color: white;
      text-shadow: 1px 1px 2px #333;
    }
    .header p {
      margin: 3px 0;
      font-size: 12px;
      color: #333;
    }
    .contact-info {
      font-size: 11px;
      text-align: center;
      margin: 5px auto;
      max-width: 800px;
      background: white;
      padding: 5px;
      border-radius: 3px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }
    .form-container {
      background: white;
      padding: 15px;
      margin: 15px auto;
      max-width: 800px;
      border: 1px solid #ddd;
      border-radius: 5px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }
    table {
      width: 100%;
      max-width: 800px;
      margin: 15px auto;
      border-collapse: collapse;
      font-size: 12px;
    }
    th, td {
      border: 1px solid #000;
      padding: 5px;
      text-align: left;
    }
    th {
      background-color: #f2f2f2;
      font-weight: bold;
    }
    input {
      padding: 5px;
      margin: 3px 0;
      font-size: 12px;
      border: 1px solid #ddd;
      border-radius: 3px;
    }
    button {
      padding: 7px 12px;
      margin: 5px;
      font-size: 12px;
      background: #4CAF50;
      color: white;
      border: none;
      border-radius: 3px;
      cursor: pointer;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1);
    }
    button:hover {
      background: #45a049;
    }
    button.danger {
      background: #f44336;
    }
    button.danger:hover {
      background: #d32f2f;
    }
    button.secondary {
      background: #2196F3;
    }
    button.secondary:hover {
      background: #0b7dda;
    }
    .actions {
      margin: 15px auto;
      max-width: 800px;
      text-align: center;
    }
    .no-print {
      display: block;
    }
    .form-row {
      display: flex;
      flex-wrap: wrap;
      margin-bottom: 8px;
      gap: 10px;
    }
    .form-group {
      flex: 1;
      min-width: 120px;
    }
    label {
      font-size: 12px;
      display: block;
      margin-bottom: 3px;
      font-weight: bold;
    }
    #printable-area {
      margin: 0 auto;
      max-width: 800px;
    }
    .print-header {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 10px;
      text-align: center;
    }
    .print-title {
      font-size: 16px;
      margin: 10px 0;
      width: 100%;
    }
    .vehicle-number {
      font-size: 14px;
      font-weight: bold;
      margin-top: 5px;
    }
    #map {
      height: 200px;
      margin: 10px 0;
      display: none;
    }
    .location-btn {
      background: #FF9800;
    }
    .location-btn:hover {
      background: #F57C00;
    }
    #tripBody tr {
      cursor: pointer;
    }
    #tripBody tr:hover {
      background-color: #f0f0f0;
    }
    .edit-btn {
      background: #FF9800;
      margin-left: 5px;
    }
    .edit-btn:hover {
      background: #F57C00;
    }
    @media print {
      body * {
        visibility: hidden;
      }
      .header-container,
      .header-container *,
      .contact-info,
      .contact-info *,
      #printable-area, 
      #printable-area * {
        visibility: visible;
      }
      #printable-area {
        position: relative;
        left: auto;
        top: auto;
        width: 100%;
      }
      .no-print {
        display: none !important;
      }
      .edit-btn {
        display: none !important;
      }
      body {
        margin: 0;
        padding: 10px;
      }
    }
  </style>
</head>
<body>
  <div class="header-container">
    <div class="header">
      <h1>ProNet North</h1>
      <p>working in partnership for sustainable development</p>
    </div>
  </div>
  <div class="contact-info">
    <p>Address: P.O. Box 360, Wa | Phone: +33 (0392) 29343 | Fax: +33 (0392) 29348</p>
    <p>e-mail: francova@gmail.com | website: pronetwa@afficaonline.com.gh | http://www.pronet-ghana.org</p>
  </div>

  <div id="printable-area">
    <div class="print-header">
      <h2 class="print-title">TRIP RECORDS</h2>
      <div class="vehicle-number" id="printedVehicleNumber"></div>
    </div>
    <table>
      <thead>
        <tr>
          <th>DATE</th>
          <th colspan="2">JOURNEY</th>
          <th colspan="2">SPEEDOMETER</th>
          <th>MILES</th>
          <th colspan="2">TIME</th>
          <th colspan="2">FUEL BOUGHT</th>
          <th>DRIVER SIGN</th>
          <th class="no-print">ACTION</th>
        </tr>
        <tr>
          <th></th>
          <th>from</th>
          <th>to</th>
          <th>out</th>
          <th>in</th>
          <th></th>
          <th>out</th>
          <th>in</th>
          <th>Petrol</th>
          <th>Diesel</th>
          <th></th>
          <th class="no-print"></th>
        </tr>
      </thead>
      <tbody id="tripBody"></tbody>
    </table>
  </div>

  <div class="form-container no-print">
    <h3>Add/Edit Trip</h3>
    <div id="map"></div>
    <form id="tripForm">
      <div class="form-row">
        <div class="form-group">
          <label>VEHICLE NUMBER</label>
          <input type="text" id="vehicleNumber">
        </div>
        <div class="form-group">
          <label>DATE</label>
          <input type="date" id="date">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>JOURNEY</label>
          <div style="display: flex; align-items: center; gap: 5px;">
            <input type="text" id="from" placeholder="From" list="communities">
            <button type="button" id="detectLocation" class="location-btn">Detect Location</button>
            <span>to</span>
            <input type="text" id="to" placeholder="To" list="communities">
          </div>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>SPEEDOMETER</label>
          <div style="display: flex; align-items: center; gap: 5px;">
            <input type="number" id="out" placeholder="Out">
            <span>/</span>
            <input type="number" id="in" placeholder="In">
          </div>
        </div>
        <div class="form-group">
          <label>MILES</label>
          <input type="number" id="miles">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>TIME</label>
          <div style="display: flex; align-items: center; gap: 5px;">
            <input type="time" id="timeOut" placeholder="Out">
            <span>/</span>
            <input type="time" id="timeIn" placeholder="In">
          </div>
        </div>
        <div class="form-group">
          <label>FUEL BOUGHT</label>
          <div style="display: flex; gap: 5px;">
            <input type="number" id="petrol" placeholder="Petrol">
            <input type="number" id="diesel" placeholder="Diesel">
          </div>
        </div>
        <div class="form-group">
          <label>DRIVER SIGN</label>
          <input type="text" id="driver">
        </div>
        <div class="form-group" style="align-self: flex-end;">
          <button type="submit" id="submitBtn">Add Trip</button>
          <button type="button" id="cancelEdit" class="danger" style="display: none;">Cancel</button>
        </div>
      </div>
    </form>
  </div>

  <div class="actions no-print">
    <button id="printBtn">Print Records</button>
    <button id="clearBtn" class="danger">Clear All Trips</button>
    <button id="toggleMap" class="secondary">Show Map</button>
  </div>

  <datalist id="communities">
    <option value="Wa"></option>
    <option value="Bamahu"></option>
    <option value="Jirapa"></option>
    <option value="Lawra"></option>
    <option value="Nandom"></option>
    <option value="Tumu"></option>
    <option value="Gwollu"></option>
    <option value="Lambussie"></option>
    <option value="Hamile"></option>
    <option value="Funsi"></option>
    <option value="Wechiau"></option>
  </datalist>

  <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
  <script>
    // Simple device identification
    function getDeviceId() {
      let deviceId = localStorage.getItem('deviceId');
      if (!deviceId) {
        deviceId = 'dev-' + Math.random().toString(36).substr(2, 9);
        localStorage.setItem('deviceId', deviceId);
      }
      return deviceId;
    }

    const deviceId = getDeviceId();
    let trips = JSON.parse(localStorage.getItem(`trips_${deviceId}`)) || [];
    let currentVehicleNumber = localStorage.getItem(`vehicleNumber_${deviceId}`) || '';
    const today = new Date().toISOString().split('T')[0];
    let map;
    let currentMarker;
    let editingIndex = null;

    // Upper West Region coordinates (centered on Wa)
    const upperWestBounds = [
      [9.5, -3.0], // Southwest
      [11.0, -1.5]  // Northeast
    ];

    // Major towns in Upper West with coordinates
    const upperWestTowns = {
      "Wa": [10.0606, -2.5099],
      "Jirapa": [10.5500, -2.7333],
      "Lawra": [10.6833, -2.9000],
      "Nandom": [10.8000, -2.7667],
      "Tumu": [10.8833, -1.9667],
      "Gwollu": [10.2667, -2.5000],
      "Lambussie": [10.9333, -2.7500],
      "Hamile": [10.0500, -2.7833],
      "Funsi": [10.3667, -2.3667],
      "Wechiau": [10.0833, -2.5000]
    };

    // Initialize map
    function initMap() {
      map = L.map('map', {
        maxBounds: upperWestBounds,
        maxBoundsViscosity: 1.0
      }).setView([10.0606, -2.5099], 9);

      // Add offline tile layer (using OSM as base)
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(map);

      // Add markers for major towns
      Object.entries(upperWestTowns).forEach(([town, coords]) => {
        L.marker(coords).addTo(map)
          .bindPopup(town)
          .on('click', () => {
            document.getElementById('from').value = town;
          });
      });
    }

    // Detect current location
    function detectLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;
            
            // Check if within Upper West bounds
            if (lat >= 9.5 && lat <= 11.0 && lng >= -3.0 && lng <= -1.5) {
              // Find nearest town
              let nearestTown = "";
              let minDistance = Infinity;
              
              Object.entries(upperWestTowns).forEach(([town, coords]) => {
                const distance = Math.sqrt(
                  Math.pow(coords[0] - lat, 2) + Math.pow(coords[1] - lng, 2)
                );
                if (distance < minDistance) {
                  minDistance = distance;
                  nearestTown = town;
                }
              });
              
              if (nearestTown) {
                document.getElementById('from').value = nearestTown;
                alert(`Detected location: ${nearestTown}`);
                
                // Add marker to map
                if (currentMarker) map.removeLayer(currentMarker);
                currentMarker = L.marker([lat, lng])
                  .addTo(map)
                  .bindPopup("Your Location")
                  .openPopup();
                map.setView([lat, lng], 13);
              }
            } else {
              alert("You appear to be outside Upper West Region");
            }
          },
          (error) => {
            alert("Error getting location: " + error.message);
          }
        );
      } else {
        alert("Geolocation is not supported by this browser.");
      }
    }

    // Set current time
    function setCurrentTime() {
      const now = new Date();
      const hours = String(now.getHours()).padStart(2, '0');
      const minutes = String(now.getMinutes()).padStart(2, '0');
      document.getElementById('timeOut').value = `${hours}:${minutes}`;
    }

    // Display trips in table
    function displayTrips() {
      const tripBody = document.getElementById('tripBody');
      tripBody.innerHTML = '';
      trips.forEach((trip, index) => {
        const row = document.createElement('tr');
        row.dataset.index = index;
        row.innerHTML = `
          <td>${trip.date || ''}</td>
          <td>${trip.from || ''}</td>
          <td>${trip.to || ''}</td>
          <td>${trip.out || ''}</td>
          <td>${trip.in || ''}</td>
          <td>${trip.miles || ''}</td>
          <td>${trip.timeOut || ''}</td>
          <td>${trip.timeIn || ''}</td>
          <td>${trip.petrol || ''}</td>
          <td>${trip.diesel || ''}</td>
          <td>${trip.driver || ''}</td>
          <td class="no-print"><button class="edit-btn" data-index="${index}">Edit</button></td>
        `;
        tripBody.appendChild(row);
      });

      // Add event listeners to edit buttons
      document.querySelectorAll('.edit-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          e.stopPropagation();
          editTrip(parseInt(btn.dataset.index));
        });
      });

      // Add click event to rows for editing
      document.querySelectorAll('#tripBody tr').forEach(row => {
        row.addEventListener('click', (e) => {
          if (!e.target.classList.contains('edit-btn')) {
            editTrip(parseInt(row.dataset.index));
          }
        });
      });
    }

    // Edit a trip
    function editTrip(index) {
      const trip = trips[index];
      if (!trip) return;

      editingIndex = index;
      
      // Fill form with trip data
      document.getElementById('date').value = trip.date || today;
      document.getElementById('vehicleNumber').value = currentVehicleNumber;
      document.getElementById('from').value = trip.from || '';
      document.getElementById('to').value = trip.to || '';
      document.getElementById('out').value = trip.out || '';
      document.getElementById('in').value = trip.in || '';
      document.getElementById('miles').value = trip.miles || '';
      document.getElementById('timeOut').value = trip.timeOut || '';
      document.getElementById('timeIn').value = trip.timeIn || '';
      document.getElementById('petrol').value = trip.petrol || '';
      document.getElementById('diesel').value = trip.diesel || '';
      document.getElementById('driver').value = trip.driver || '';

      // Update button states
      document.getElementById('submitBtn').textContent = 'Update Trip';
      document.getElementById('cancelEdit').style.display = 'inline-block';
    }

    // Cancel editing
    function cancelEdit() {
      editingIndex = null;
      resetForm();
    }

    // Reset form to default state
    function resetForm() {
      document.getElementById('tripForm').reset();
      document.getElementById('date').value = today;
      document.getElementById('vehicleNumber').value = currentVehicleNumber;
      setCurrentTime();
      
      document.getElementById('submitBtn').textContent = 'Add Trip';
      document.getElementById('cancelEdit').style.display = 'none';
      editingIndex = null;
    }

    // Save trip (add or update)
    function saveTrip(e) {
      e.preventDefault();
      
      const trip = {
        date: document.getElementById('date').value,
        from: document.getElementById('from').value,
        to: document.getElementById('to').value,
        out: document.getElementById('out').value,
        in: document.getElementById('in').value,
        miles: document.getElementById('miles').value,
        timeOut: document.getElementById('timeOut').value,
        timeIn: document.getElementById('timeIn').value,
        petrol: document.getElementById('petrol').value,
        diesel: document.getElementById('diesel').value,
        driver: document.getElementById('driver').value
      };

      if (editingIndex !== null) {
        // Update existing trip
        trips[editingIndex] = trip;
      } else {
        // Add new trip
        trips.push(trip);
      }

      // Save to localStorage and refresh display
      localStorage.setItem(`trips_${deviceId}`, JSON.stringify(trips));
      displayTrips();
      resetForm();
    }

    // Initialize
    document.addEventListener('DOMContentLoaded', () => {
      document.getElementById('date').value = today;
      document.getElementById('vehicleNumber').value = currentVehicleNumber;
      document.getElementById('printedVehicleNumber').textContent = currentVehicleNumber ? `Vehicle: ${currentVehicleNumber}` : '';
      setCurrentTime();
      displayTrips();
      initMap();
      
      // Set up event listeners
      document.getElementById('detectLocation').addEventListener('click', detectLocation);
      document.getElementById('toggleMap').addEventListener('click', () => {
        const mapDiv = document.getElementById('map');
        mapDiv.style.display = mapDiv.style.display === 'none' ? 'block' : 'none';
      });
      
      document.getElementById('tripForm').addEventListener('submit', saveTrip);
      document.getElementById('cancelEdit').addEventListener('click', cancelEdit);
      document.getElementById('printBtn').addEventListener('click', () => {
        window.print();
      });

      document.getElementById('clearBtn').addEventListener('click', () => {
        if (confirm('Clear ALL trips for this device?')) {
          trips = [];
          currentVehicleNumber = '';
          localStorage.setItem(`trips_${deviceId}`, JSON.stringify(trips));
          localStorage.setItem(`vehicleNumber_${deviceId}`, '');
          document.getElementById('printedVehicleNumber').textContent = '';
          displayTrips();
        }
      });
    });

    window.addEventListener('beforeunload', () => {
      localStorage.setItem(`trips_${deviceId}`, JSON.stringify(trips));
      localStorage.setItem(`vehicleNumber_${deviceId}`, currentVehicleNumber);
    });
  </script>
</body>
</html>
