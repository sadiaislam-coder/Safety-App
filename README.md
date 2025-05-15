<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SafeHer - Women Safety App</title>
    <style>
        :root {
            --primary: #6C63FF;  
            --secondary: #2D3250;  
            --accent: #FF6584;  
            --background: #F5F5F5;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }

        body {
            background: var(--background);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 500px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 25px;
            text-align: center;
        }

        .emergency-section {
            position: relative;
            padding: 30px;
        }

        .sos-btn {
            width: 180px;
            height: 180px;
            background: var(--accent);
            border-radius: 50%;
            margin: 20px auto;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            box-shadow: 0 0 30px rgba(255,101,132,0.3);
            transition: transform 0.3s;
            border: none;
            color: white;
            font-size: 1.4rem;
            font-weight: 600;
            position: relative;
            animation: pulse 2s infinite;
        }

        .quick-actions {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            padding: 20px;
        }

        .action-card {
            background: white;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid var(--primary);
            position: relative;
            overflow: hidden;
        }

        .action-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(108,99,255,0.2);
        }

        .action-icon {
            width: 50px;
            height: 50px;
            margin-bottom: 10px;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.1));
        }

        .location-info {
            background: var(--background);
            padding: 15px;
            border-radius: 15px;
            margin: 20px;
            font-size: 0.9rem;
        }

        @keyframes pulse {
            0% { transform: scale(0.95); }
            50% { transform: scale(1.05); }
            100% { transform: scale(0.95); }
        }

        
        .dark-mode-toggle {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            cursor: pointer;
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>SafeHer</h1>
            <p>Your Personal Safety Companion</p>
        </div>

        <div class="emergency-section">
            <button class="sos-btn" id="sosBtn">
                🆘 EMERGENCY
            </button>
            
            <div class="quick-actions">
                <div class="action-card" id="shareLocation">
                    <img src="https://img.icons8.com/fluency/48/map-pin.png" class="action-icon">
                    Share Live Location
                </div>
                <div class="action-card" id="fakeCall">
                    <img src="https://img.icons8.com/color/48/phone.png" class="action-icon">
                    Fake Call
                </div>
                <div class="action-card" id="safetyNetwork">
                    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSA0wTYf7yD1-70lPKiDa1DItZUYHxUz4cdR3BU-JjJ_9FAIivBgRra0Zk&s=10" class="action-icon">
                    Safety Network
                </div>
                <div class="action-card" id="safetyTips">
                    <img src="https://img.icons8.com/color/48/safety-hat.png" class="action-icon">
                    Safety Guide
                </div>
            </div>

            <div class="location-info" id="locationInfo">
                <p>📍 Current Location: Loading...</p>
                <div class="map-preview" id="map"></div>
            </div>
        </div>
    </div>

    <button class="dark-mode-toggle" id="darkModeToggle">🌓</button>

    <script>
        
        const colors = {
            light: {
                primary: '#6C63FF',
                secondary: '#2D3250',
                accent: '#FF6584',
                background: '#F5F5F5'
            },
            dark: {
                primary: '#7B73FF',
                secondary: '#1A1F38',
                accent: '#FF4D6D',
                background: '#121212'
            }
        };

        
        const darkModeToggle = document.getElementById('darkModeToggle');
        let isDarkMode = false;

        darkModeToggle.addEventListener('click', () => {
            isDarkMode = !isDarkMode;
            const theme = isDarkMode ? colors.dark : colors.light;
            document.documentElement.style.setProperty('--primary', theme.primary);
            document.documentElement.style.setProperty('--secondary', theme.secondary);
            document.documentElement.style.setProperty('--accent', theme.accent);
            document.documentElement.style.setProperty('--background', theme.background);
        });

    
        let emergencyTimer;
        const sosBtn = document.getElementById('sosBtn');

        sosBtn.addEventListener('mousedown', startEmergency);
        sosBtn.addEventListener('touchstart', startEmergency);
        
        sosBtn.addEventListener('mouseup', cancelEmergency);
        sosBtn.addEventListener('touchend', cancelEmergency);

        function startEmergency() {
            emergencyTimer = setTimeout(() => {
                triggerEmergency();
            }, 3000);
            sosBtn.style.animation = 'pulse 0.5s infinite';
        }

        function cancelEmergency() {
            clearTimeout(emergencyTimer);
            sosBtn.style.animation = 'pulse 2s infinite';
        }

        function triggerEmergency() {
            const loc = document.getElementById('locationInfo').innerText;
            const emergencyData = {
                message: "EMERGENCY ALERT!",
                location: loc,
                time: new Date().toLocaleString(),
                contacts: ["emergency@localauthority.com", "trusted_contact@example.com"]
            };
            
            
            alert(`Emergency alert sent!\nLocation: ${loc}`);
            navigator.vibrate([200, 100, 200]); 
        }

        
        if(navigator.geolocation) {
            navigator.geolocation.watchPosition(position => {
                const loc = position.coords;
                document.getElementById('locationInfo').innerHTML = `
                    <p>📍 Current Location:
                    <br>Lat: ${loc.latitude.toFixed(4)}
                    <br>Lon: ${loc.longitude.toFixed(4)}</p>
                    <div style="margin-top:10px; height:150px; background:#ddd; border-radius:10px;" id="map"></div>
                `;
                
            
            });
        }

        
        document.getElementById('fakeCall').addEventListener('click', () => {
            window.location.href = 'tel:0123456789';  
            setTimeout(() => {
                new Audio('https://assets.mixkit.co/active_storage/sfx/2863/2863-preview.mp3').play();
            }, 2000);
        });

        
        document.getElementById('safetyNetwork').addEventListener('click', () => {
            alert('Safety Network Features:\n\n1. Add Trusted Contacts\n2. Automatic Location Sharing\n3. Check-in System\n4. Group Alerts');
        });

        
        document.getElementById('safetyTips').addEventListener('click', () => {
            alert('Personal Safety Guide:\n\n🚨 Always share live location\n📱 Memorize emergency numbers\n🎧 Use discreet earphones\n🗺️ Plan routes in advance\n🚖 Verify ride-sharing details');
        });
    </script>
</body>
</html>
