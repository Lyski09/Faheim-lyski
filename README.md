# Faheim-lyski
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Muslim Ummah - Halal Chatroom</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f2f2f2;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .message {
            margin-bottom: 10px;
        }
        .message span {
            font-weight: bold;
            color: #333;
        }
        .message p {
            margin: 5px 0;
        }
        .input-container {
            margin-top: 20px;
        }
        input[type="text"], input[type="password"] {
            width: calc(100% - 80px);
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        input[type="submit"] {
            width: 80px;
            padding: 10px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Muslim Ummah - Halal Chatroom</h2>
        <p>This chatroom is dedicated to fostering halal discussions, reminding each other of Islamic teachings, and spreading goodness within the Muslim community. Let's engage in respectful communication and positive interactions as we adhere to Islamic principles and strengthen our bond as an ummah.</p>
        <div id="loginForm">
            <input type="text" id="username" placeholder="Username">
            <input type="password" id="password" placeholder="Password">
            <input type="submit" value="Login" onclick="login()">
        </div>
        <div id="chatbox" style="display: none;">
            <!-- Chat messages will appear here -->
        </div>
        <div class="input-container" id="messageInputContainer" style="display: none;">
            <input type="text" id="messageInput" placeholder="Type your message...">
            <input type="submit" value="Send" onclick="sendMessage()">
        </div>
        <div class="rooms" style="margin-top: 20px;">
            <h3>Rooms:</h3>
            <ul>
                <li><a href="#" onclick="joinRoom('Quran')">Quran Room</a></li>
                <li><a href="#" onclick="joinRoom('Hadith')">Hadith Room</a></li>
                <li><a href="#" onclick="joinRoom('Fiqh')">Fiqh Room</a></li>
                <li><a href="#" onclick="joinRoom('Marriage')">Marriage Room</a></li>
            </ul>
        </div>
    </div>

    <script>
        var isLoggedIn = false;
        var curseWords = ["fuck", "shit", "motherfucker"]; // Add more curse words as needed

        function login() {
            var username = document.getElementById("username").value;
            var password = document.getElementById("password").value;
            // Implement your authentication logic here
            // For simplicity, let's assume login is successful if username and password are not empty
            if (username.trim() !== "" && password.trim() !== "") {
                isLoggedIn = true;
                document.getElementById("loginForm").style.display = "none";
                document.getElementById("chatbox").style.display = "block";
                document.getElementById("messageInputContainer").style.display = "block";
                document.getElementById("messageInput").focus();

                // Schedule Sahih Bukhari reminder
                scheduleReminder();

                // Fetch prayer times and play Adhan
                fetchPrayerTimes();
            } else {
                alert("Please enter both username and password.");
            }
        }

        function sendMessage() {
            if (!isLoggedIn) {
                alert("Please login first.");
                return;
            }
            var messageInput = document.getElementById("messageInput");
            var message = messageInput.value;
            if (message.trim() !== "") {
                if (containsCurseWords(message)) {
                    alert("Your message contains inappropriate language. Please refrain from using offensive language.");
                    messageInput.value = "";
                    return;
                }
                var chatbox = document.getElementById("chatbox");
                var messageDiv = document.createElement("div");
                messageDiv.classList.add("message");
                messageDiv.innerHTML = "<span>You:</span><p>" + message + "</p>";
                chatbox.appendChild(messageDiv);
                messageInput.value = "";
                messageInput.focus();
            } else {
                alert("Please enter a message.");
            }
        }

        function containsCurseWords(message) {
            var lowerCaseMessage = message.toLowerCase();
            for (var i = 0; i < curseWords.length; i++) {
                if (lowerCaseMessage.includes(curseWords[i])) {
                    return true;
                }
            }
            return false;
        }

        function scheduleReminder() {
            var now = new Date();
            var millisTill10 = new Date(now.getFullYear(), now.getMonth(), now.getDate(), 10, 0, 0, 0) - now;
            if (millisTill10 < 0) {
                millisTill10 += 86400000; // it's after 10am, try 10am tomorrow.
            }
            setTimeout(function() {
                alert("Sahih Bukhari reminder: Don't forget to read Sahih Bukhari today.");
                // You can replace the alert with any other notification mechanism
                scheduleReminder(); // Reschedule for the next day
            }, millisTill10);
        }

        function fetchPrayerTimes() {
            // Simulating prayer times (replace with actual API call if needed)
            var prayerTimes = {
                'Fajr': '05:00 AM',
                'Dhuhr': '12:00 PM',
                'Asr': '03:30 PM',
                'Maghrib': '06:00 PM',
                'Isha': '08:00 PM'
            };
            var currentPrayer = getCurrentPrayer(prayerTimes);
            var prayerTime = prayerTimes[currentPrayer];

            alert("It's time for " + currentPrayer + " (Time: " + prayerTime + "). Adhan will be played shortly.");

              getCurrentPrayer(prayerTimes) {
            var now = new Date();
            var currentTime = now.getHours() * 60 + now.getMinutes();

            if (
                currentTime >= convertToMinutes(prayerTimes['Isha'])
