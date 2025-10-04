<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SmartPark Live Dashboard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            color: #333;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }
        
        h1 {
            text-align: center;
            color: #1a2a6c;
            margin-bottom: 20px;
            font-size: 2.5rem;
            padding-bottom: 10px;
            border-bottom: 2px solid #fdbb2d;
        }
        
        .zone-section {
            margin-bottom: 30px;
            padding: 15px;
            background: #f9f9f9;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        
        .zone-section h2 {
            color: #1a2a6c;
            margin-bottom: 15px;
            padding-bottom: 5px;
            border-bottom: 1px solid #ddd;
        }
        
        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }
        
        .slot {
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
            transition: all 0.3s ease;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            min-height: 150px;
        }
        
        .slot:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .slot-free {
            background-color: #d4edda;
            border-color: #c3e6cb;
        }
        
        .slot-occupied {
            background-color: #f8d7da;
            border-color: #f5c6cb;
        }
        
        .slot-soon {
            background-color: #fff3cd;
            border-color: #ffeaa7;
        }
        
        .slot-id {
            font-weight: bold;
            font-size: 1.2rem;
            color: #1a2a6c;
        }
        
        .slot-status {
            font-size: 1rem;
            margin: 8px 0;
            font-weight: 600;
        }
        
        .slot-prediction {
            font-size: 0.85rem;
            color: #6c757d;
            margin-top: 5px;
        }
        
        .book-btn {
            background-color: #1a2a6c;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 10px;
            transition: background-color 0.3s;
            font-weight: 600;
        }
        
        .book-btn:hover {
            background-color: #fdbb2d;
            color: #333;
        }
        
        .book-btn:disabled {
            background-color: #6c757d;
            cursor: not-allowed;
        }
        
        .free-btn {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 10px;
            transition: background-color 0.3s;
            font-weight: 600;
        }
        
        .free-btn:hover {
            background-color: #218838;
        }
        
        .last-updated {
            text-align: center;
            margin-top: 20px;
            color: #6c757d;
            font-size: 0.9rem;
        }
        
        .booking-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .modal-content {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .close-btn {
            font-size: 1.5rem;
            cursor: pointer;
            color: #6c757d;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
        }
        
        .form-group input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        .submit-btn {
            background-color: #1a2a6c;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
            font-size: 1rem;
            font-weight: 600;
            margin-top: 10px;
        }
        
        .submit-btn:hover {
            background-color: #fdbb2d;
            color: #333;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 5px;
            color: white;
            font-weight: 600;
            z-index: 1100;
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .notification.success {
            background-color: #28a745;
        }
        
        .notification.error {
            background-color: #dc3545;
        }
        
        .notification.show {
            opacity: 1;
        }
        
        @media (max-width: 768px) {
            .grid-container {
                grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
            }
            
            h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>SmartPark Live Dashboard</h1>
        <div id="parking-data"></div>
        <div class="last-updated" id="last-updated"></div>
    </div>

    <div class="booking-modal" id="booking-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Book Parking Slot</h2>
                <span class="close-btn" id="close-modal">&times;</span>
            </div>
            <form id="booking-form">
                <input type="hidden" id="slot-id">
                <input type="hidden" id="location">
                
                <div class="form-group">
                    <label for="vehicle-number">Vehicle Number</label>
                    <input type="text" id="vehicle-number" required placeholder="Enter your vehicle number">
                </div>
                
                <div class="form-group">
                    <label for="duration">Parking Duration (hours)</label>
                    <input type="number" id="duration" min="1" max="24" value="2" required>
                </div>
                
                <button type="submit" class="submit-btn">Confirm Booking</button>
            </form>
        </div>
    </div>

    <div id="notification" class="notification"></div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            let currentData = {};
            const bookingModal = document.getElementById('booking-modal');
            const closeModal = document.getElementById('close-modal');
            const bookingForm = document.getElementById('booking-form');
            const slotIdInput = document.getElementById('slot-id');
            const locationInput = document.getElementById('location');
            const notification = document.getElementById('notification');
            
            // API base URL
            const API_BASE = 'http://localhost:5000/api';
            
            // Initial load
            fetchData();
            
            // Poll for updates every 5 seconds
            setInterval(fetchData, 5000);
            
            // Close modal when clicking on X
            closeModal.addEventListener('click', function() {
                bookingModal.style.display = 'none';
            });
            
            // Close modal when clicking outside
            window.addEventListener('click', function(event) {
                if (event.target === bookingModal) {
                    bookingModal.style.display = 'none';
                }
            });
            
            // Handle booking form submission
            bookingForm.addEventListener('submit', function(e) {
                e.preventDefault();
                const slotId = slotIdInput.value;
                const location = locationInput.value;
                const vehicleNumber = document.getElementById('vehicle-number').value;
                const duration = document.getElementById('duration').value;
                
                // Send booking request to backend
                bookSlot(slotId, location, vehicleNumber, duration);
            });
            
            function showNotification(message, isSuccess = true) {
                notification.textContent = message;
                notification.className = `notification ${isSuccess ? 'success' : 'error'} show`;
                
                setTimeout(() => {
                    notification.className = 'notification';
                }, 3000);
            }
            
            function fetchData() {
                fetch(`${API_BASE}/parking-data`)
                    .then(response => {
                        if (!response.ok) {
                            throw new Error('Network response was not ok');
                        }
                        return response.json();
                    })
                    .then(data => {
                        // Update only if data has changed
                        if (JSON.stringify(data) !== JSON.stringify(currentData)) {
                            currentData = data;
                            updateAllLocations(data);
                        }
                        
                        // Update last updated time
                        document.getElementById('last-updated').textContent = 
                            `Last updated: ${new Date().toLocaleTimeString()}`;
                    })
                    .catch(error => {
                        console.error('Error fetching parking data:', error);
                        showNotification('Error connecting to server', false);
                    });
            }
            
            function updateAllLocations(data) {
                // Clear old grids
                const container = document.getElementById('parking-data');
                container.innerHTML = '';
                
                // Loop through locations
                for (const [location, slots] of Object.entries(data)) {
                    const section = document.createElement('div');
                    section.className = 'zone-section';
                    
                    const title = document.createElement('h2');
                    title.textContent = location;
                    section.appendChild(title);
                    
                    const grid = document.createElement('div');
                    grid.className = 'grid-container';
                    
                    // Add slots to this grid
                    slots.forEach(slot => {
                        const slotElement = document.createElement('div');
                        slotElement.className = 'slot';
                        
                        // Determine slot class
                        const now = new Date();
                        let slotClass = 'slot-free';
                        
                        if (slot.status === 'occupied') {
                            if (slot.predicted_free_time) {
                                const freeTime = new Date(slot.predicted_free_time);
                                const minutesLeft = (freeTime - now) / (1000 * 60);
                                
                                if (minutesLeft <= 20) {
                                    slotClass = 'slot-soon';
                                } else {
                                    slotClass = 'slot-occupied';
                                }
                            } else {
                                slotClass = 'slot-occupied';
                            }
                        }
                        
                        slotElement.classList.add(slotClass);
                        
                        // Add slot content
                        slotElement.innerHTML = `
                            <div class="slot-id">${slot.slot_id}</div>
                            <div class="slot-status">${slot.status.toUpperCase()}</div>
                            ${slot.predicted_free_time ? 
                                `<div class="slot-prediction">Free at: ${new Date(slot.predicted_free_time).toLocaleTimeString()}</div>` : 
                                ''}
                            ${slot.status === 'free' ? 
                                `<button class="book-btn" data-slot="${slot.slot_id}" data-location="${location}">Book Now</button>` : 
                                `<button class="free-btn" data-slot="${slot.slot_id}">Free Slot</button>`}
                        `;
                        
                        grid.appendChild(slotElement);
                    });
                    
                    section.appendChild(grid);
                    container.appendChild(section);
                }
                
                // Add event listeners to booking buttons
                document.querySelectorAll('.book-btn').forEach(btn => {
                    btn.addEventListener('click', function() {
                        const slotId = this.getAttribute('data-slot');
                        const location = this.getAttribute('data-location');
                        
                        // Set values in the form
                        slotIdInput.value = slotId;
                        locationInput.value = location;
                        
                        // Show modal
                        bookingModal.style.display = 'flex';
                    });
                });
                
                // Add event listeners to free buttons
                document.querySelectorAll('.free-btn').forEach(btn => {
                    btn.addEventListener('click', function() {
                        const slotId = this.getAttribute('data-slot');
                        freeSlot(slotId);
                    });
                });
            }
            
            function bookSlot(slotId, location, vehicleNumber, duration) {
                fetch(`${API_BASE}/book-slot`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        slot_id: slotId,
                        location: location,
                        vehicle_number: vehicleNumber,
                        duration: parseInt(duration)
                    })
                })
                .then(response => {
                    if (!response.ok) {
                        return response.json().then(err => { throw new Error(err.error) });
                    }
                    return response.json();
                })
                .then(data => {
                    showNotification(`Booking confirmed for ${slotId}`);
                    bookingModal.style.display = 'none';
                    bookingForm.reset();
                    
                    // Refresh data to reflect the booking
                    fetchData();
                })
                .catch(error => {
                    showNotification(error.message, false);
                    console.error('Error booking slot:', error);
                });
            }
            
            function freeSlot(slotId) {
                fetch(`${API_BASE}/free-slot`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        slot_id: slotId
                    })
                })
                .then(response => {
                    if (!response.ok) {
                        return response.json().then(err => { throw new Error(err.error) });
                    }
                    return response.json();
                })
                .then(data => {
                    showNotification(`Slot ${slotId} freed successfully`);
                    
                    // Refresh data to reflect the change
                    fetchData();
                })
                .catch(error => {
                    showNotification(error.message, false);
                    console.error('Error freeing slot:', error);
                });
            }
        });
    </script>
</body>
</html>
