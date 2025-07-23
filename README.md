<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Trip Records</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #2E7D32; /* Dark green background */
      color: white; /* White text */
      font-size: 14px;
    }
    
    .header-container {
      background: linear-gradient(to right, #1B5E20, #2E7D32); /* Darker green gradient */
      padding: 10px 0;
      margin-bottom: 15px;
      border-bottom: 2px solid #1B5E20;
    }
    .header {
      text-align: center;
      margin: 0 auto;
      max-width: 800px;
      padding: 0 10px;
    }
    .header h1 {
      margin: 0;
      font-size: clamp(18px, 4vw, 22px);
      font-weight: bold;
      color: white;
      text-shadow: 1px 1px 2px #333;
    }
    .header p {
      margin: 3px 0;
      font-size: clamp(10px, 2.5vw, 12px);
      color: white;
    }
    
    .contact-info {
      font-size: clamp(9px, 2.5vw, 11px);
      text-align: center;
      margin: 5px auto;
      max-width: 800px;
      background: rgba(255, 255, 255, 0.2); /* Semi-transparent white */
      padding: 5px;
      border-radius: 3px;
      color: white;
    }
    
    .form-container {
      background: rgba(255, 255, 255, 0.1); /* Semi-transparent white */
      padding: 15px;
      margin: 15px auto;
      max-width: 800px;
      border: 1px solid rgba(255, 255, 255, 0.3);
      border-radius: 5px;
      color: white;
    }
    
    .table-container {
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      margin: 15px auto;
      max-width: 800px;
      border: 1px solid rgba(255, 255, 255, 0.3);
      background: rgba(255, 255, 255, 0.1);
      color: white;
    }
    
    table {
      width: 100%;
      min-width: 700px;
      border-collapse: collapse;
      font-size: clamp(10px, 2.5vw, 12px);
      color: white;
    }
    th, td {
      border: 1px solid rgba(255, 255, 255, 0.5);
      padding: 6px;
      text-align: left;
      white-space: nowrap;
    }
    th {
      background-color: rgba(0, 0, 0, 0.3);
      font-weight: bold;
      position: sticky;
      top: 0;
    }
    
    input {
      padding: 6px;
      margin: 3px 0;
      font-size: clamp(11px, 3vw, 12px);
      border: 1px solid rgba(255, 255, 255, 0.3);
      border-radius: 3px;
      width: 100%;
      box-sizing: border-box;
      background: rgba(255, 255, 255, 0.2);
      color: white;
    }
    input::placeholder {
      color: rgba(255, 255, 255, 0.7);
    }
    
    button {
      padding: 8px 12px;
      margin: 5px;
      font-size: clamp(11px, 3vw, 12px);
      color: white;
      border: none;
      border-radius: 3px;
      cursor: pointer;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1);
      white-space: nowrap;
      font-weight: bold;
      transition: all 0.2s;
    }
    button:hover {
      transform: translateY(-1px);
      box-shadow: 0 2px 4px rgba(0,0,0,0.2);
    }
    button:active {
      transform: translateY(0);
      box-shadow: 0 1px 2px rgba(0,0,0,0.1);
    }
    
    /* Primary button - Bright blue */
    button.primary {
      background: #2196F3;
    }
    button.primary:hover {
      background: #0b7dda;
    }
    
    /* Danger button - Bright red */
    button.danger {
      background: #F44336;
    }
    button.danger:hover {
      background: #d32f2f;
    }
    
    /* Secondary button - Purple */
    button.secondary {
      background: #9C27B0;
    }
    button.secondary:hover {
      background: #7B1FA2;
    }
    
    /* Success button - Bright green */
    button.success {
      background: #4CAF50;
    }
    button.success:hover {
      background: #388E3C;
    }
    
    /* Warning button - Orange */
    button.warning {
      background: #FF9800;
    }
    button.warning:hover {
      background: #F57C00;
    }
    
    /* File button - Teal */
    button.file-btn {
      background: #009688;
    }
    button.file-btn:hover {
      background: #00796B;
    }
    
    /* Location button - Amber */
    button.location-btn {
      background: #FFC107;
    }
    button.location-btn:hover {
      background: #FFA000;
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
      flex-wrap: nowrap;
      overflow-x: auto;
      margin-bottom: 8px;
      gap: 10px;
      padding-bottom: 15px;
    }
    .form-group {
      flex: 0 0 auto;
      min-width: 120px;
    }
    label {
      font-size: clamp(11px, 3vw, 12px);
      display: block;
      margin-bottom: 3px;
      font-weight: bold;
      color: white;
    }
    #printable-area {
      margin: 0 auto;
      max-width: 800px;
      padding: 0 10px;
      color: white;
    }
    .print-header {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-bottom: 10px;
      text-align: center;
    }
    .print-title {
      font-size: clamp(14px, 3.5vw, 16px);
      margin: 10px 0;
      width: 100%;
      color: white;
    }
    .vehicle-number {
      font-size: clamp(12px, 3vw, 14px);
      font-weight: bold;
      margin-top: 5px;
      color: white;
    }
    
    #map {
      height: 200px;
      margin: 10px 0;
      display: none;
      border-radius: 5px;
      border: 1px solid rgba(255, 255, 255, 0.3);
    }
    
    #tripBody tr {
      cursor: pointer;
    }
    #tripBody tr:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }
    
    /* Signature Modal Styles */
    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.7);
    }
    
    .modal-content {
      background-color: white;
      margin: 5% auto;
      padding: 20px;
      border-radius: 10px;
      width: 90%;
      max-width: 600px;
      color: #333;
    }
    
    .signature-container {
      margin-top: 20px;
      text-align: center;
    }
    
    #signatureCanvas {
      border: 1px solid #ccc;
      background-color: white;
      width: 100%;
      height: 200px;
      margin: 10px 0;
      cursor: crosshair;
    }
    
    .color-options {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin: 10px 0;
    }
    
    .color-option {
      width: 30px;
      height: 30px;
      border-radius: 50%;
      cursor: pointer;
      border: 2px solid transparent;
    }
    
    .color-option.selected {
      border-color: #333;
    }
    
    .signature-btn-group {
      display: flex;
      justify-content: space-between;
      margin-top: 15px;
    }
    
    .signature-preview {
      margin-top: 20px;
      padding: 15px;
      border: 1px dashed #ccc;
      border-radius: 4px;
      min-height: 80px;
      text-align: center;
    }
    
    #signaturePreview {
      max-width: 100%;
      max-height: 100px;
    }
    
    @media (max-width: 768px) {
      .form-group {
        min-width: 100px;
      }
      th, td {
        padding: 4px;
      }
    }
    
    @media (max-width: 480px) {
      .header h1 {
        font-size: 16px;
      }
      .header p {
        font-size: 10px;
      }
      .contact-info {
        font-size: 9px;
        padding: 3px;
      }
      input {
        padding: 5px;
        font-size: 11px;
      }
      button {
        padding: 6px 10px;
        font-size: 11px;
      }
      .table-container {
        padding-bottom: 20px;
      }
      .modal-content {
        margin: 10% auto;
        width: 95%;
      }
    }
    
    @media print {
      body {
        background: white;
        color: black;
      }
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
        color: black;
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
        font-size: 12px;
      }
      table {
        font-size: 10px;
      }
      .table-container {
        overflow: visible;
        background: white;
      }
      th, td {
        border-color: #000;
      }
      th {
        background-color: #f2f2f2;
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
      <h2 class="print-title">TRIP RECORDS - <span id="printedVehicleNumber"></span></h2>
    </div>
    <div class="table-container">
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
            <button type="button" id="detectLocation" class="location-btn warning">Detect Location</button>
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
          <input type="number" id="miles" placeholder="Calculated automatically" readonly>
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
            <input type="number" id="petrol" placeholder="Petrol" value="0" min="0" step="0.1">
            <input type="number" id="diesel" placeholder="Diesel" value="0" min="0" step="0.1">
          </div>
        </div>
        <div class="form-group">
          <label>DRIVER SIGN</label>
          <input type="hidden" id="driverSignature">
          <button type="button" id="captureSignature" class="secondary" style="width: 100%;">Capture Signature</button>
        </div>
        <div class="form-group" style="align-self: flex-end;">
          <button type="submit" id="submitBtn" class="success">Add Trip</button>
          <button type="button" id="cancelEdit" class="danger" style="display: none;">Cancel</button>
        </div>
      </div>
    </form>
  </div>

  <div class="actions no-print">
    <button id="printBtn" class="primary">Print Records</button>
    <button id="saveBtn" class="file-btn">Save to File (DOCX)</button>
    <button id="clearBtn" class="danger">Clear All Trips</button>
    <button id="toggleMap" class="secondary">Show Map</button>
  </div>

  <!-- Signature Capture Modal -->
  <div id="signatureModal" class="modal">
    <div class="modal-content">
      <h2>Driver Signature</h2>
      
      <div class="signature-container">
        <p>Sign in the box below using your mouse or touchscreen</p>
        
        <div class="color-options">
          <div class="color-option selected" style="background-color: black;" data-color="#000000"></div>
          <div class="color-option" style="background-color: blue;" data-color="#0000FF"></div>
          <div class="color-option" style="background-color: red;" data-color="#FF0000"></div>
          <div class="color-option" style="background-color: green;" data-color="#008000"></div>
        </div>
        
        <canvas id="signatureCanvas"></canvas>
        
        <div class="signature-btn-group">
          <button id="clearSignatureBtn" class="danger">Clear</button>
          <button id="saveSignatureBtn" class="success">Save Signature</button>
        </div>
      </div>
      
      <div class="signature-preview">
        <h3>Signature Preview</h3>
        <img id="signaturePreview" style="display: none;">
        <p id="noSignatureText">No signature captured yet</p>
      </div>
    </div>
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
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/docx/7.1.0/docx.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"></script>
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

    // Calculate miles automatically when speedometer values change
    function calculateMiles() {
      const out = parseFloat(document.getElementById('out').value) || 0;
      const inVal = parseFloat(document.getElementById('in').value) || 0;
      
      if (inVal > out) {
        document.getElementById('miles').value = (inVal - out).toFixed(1);
      } else {
        document.getElementById('miles').value = '';
      }
    }

    // Display trips in table
    function displayTrips() {
      const tripBody = document.getElementById('tripBody');
      tripBody.innerHTML = '';
      
      // Update printed vehicle number
      document.getElementById('printedVehicleNumber').textContent = 
        currentVehicleNumber ? currentVehicleNumber : 'No Vehicle Specified';
      
      trips.forEach((trip, index) => {
        const row = document.createElement('tr');
        row.dataset.index = index;
        
        // Check if there's a signature image
        const signatureCell = trip.driverSignature 
          ? `<td><img src="${trip.driverSignature}" style="max-height: 30px;"></td>`
          : `<td></td>`;
        
        row.innerHTML = `
          <td>${trip.date || ''}</td>
          <td>${trip.from || ''}</td>
          <td>${trip.to || ''}</td>
          <td>${trip.out || ''}</td>
          <td>${trip.in || ''}</td>
          <td>${trip.miles || ''}</td>
          <td>${trip.timeOut || ''}</td>
          <td>${trip.timeIn || ''}</td>
          <td>${trip.petrol || '0'}</td>
          <td>${trip.diesel || '0'}</td>
          ${signatureCell}
          <td class="no-print"><button class="edit-btn warning" data-index="${index}">Edit</button></td>
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
      document.getElementById('petrol').value = trip.petrol || '0';
      document.getElementById('diesel').value = trip.diesel || '0';
      document.getElementById('driverSignature').value = trip.driverSignature || '';

      // Update button states
      document.getElementById('submitBtn').textContent = 'Update Trip';
      document.getElementById('cancelEdit').style.display = 'inline-block';
      
      // Scroll to form
      document.querySelector('.form-container').scrollIntoView({ behavior: 'smooth' });
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
      document.getElementById('petrol').value = '0';
      document.getElementById('diesel').value = '0';
      setCurrentTime();
      
      document.getElementById('submitBtn').textContent = 'Add Trip';
      document.getElementById('cancelEdit').style.display = 'none';
      editingIndex = null;
    }

    // Save trip (add or update)
    function saveTrip(e) {
      e.preventDefault();
      
      // Get vehicle number and update if changed
      const newVehicleNumber = document.getElementById('vehicleNumber').value.trim();
      if (newVehicleNumber !== currentVehicleNumber) {
        currentVehicleNumber = newVehicleNumber;
        localStorage.setItem(`vehicleNumber_${deviceId}`, currentVehicleNumber);
      }
      
      const trip = {
        date: document.getElementById('date').value || today,
        from: document.getElementById('from').value || '',
        to: document.getElementById('to').value || '',
        out: document.getElementById('out').value || '',
        in: document.getElementById('in').value || '',
        miles: document.getElementById('miles').value || '',
        timeOut: document.getElementById('timeOut').value || '',
        timeIn: document.getElementById('timeIn').value || '',
        petrol: document.getElementById('petrol').value || '0',
        diesel: document.getElementById('diesel').value || '0',
        driverSignature: document.getElementById('driverSignature').value || ''
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

    // Print function
    async function handlePrint() {
      try {
        // Create a printable element
        const printableElement = document.createElement('div');
        printableElement.innerHTML = `
          <div style="text-align:center;margin-bottom:10px;">
            <h2 style="color:#4CAF50;margin:0;">ProNet North</h2>
            <p style="margin:5px 0;">working in partnership for sustainable development</p>
            <p style="margin:5px 0;font-size:12px;">Address: P.O. Box 360, Wa | Phone: +33 (0392) 29343</p>
            <h3 style="margin:10px 0;">TRIP RECORDS - ${currentVehicleNumber || 'No Vehicle Specified'}</h3>
            <p style="margin:0;font-size:12px;">Generated on: ${new Date().toLocaleDateString()}</p>
          </div>
          ${document.querySelector('table').outerHTML}
        `;
        printableElement.style.padding = '20px';
        
        // Hide all elements except the printable content
        const originalBody = document.body.innerHTML;
        document.body.innerHTML = printableElement.outerHTML;
        
        // Print and then restore original content
        window.print();
        document.body.innerHTML = originalBody;
        
        // Reinitialize event listeners
        initializeEventListeners();
      } catch (error) {
        console.error('Printing error:', error);
        alert('Error generating print: ' + error.message);
      }
    }

    // Save data to Word DOCX file
    async function saveToWord() {
      try {
        const { Document, Paragraph, TextRun, HeadingLevel, Packer } = docx;
        
        // Create document content
        const doc = new Document({
          title: `Trip Records - ${currentVehicleNumber || 'No Vehicle Specified'}`,
          description: "Generated by ProNet North Trip Records System",
          styles: {
            paragraphStyles: [
              {
                id: "normal",
                name: "Normal",
                run: {
                  size: 24, // 12pt
                  font: "Arial"
                },
                paragraph: {
                  spacing: {
                    line: 276, // 1.15 line spacing
                  }
                }
              },
              {
                id: "heading1",
                name: "Heading 1",
                basedOn: "normal",
                next: "normal",
                run: {
                  size: 32, // 16pt
                  bold: true,
                  color: "4CAF50"
                },
                paragraph: {
                  spacing: {
                    before: 240, // 12pt
                    after: 120 // 6pt
                  },
                  alignment: "center"
                }
              },
              {
                id: "heading2",
                name: "Heading 2",
                basedOn: "normal",
                next: "normal",
                run: {
                  size: 28, // 14pt
                  bold: true
                },
                paragraph: {
                  spacing: {
                    before: 200, // 10pt
                    after: 100 // 5pt
                  },
                  alignment: "center"
                }
              },
              {
                id: "tableHeader",
                name: "Table Header",
                basedOn: "normal",
                run: {
                  bold: true,
                  size: 22 // 11pt
                }
              }
            ]
          }
        });

        // Add title and headers
        const title = new Paragraph({
          text: "ProNet North",
          heading: HeadingLevel.HEADING_1,
          spacing: {
            after: 120
          }
        });

        const subtitle = new Paragraph({
          text: "working in partnership for sustainable development",
          heading: HeadingLevel.HEADING_2
        });

        const contactInfo = new Paragraph({
          text: "Address: P.O. Box 360, Wa | Phone: +33 (0392) 29343",
          heading: HeadingLevel.HEADING_2,
          size: 20 // 10pt
        });

        const reportTitle = new Paragraph({
          text: `TRIP RECORDS - ${currentVehicleNumber || 'No Vehicle Specified'}`,
          heading: HeadingLevel.HEADING_1,
          spacing: {
            before: 240
          }
        });

        const generatedDate = new Paragraph({
          text: `Generated on: ${new Date().toLocaleDateString()}`,
          heading: HeadingLevel.HEADING_2
        });

        // Create table
        const tableRows = [];
        
        // Add table headers
        const headerRow = [
          new Paragraph({ text: "DATE", style: "tableHeader" }),
          new Paragraph({ text: "FROM", style: "tableHeader" }),
          new Paragraph({ text: "TO", style: "tableHeader" }),
          new Paragraph({ text: "OUT", style: "tableHeader" }),
          new Paragraph({ text: "IN", style: "tableHeader" }),
          new Paragraph({ text: "MILES", style: "tableHeader" }),
          new Paragraph({ text: "TIME OUT", style: "tableHeader" }),
          new Paragraph({ text: "TIME IN", style: "tableHeader" }),
          new Paragraph({ text: "PETROL", style: "tableHeader" }),
          new Paragraph({ text: "DIESEL", style: "tableHeader" }),
          new Paragraph({ text: "DRIVER SIGN", style: "tableHeader" })
        ];
        tableRows.push(headerRow);

        // Add trip data
        trips.forEach(trip => {
          const tripRow = [
            new Paragraph({ text: trip.date || "" }),
            new Paragraph({ text: trip.from || "" }),
            new Paragraph({ text: trip.to || "" }),
            new Paragraph({ text: trip.out || "" }),
            new Paragraph({ text: trip.in || "" }),
            new Paragraph({ text: trip.miles || "" }),
            new Paragraph({ text: trip.timeOut || "" }),
            new Paragraph({ text: trip.timeIn || "" }),
            new Paragraph({ text: trip.petrol || "0" }),
            new Paragraph({ text: trip.diesel || "0" }),
            new Paragraph({ text: trip.driverSignature ? "[Signature]" : "" })
          ];
          tableRows.push(tripRow);
        });

        // Add all content to document
        doc.addSection({
          properties: {},
          children: [
            title,
            subtitle,
            contactInfo,
            reportTitle,
            generatedDate,
            new Paragraph({
              text: "",
              spacing: {
                before: 240
              }
            }),
            new docx.Table({
              rows: tableRows.map(row => {
                return new docx.TableRow({
                  children: row.map(cell => {
                    return new docx.TableCell({
                      children: [cell],
                      margins: {
                        top: 100,
                        bottom: 100,
                        left: 100,
                        right: 100
                      }
                    });
                  })
                });
              }),
              width: {
                size: 100,
                type: docx.WidthType.PERCENTAGE
              },
              columnWidths: [10, 10, 10, 8, 8, 8, 8, 8, 8, 8, 12].map(w => w * 100)
            })
          ]
        });

        // Generate and download the document
        Packer.toBlob(doc).then(blob => {
          const fileName = `TripRecords_${currentVehicleNumber || 'unknown'}_${new Date().toISOString().split('T')[0]}.docx`;
          saveAs(blob, fileName);
        });
        
      } catch (error) {
        console.error('Error saving Word document:', error);
        alert('Error saving Word document: ' + error.message);
      }
    }

    // Initialize signature capture functionality
    function initSignatureCapture() {
      const modal = document.getElementById('signatureModal');
      const canvas = document.getElementById('signatureCanvas');
      const ctx = canvas.getContext('2d');
      let isDrawing = false;
      let currentColor = '#000000';
      
      // Initialize canvas
      function initCanvas() {
        const ratio = window.devicePixelRatio || 1;
        const width = canvas.offsetWidth;
        const height = canvas.offsetHeight;
        
        canvas.width = width * ratio;
        canvas.height = height * ratio;
        canvas.style.width = width + 'px';
        canvas.style.height = height + 'px';
        
        ctx.scale(ratio, ratio);
        ctx.lineWidth = 2;
        ctx.lineCap = 'round';
        ctx.lineJoin = 'round';
        ctx.strokeStyle = currentColor;
        
        // Fill with white background
        ctx.fillStyle = 'white';
        ctx.fillRect(0, 0, width, height);
      }
      
      // Drawing functions
      function startDrawing(e) {
        isDrawing = true;
        draw(e);
      }
      
      function draw(e) {
        if (!isDrawing) return;
        
        e.preventDefault();
        
        // Get coordinates
        const rect = canvas.getBoundingClientRect();
        let x, y;
        
        if (e.type.includes('touch')) {
          x = e.touches[0].clientX - rect.left;
          y = e.touches[0].clientY - rect.top;
        } else {
          x = e.clientX - rect.left;
          y = e.clientY - rect.top;
        }
        
        // Draw
        ctx.lineTo(x, y);
        ctx.stroke();
        ctx.beginPath();
        ctx.moveTo(x, y);
      }
      
      function stopDrawing() {
        isDrawing = false;
        ctx.beginPath();
      }
      
      // Color selection
      document.querySelectorAll('.color-option').forEach(option => {
        option.addEventListener('click', () => {
          document.querySelector('.color-option.selected').classList.remove('selected');
          option.classList.add('selected');
          currentColor = option.dataset.color;
          ctx.strokeStyle = currentColor;
        });
      });
      
      // Clear button
      document.getElementById('clearSignatureBtn').addEventListener('click', () => {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = 'white';
        ctx.fillRect(0, 0, canvas.width, canvas.height);
      });
      
      // Save button
      document.getElementById('saveSignatureBtn').addEventListener('click', () => {
        const signatureData = canvas.toDataURL('image/png');
        document.getElementById('driverSignature').value = signatureData;
        
        // Update preview
        const preview = document.getElementById('signaturePreview');
        preview.src = signatureData;
        preview.style.display = 'block';
        document.getElementById('noSignatureText').style.display = 'none';
        
        // Close modal
        modal.style.display = 'none';
      });
      
      // Event listeners for drawing
      canvas.addEventListener('mousedown', startDrawing);
      canvas.addEventListener('mousemove', draw);
      canvas.addEventListener('mouseup', stopDrawing);
      canvas.addEventListener('mouseout', stopDrawing);
      
      // Touch support
      canvas.addEventListener('touchstart', (e) => {
        e.preventDefault();
        const touch = e.touches[0];
        const mouseEvent = new MouseEvent('mousedown', {
          clientX: touch.clientX,
          clientY: touch.clientY
        });
        startDrawing(mouseEvent);
      });
      
      canvas.addEventListener('touchmove', (e) => {
        e.preventDefault();
        const touch = e.touches[0];
        const mouseEvent = new MouseEvent('mousemove', {
          clientX: touch.clientX,
          clientY: touch.clientY
        });
        draw(mouseEvent);
      });
      
      canvas.addEventListener('touchend', stopDrawing);
      
      // Initialize canvas
      window.addEventListener('load', initCanvas);
      window.addEventListener('resize', initCanvas);
      
      // Open modal when capture signature button is clicked
      document.getElementById('captureSignature').addEventListener('click', () => {
        // Clear canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = 'white';
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        
        // Reset preview
        document.getElementById('signaturePreview').style.display = 'none';
        document.getElementById('noSignatureText').style.display = 'block';
        
        // Show modal
        modal.style.display = 'block';
      });
      
      // Close modal when clicking outside
      window.addEventListener('click', (e) => {
        if (e.target === modal) {
          modal.style.display = 'none';
        }
      });
    }

    // Initialize event listeners
    function initializeEventListeners() {
      document.getElementById('detectLocation').addEventListener('click', detectLocation);
      document.getElementById('toggleMap').addEventListener('click', () => {
        const mapDiv = document.getElementById('map');
        mapDiv.style.display = mapDiv.style.display === 'none' ? 'block' : 'none';
        if (mapDiv.style.display === 'block') {
          setTimeout(() => {
            map.invalidateSize();
          }, 100);
        }
      });
      
      document.getElementById('tripForm').addEventListener('submit', saveTrip);
      document.getElementById('cancelEdit').addEventListener('click', cancelEdit);
      document.getElementById('printBtn').addEventListener('click', handlePrint);
      document.getElementById('saveBtn').addEventListener('click', saveToWord);

      // Auto-calculate miles when speedometer values change
      document.getElementById('out').addEventListener('change', calculateMiles);
      document.getElementById('in').addEventListener('change', calculateMiles);

      document.getElementById('clearBtn').addEventListener('click', () => {
        if (confirm('Clear ALL trips for this device?')) {
          trips = [];
          currentVehicleNumber = '';
          localStorage.setItem(`trips_${deviceId}`, JSON.stringify(trips));
          localStorage.setItem(`vehicleNumber_${deviceId}`, '');
          document.getElementById('printedVehicleNumber').textContent = 'No Vehicle Specified';
          displayTrips();
          resetForm();
        }
      });
      
      // Handle vehicle number changes
      document.getElementById('vehicleNumber').addEventListener('change', function() {
        currentVehicleNumber = this.value.trim();
        localStorage.setItem(`vehicleNumber_${deviceId}`, currentVehicleNumber);
        document.getElementById('printedVehicleNumber').textContent = 
          currentVehicleNumber || 'No Vehicle Specified';
      });
      
      // Initialize signature capture
      initSignatureCapture();
    }

    // Initialize application
    function initializeApp() {
      document.getElementById('date').value = today;
      document.getElementById('vehicleNumber').value = currentVehicleNumber;
      document.getElementById('printedVehicleNumber').textContent = currentVehicleNumber || 'No Vehicle Specified';
      document.getElementById('petrol').value = '0';
      document.getElementById('diesel').value = '0';
      setCurrentTime();
      displayTrips();
      initMap();
      initializeEventListeners();
    }

    // Start the application when DOM is loaded
    document.addEventListener('DOMContentLoaded', initializeApp);

    // Save data before unloading
    window.addEventListener('beforeunload', () => {
      localStorage.setItem(`trips_${deviceId}`, JSON.stringify(trips));
      localStorage.setItem(`vehicleNumber_${deviceId}`, currentVehicleNumber);
    });
  </script>
</body>
</html>
