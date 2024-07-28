# Weather-App_1st_Project
This is my 2nd Git repository
Author-Damini Patil
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather-App</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflre.com/ajax/libs/bootstrap/5.1.3/css/bootstrap.min.css">
    <link rel="stylesheet" href="style.css">
    <link rel="manifest" href="n=manifest.json">
</head>
<body>
    
    <div class="card mx-auto mt-5 p-4 shadow-sm" style="max-width: 400px;">
        <div class="search mb-3">
            <input type="text" class="form-control" placeholder="Enter city name" spellcheck="false" id="cityInput">
            <button class="btn btn-primary mt-2 w-100" id="searchBtn">
                <img src="images/search.png" alt="Search" style="width: 20px;">
            </button>
        </div>
        <div class="weather text-center">
            <img src="images/rain.png" class="weather-icon" alt="Weather Icon" id="weatherIcon">
            <h1 class="temp">22°C</h1>
            <h2 class="city">New York</h2>
            <div class="details">
                <div class="col mb-2">
                    <img src="images/humidity.png" alt="Humidity Icon">
                    <div>
                        <p class="humidity">50%</p>
                        <p>humidity</p>
                    </div>
                </div>
                <div class="col">
                    <img src="images/wind.png" alt="Wind Icon">
                    <div>
                        <p class="wind">15 km/h</p>
                        <p>wind speed</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div class="chart-container my-3">
        <canvas id="tempChart"></canvas> 
    </div>
      <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        const apiKey = "efc8bf0f225358be276e903074596fe0";
        const apiUrl = "https://api.openweathermap.org/data/2.5/weather?units=metric&q=";    
        
        const searchBox = document.querySelector("#cityInput");   
        const searchBtn = document.querySelector("#searchBtn");

        async function checkWeather(city) {
            const response = await fetch(`${apiUrl}${city}&appid=${apiKey}`);
            if (response.ok) {
                const data = await response.json();
                console.log(data);
                document.querySelector(".city").innerHTML = data.name;
                document.querySelector(".temp").innerHTML = Math.round(data.main.temp) + "°C";
                document.querySelector(".humidity").innerHTML = data.main.humidity + "%";
                document.querySelector(".wind").innerHTML = data.wind.speed + " km/h";
                // document.querySelector("#weatherIcon").src = 'images/${data.weather[0].icon.png}';
                updateChart(data);
            } else {
                alert("City not found");
            }
        }

        searchBtn.addEventListener("click", () => {
            checkWeather(searchBox.value);
        });
        //adding geolocation feature
        function getLocation(){
            if(navigator.geolocation){
                navigator.geolocation.getCurrentPosition(position => {
                    const lat =position.coords.lattitude;
                    const lon =position.coords.lattitude;
                    fetch('https://api.openweathermap.org/data/2.5//weather?lat=${lat}&lon=${lon}&units=metric&appid=${apiKey}')
                    .then(response => response.json())
                    .then(data =>{
                document.querySelector(".city").innerHTML = data.name;
                document.querySelector(".temp").innerHTML = Math.round(data.main.temp) + "°C";
                document.querySelector(".humidity").innerHTML = data.main.humidity + "%";
                document.querySelector(".wind").innerHTML = data.wind.speed + " km/h";

                    })
                })
            }else{
                alert("Geolocation is not supported by this brower.");
            }
        }
        //call getlocation on page load
        window.onload = getLocation;
    </script>

</body>
</html>

