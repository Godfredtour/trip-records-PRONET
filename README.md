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
      background: #f5f5f5;
      font-size: 14px;
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
      color: #333;
    }
    
    .contact-info {
      font-size: clamp(9px, 2.5vw, 11px);
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
    
    .table-container {
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      margin: 15px auto;
      max-width: 800px;
      border: 1px solid #ddd;
      background: white;
    }
    
    table {
      width: 100%;
      min-width: 700px;
      border-collapse: collapse;
      font-size: clamp(10px, 2.5vw, 12px);
    }
    th, td {
      border: 1px solid #000;
      padding: 6px;
      text-align: left;
      white-space: nowrap;
    }
    th {
      background-color: #f2f2f2;
      font-weight: bold;
      position: sticky;
      top: 0;
    }
    
    input {
      padding: 6px;
      margin: 3px 0;
      font-size: clamp(11px, 3vw, 12px);
      border: 1px solid #ddd;
      border-radius: 3px;
      width: 100%;
      box-sizing: border-box;
    }
    
    button {
      padding: 8px 12px;
      margin: 5px;
      font-size: clamp(11px, 3vw, 12px);
      background: #4CAF50;
      color: white;
      border: none;
      border-radius: 3px;
      cursor: pointer;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1);
      white-space: nowrap;
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
    button.file-btn {
      background: #9C27B0;
    }
    button.file-btn:hover {
      background: #7B1FA2;
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
    }
    #printable-area {
      margin: 0 auto;
      max-width: 800px;
      padding: 0 10px;
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
    }
    .vehicle-number {
      font-size: clamp(12px, 3vw, 14px);
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
        font-size: 12px;
      }
      table {
        font-size: 10px;
      }
      .table-container {
        overflow: visible;
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
          <input type="number" id="miles" placeholder="Calculated automatically">
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
            <input type="number" id="petrol" placeholder="Petrol" value="0">
            <input type="number" id="diesel" placeholder="Diesel" value="0">
          </div>
        </div>
        <div class="form-group">
          <label>DRIVER SIGN</label>
          <input type="text" id="driver" placeholder="Optional">
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
    <button id="saveBtn" class="file-btn">Save to File</button>
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
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
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
        driver: document.getElementById('driver').value || ''
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

    // Generate HTML for print/save
    function generatePrintableHTML() {
      return `
        <div style="text-align:center;margin-bottom:10px;">
          <h2 style="color:#4CAF50;margin:0;">ProNet North</h2>
          <p style="margin:5px 0;">working in partnership for sustainable development</p>
          <p style="margin:5px 0;font-size:12px;">Address: P.O. Box 360, Wa | Phone: +33 (0392) 29343</p>
          <h3 style="margin:10px 0;">TRIP RECORDS - ${currentVehicleNumber || 'No Vehicle Specified'}</h3>
        </div>
        ${document.querySelector('table').outerHTML}
      `;
    }

    // Android-friendly print function
    async function handlePrint() {
      try {
        // Try regular print first
        if (window.print && !/Android/i.test(navigator.userAgent)) {
          window.print();
        } else {
          // Fallback for Android - generate PDF
          const { jsPDF } = window.jspdf;
          const doc = new jsPDF({
            orientation: 'landscape'
          });
          
          // Get printable content
          const printableElement = document.createElement('div');
          printableElement.innerHTML = generatePrintableHTML();
          
          // Create PDF
          await doc.html(printableElement, {
            callback: function(doc) {
              // For Android: Save as PDF
              if (/Android/i.test(navigator.userAgent)) {
                doc.save('TripRecords.pdf');
              } else {
                // For other devices - try printing PDF
                const pdfBlob = doc.output('blob');
                const pdfUrl = URL.createObjectURL(pdfBlob);
                window.open(pdfUrl, '_blank');
              }
            },
            x: 10,
            y: 10,
            width: 280, // Landscape width in mm
            windowWidth: 800
          });
        }
      } catch (error) {
        console.error('Printing error:', error);
        alert('Error generating print: ' + error.message);
      }
    }

    // Save data to PDF file (matching print layout)
    async function saveToFile() {
      try {
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF({
          orientation: 'landscape'
        });
        
        // Get printable content
        const printableElement = document.createElement('div');
        printableElement.innerHTML = generatePrintableHTML();
        
        // Create PDF
        await doc.html(printableElement, {
          callback: function(doc) {
            const fileName = `TripRecords_${currentVehicleNumber || 'unknown'}_${new Date().toISOString().split('T')[0]}.pdf`;
            doc.save(fileName);
          },
          x: 10,
          y: 10,
          width: 280, // Landscape width in mm
          windowWidth: 800
        });
        
      } catch (error) {
        console.error('Error saving file:', error);
        alert('Error saving file: ' + error.message);
      }
    }

    // Initialize
    document.addEventListener('DOMContentLoaded', () => {
      document.getElementById('date').value = today;
      document.getElementById('vehicleNumber').value = currentVehicleNumber;
      document.getElementById('printedVehicleNumber').textContent = currentVehicleNumber || 'No Vehicle Specified';
      document.getElementById('petrol').value = '0';
      document.getElementById('diesel').value = '0';
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
      document.getElementById('printBtn').addEventListener('click', handlePrint);
      document.getElementById('saveBtn').addEventListener('click', saveToFile);

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
    });

    window.addEventListener('beforeunload', () => {
      localStorage.setItem(`trips_${deviceId}`, JSON.stringify(trips));
      localStorage.setItem(`vehicleNumber_${deviceId}`, currentVehicleNumber);
    });
  </script>
</body>
</html>
