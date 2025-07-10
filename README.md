<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Auto Trip Records</title>
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
    .actions {
      margin: 15px auto;
      max-width: 800px;
      text-align: center;
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
    @media print {
      .no-print, .form-container, .actions {
        display: none !important;
      }
      table {
        page-break-inside: auto;
      }
      tr {
        page-break-inside: avoid;
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
    <p>e-mail: francova@gmail.com | website: pronetwa@afficaonline.com.gh</p>
  </div>

  <div class="form-container no-print">
    <h3>Add New Trip</h3>
    <form id="tripForm">
      <div class="form-row">
        <div class="form-group">
          <label>DATE</label>
          <input type="date" id="date">
        </div>
        <div class="form-group">
          <label>FROM</label>
          <input type="text" id="from" placeholder="Starting location" list="communities">
        </div>
        <div class="form-group">
          <label>TO</label>
          <input type="text" id="to" placeholder="Destination" list="communities" disabled>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>START TIME</label>
          <input type="time" id="startTime">
        </div>
        <div class="form-group">
          <label>END TIME</label>
          <input type="time" id="endTime" disabled>
        </div>
        <div class="form-group">
          <label>SPEEDOMETER START</label>
          <input type="number" id="odoStart">
        </div>
        <div class="form-group">
          <label>SPEEDOMETER END</label>
          <input type="number" id="odoEnd" disabled>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>FUEL (Diesel)</label>
          <input type="number" id="diesel">
        </div>
        <div class="form-group">
          <label>FUEL (Oil)</label>
          <input type="number" id="oil">
        </div>
        <div class="form-group">
          <label>DRIVER SIGN</label>
          <input type="text" id="driverSign" placeholder="Driver's signature">
        </div>
      </div>
      <div class="form-row">
        <button type="submit">Save Trip</button>
        <button type="button" id="completeDayBtn">Complete Day's Trips</button>
      </div>
    </form>
  </div>

  <div class="actions no-print">
    <button id="printBtn">Print Day's Records</button>
    <button id="clearBtn" class="danger">Clear All Data</button>
  </div>

  <table id="tripTable">
    <thead>
      <tr>
        <th>DATE</th>
        <th>FROM</th>
        <th>TO</th>
        <th>START TIME</th>
        <th>END TIME</th>
        <th>ODOMETER START</th>
        <th>ODOMETER END</th>
        <th>DIESEL</th>
        <th>OIL</th>
        <th>DRIVER SIGN</th>
      </tr>
    </thead>
    <tbody id="tripBody"></tbody>
  </table>

  <datalist id="communities">
    <!-- Comprehensive list of Upper West locations -->
    <option value="Wa (Municipal Capital)"></option>
    <option value="Jirapa"></option>
    <option value="Lawra"></option>
    <option value="Nadowli"></option>
    <option value="Daffiama"></option>
    <option value="Nandom"></option>
    <option value="Lambussie"></option>
    <option value="Tumu (Sissala East)"></option>
    <option value="Gwollu (Sissala West)"></option>
    <option value="Hamile"></option>
    <option value="Wechiau"></option>
    <option value="Babile"></option>
    <option value="Busa"></option>
    <option value="Charikpong"></option>
    <option value="Daboya"></option>
    <option value="Eremon"></option>
    <option value="Fian"></option>
    <option value="Gbelinkaa"></option>
    <option value="Goli"></option>
    <option value="Kpaglagi"></option>
    <option value="Kperisi"></option>
    <option value="Kunyukuo"></option>
    <option value="Loho"></option>
    <option value="Mangu"></option>
    <option value="Nakore"></option>
    <option value="Nyoli"></option>
    <option value="Sabuli"></option>
    <option value="Takpo"></option>
    <option value="Tizza"></option>
    <option value="Ullo"></option>
    <option value="Zukpiri"></option>
  </datalist>

  <script>
    // Comprehensive list of Upper West locations with coordinates
    const upperWestLocations = {
      "Wa (Municipal Capital)": { lat: 10.060, lng: -2.500 },
      "Jirapa": { lat: 10.533, lng: -2.733 },
      "Lawra": { lat: 10.833, lng: -2.900 },
      // Add all other locations with coordinates
    };

    // Initialize application
    document.addEventListener('DOMContentLoaded', function() {
      const today = new Date();
      const dateStr = today.toISOString().split('T')[0];
      document.getElementById('date').value = dateStr;
      
      // Set current time as start time
      const now = new Date();
      const timeStr = now.toTimeString().substr(0, 5);
      document.getElementById('startTime').value = timeStr;
      
      // Load any saved trips
      loadTrips();
      
      // Enable location auto-fill if available
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(fillCurrentLocation, showError);
      }
      
      // Setup event listeners
      setupEventHandlers();
    });

    // Fill current location if available
    function fillCurrentLocation(position) {
      const lat = position.coords.latitude;
      const lng = position.coords.longitude;
      
      // Find nearest location from our list
      let nearestLocation = "";
      let minDistance = Infinity;
      
      for (const [location, coords] of Object.entries(upperWestLocations)) {
        const distance = Math.sqrt(
          Math.pow(lat - coords.lat, 2) + 
          Math.pow(lng - coords.lng, 2)
        );
        
        if (distance < minDistance) {
          minDistance = distance;
          nearestLocation = location;
        }
      }
      
      if (nearestLocation) {
        document.getElementById('from').value = nearestLocation;
        document.getElementById('to').disabled = false;
      }
    }

    function showError(error) {
      console.log("Geolocation error:", error);
      // Proceed without auto-fill if location not available
      document.getElementById('to').disabled = false;
    }

    function setupEventHandlers() {
      // Auto-fill end time when start time changes
      document.getElementById('startTime').addEventListener('change', function() {
        const startTime = this.value;
        if (startTime) {
          const [hours, minutes] = startTime.split(':');
          let endHours = parseInt(hours) + 1; // Default 1 hour trip
          if (endHours >= 20) endHours = 20; // Cap at 8pm
          document.getElementById('endTime').value = `${endHours.toString().padStart(2, '0')}:${minutes}`;
          document.getElementById('endTime').disabled = false;
        }
      });
      
      // Auto-fill odometer end when start is entered
      document.getElementById('odoStart').addEventListener('change', function() {
        if (this.value) {
          document.getElementById('odoEnd').disabled = false;
        }
      });
      
      // Form submission
      document.getElementById('tripForm').addEventListener('submit', function(e) {
        e.preventDefault();
        saveTrip();
      });
      
      // Complete day's trips
      document.getElementById('completeDayBtn').addEventListener('click', function() {
        if (confirm("Complete all trips for today and prepare for printing?")) {
          completeDaysTrips();
        }
      });
      
      // Print button
      document.getElementById('printBtn').addEventListener('click', function() {
        window.print();
      });
      
      // Clear button
      document.getElementById('clearBtn').addEventListener('click', function() {
        if (confirm("Clear ALL trip data?")) {
          localStorage.clear();
          document.getElementById('tripBody').innerHTML = '';
        }
      });
    }

    function saveTrip() {
      const trip = {
        date: document.getElementById('date').value,
        from: document.getElementById('from').value,
        to: document.getElementById('to').value,
        startTime: document.getElementById('startTime').value,
        endTime: document.getElementById('endTime').value,
        odoStart: document.getElementById('odoStart').value,
        odoEnd: document.getElementById('odoEnd').value,
        diesel: document.getElementById('diesel').value,
        oil: document.getElementById('oil').value,
        driverSign: document.getElementById('driverSign').value
      };
      
      // Save to localStorage
      let trips = JSON.parse(localStorage.getItem('trips')) || [];
      trips.push(trip);
      localStorage.setItem('trips', JSON.stringify(trips));
      
      // Add to table
      addTripToTable(trip);
      
      // Reset form for next trip
      resetFormForNextTrip();
    }

    function addTripToTable(trip) {
      const row = document.createElement('tr');
      row.innerHTML = `
        <td>${trip.date || ''}</td>
        <td>${trip.from || ''}</td>
        <td>${trip.to || ''}</td>
        <td>${trip.startTime || ''}</td>
        <td>${trip.endTime || ''}</td>
        <td>${trip.odoStart || ''}</td>
        <td>${trip.odoEnd || ''}</td>
        <td>${trip.diesel || ''}</td>
        <td>${trip.oil || ''}</td>
        <td>${trip.driverSign || ''}</td>
      `;
      document.getElementById('tripBody').appendChild(row);
    }

    function resetFormForNextTrip() {
      // Set "from" as previous "to"
      const toValue = document.getElementById('to').value;
      document.getElementById('from').value = toValue;
      
      // Clear other fields
      document.getElementById('to').value = '';
      document.getElementById('startTime').value = '';
      document.getElementById('endTime').value = '';
      document.getElementById('endTime').disabled = true;
      document.getElementById('odoStart').value = document.getElementById('odoEnd').value;
      document.getElementById('odoEnd').value = '';
      document.getElementById('odoEnd').disabled = true;
      document.getElementById('diesel').value = '';
      document.getElementById('oil').value = '';
      document.getElementById('driverSign').value = '';
      
      // Focus on next field
      document.getElementById('to').focus();
    }

    function loadTrips() {
      const trips = JSON.parse(localStorage.getItem('trips')) || [];
      trips.forEach(trip => addTripToTable(trip));
    }

    function completeDaysTrips() {
      // Add any final trip
      if (document.getElementById('from').value && document.getElementById('to').value) {
        saveTrip();
      }
      
      // Prepare for printing
      alert("Day's trips completed. Ready for printing.");
    }
  </script>
</body>
</html>
