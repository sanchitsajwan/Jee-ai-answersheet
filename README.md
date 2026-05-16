<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>25-Slot Answering Tool</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            padding: 20px;
            margin: 0;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        #grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }
        .slot-card {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            transition: border 0.3s ease;
            border: 2px solid transparent;
        }
        .slot-card.active {
            border: 2px solid #007BFF;
            box-shadow: 0 4px 10px rgba(0, 123, 255, 0.3);
        }
        .slot-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            font-weight: bold;
            color: #555;
        }
        .timer {
            color: #d9534f;
            font-family: monospace;
            font-size: 1.1em;
        }
        textarea {
            width: 100%;
            height: 80px;
            resize: vertical;
            padding: 8px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-family: inherit;
        }
        textarea:focus {
            outline: none;
        }
    </style>
</head>
<body>

    <h1>Answering Tracker</h1>
    <div id="grid-container"></div>

    <script>
        const TOTAL_SLOTS = 25;
        const container = document.getElementById('grid-container');
        
        // Array to store the time in seconds for each slot
        let slotTimes = new Array(TOTAL_SLOTS).fill(0);
        let activeInterval = null; // Holds the timer process

        // Helper function to format seconds into MM:SS
        function formatTime(totalSeconds) {
            const minutes = Math.floor(totalSeconds / 60);
            const seconds = totalSeconds % 60;
            return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        // Generate the 25 slots dynamically
        for (let i = 0; i < TOTAL_SLOTS; i++) {
            // Create Card
            const card = document.createElement('div');
            card.className = 'slot-card';
            card.id = `card-${i}`;

            // Create Header (Title + Timer)
            const header = document.createElement('div');
            header.className = 'slot-header';
            
            const title = document.createElement('span');
            title.innerText = `Slot ${i + 1}`;
            
            const timerDisplay = document.createElement('span');
            timerDisplay.className = 'timer';
            timerDisplay.id = `timer-${i}`;
            timerDisplay.innerText = "00:00";

            header.appendChild(title);
            header.appendChild(timerDisplay);

            // Create Textarea
            const textarea = document.createElement('textarea');
            textarea.placeholder = "Start typing here...";
            
            // Start timer when user clicks into the textarea
            textarea.addEventListener('focus', () => {
                card.classList.add('active'); // Highlight the active box
                
                // Ensure no other timers are running
                clearInterval(activeInterval); 
                
                // Start incrementing this specific slot's time every second
                activeInterval = setInterval(() => {
                    slotTimes[i]++;
                    timerDisplay.innerText = formatTime(slotTimes[i]);
                }, 1000);
            });

            // Stop timer when user clicks away from the textarea
            textarea.addEventListener('blur', () => {
                card.classList.remove('active'); // Remove highlight
                clearInterval(activeInterval); // Pause the timer
            });

            // Put it all together
            card.appendChild(header);
            card.appendChild(textarea);
            container.appendChild(card);
        }
    </script>
</body>
</html>
