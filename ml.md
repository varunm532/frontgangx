---
permalink: /ml
layout: nav_ml
---
<html>
<head>
    <title>Slide Navbar</title>
    <link rel="stylesheet" type="text/css" href="ml-styles.css">
    <link href="https://fonts.googleapis.com/css2?family=Jost:wght@500&display=swap" rel="stylesheet">
</head>
<body>
    <div class="main">        
        <input type="checkbox" id="chk" aria-hidden="true">
            <div class="signup">
                <form>
                    <label for="chk" aria-hidden="true">Titanic Survival Predictor</label>
                    <p id="result" aria-hidden="true"></p>
                    <input id="Name" placeholder="Name" required="">
                    <select id="class" required>
                        <option disabled selected>Passenger class</option>
                        <option value="1">1st</option>
                        <option value="2">2nd</option>
                        <option value="3">3rd</option>
                    </select>
                    <select id="sex">
                        <option disabled selected>Sex</option>
                        <option value="male">Male</option>
                        <option value="female">Female</option>
                    </select>
                    <input id="age" placeholder="Age">
                    <input id="price" placeholder="Ticket Price">
                    <select id="embark">
                        <option disabled selected>Embark</option>
                        <option value="C">Cherbourg</option>
                        <option value="Q">Queenstown</option>
                        <option value="S">Southampton</option>
                    </select>
                    <input id="sibsp" placeholder="# of siblings/spouses aboard">
                    <input id="parch" placeholder="# of parents/children aboard">
                    <select id="alone">
                        <option disabled selected>Are you traveling alone?</option>
                        <option value="True">Yes</option>
                        <option value="False">No</option>
                    </select>
                    <button type="button" onclick="mltitanic()">Predict</button>
                </form>
            </div>
            <div class="login">
                <form>
                    <label for="chk" aria-hidden="true">Food Predictor</label>
                    <p id="resultx" aria-hidden="true"></p>
                    <select id="TimeOfDay">
                        <option disabled selected>Time of Day</option>
                        <option value="Morning">Morning</option>
                        <option value="Afternoon">Afternoon</option>
                    </select> </br>
                    <select id="DayOfWeek">
                        <option disabled selected>Day of Week</option>
                        <option value="Weekend">Weekend</option>
                        <option value="Weekday">Weekday</option>
                    </select>
                    <input id="time" type="text" placeholder="Time" onfocus="(this.type='time')">
                    <button type="button" onclick="extra()">Predict</button>
                </form>
            </div>
    </div>
    <script>
        function mltitanic() {
            var dom = document.getElementById('result');
            var name = document.getElementById('Name').value;
            var pclass = document.getElementById('class').value;
            var sex = document.getElementById('sex').value;
            var age = document.getElementById('age').value;
            var fare = document.getElementById('price').value;
            var embarked = document.getElementById('embark').value;
            var sibsp = document.getElementById('sibsp').value;
            var parch = document.getElementById('parch').value;
            var alone = document.getElementById('alone').value;
            var passenger = {
                pclass: pclass,
                name: name,
                sex: sex,
                age: age,
                sibsp: sibsp,
                parch: parch,
                fare: fare,
                embarked: embarked,
                alone: alone
            };
            var url = 'http://127.0.0.1:8086/api/titanic/predict'; // Specify your URL
            var json = JSON.stringify(passenger);
            const authOptions = {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: json,
                credentials: 'include'
            };
            fetch(url, authOptions)
                .then(response => {
                    if (!response.ok) {
                        throw new Error(`HTTP error! Status: ${response.status}`);
                    }
                    return response.json();
                })
                .then(data => {
                    console.log('success', data);
                    dom.innerText = "Death Probability:" + data["Death probability"];
                    // Display in alert
                    alert("Death Probability: " + data["Death probability"]);
                })
                .catch(error => {
                    console.error('error', error);
                    // Handle error
                    dom.innerText = "Error occurred";
                });
        }
        function extra() {
            var dom = document.getElementById('resultx');
            var TOD = document.getElementById('TimeOfDay').value;
            var DOW = document.getElementById('DayOfWeek').value;
            var time = document.getElementById('time');
            var enteredTime = time.value + ":00"
            var payload = {
                Time: enteredTime,
                DayPart: TOD,
                DayType: DOW,
            };
            var url = 'http://127.0.0.1:8086/api/food/predict'
            var json = JSON.stringify(payload);
            console.log(json)
            const authOptions = {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: json,
                credentials: 'include'
            };
            fetch(url, authOptions)
                .then(response => {
                    if (!response.ok) {
                        throw new Error(`HTTP error! Status: ${response.status}`);
                    }
                    return response.json();
                })
                .then(data => {
                    console.log('success', data);
                    dom.innerText = "Predicted Item: " + data["item"]
                // Display in alert
                    alert("Predicted Item: " + data["item"]);
                })                
                .catch(error => {
                    console.error('error', error);
                    // Handle error
                    dom.innerText = "Error occurred";
                });
        }
    </script>
</body>
</html>