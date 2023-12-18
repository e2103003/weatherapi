<template>
    <section id="weather-container" class="justify-content-center container " style="width:40%;">
        <div class="input-group mb-3 shadow-sm">
            <span class="input-group-text" id="addon-wrapping">🔍</span>
            <input type="text" class="form-control"  id="city-input" placeholder="City to research" required>   
            <button class="btn btn-outline-primary" type="button" id="validate-button" v-on:click="researchWeather()">Button</button>
        </div>

        <div id="data-display" class="border border-dark rounded bg-light bg-gradient pb-1 shadow " style="display:none;margin-bottom:15% ;">
            <div class="bg-info bg-gradient border-bottom border-dark border-1 rounded-top mb-0 d-flex flex-wrap" id="top">
                
            </div>
            <ul class="list-group">
                <li id="city-name" class="list-group-item bg-light"></li>
                <li id="temperature" class="list-group-item bg-light"></li>
                <li id="pression" class="list-group-item bg-light"></li>
            </ul>

            <a id="open-other-data" class="lead text-decoration-none ms-2" v-on:click="displayMoreInfo()"> click to see more info </a>
            <div class="" id="bottom" style="display: none;" >
                <ul class="list-group">
                    <li id="wind" class="list-group-item bg-light"></li>
                    <li id="humidity" class="list-group-item bg-light"></li>
                    <li id="feels" class="list-group-item bg-light"></li>
                    <li id="temp-min" class="list-group-item bg-light"></li>
                    <li id="temp-max" class="list-group-item bg-light"></li>
                </ul>


                <div class="container border border-dark" id="forecasts" >
                    <ul class="overflow-auto d-print-table-row ps-0" id="list-forecast" style="height:300px!important;"> Forecast :
                    </ul>

                </div>
            </div>
        </div>
    </section>

</template>


<script>



export default {
    name: 'vue-central',
    props: {
        msg: String
    },
    methods:{
        displayWeatherData:function(weatherData) { // displayWeatherData function
            document.getElementById('data-display').style.display = 'block'; // display weather data

            document.getElementById('top').innerHTML = " <h2 id='weather-main' class='h2 ms-2 text-dark'>Today's weather</h2> <img class='weather-icon' src='http://openweathermap.org/img/w/" + weatherData.weather[0].icon + ".png'>"; // display weather icon and header
            document.getElementById('city-name').innerHTML = "City : " + weatherData.name; // display city name
            document.getElementById('temperature').innerHTML = "Temperature : " + Math.round(weatherData.main.temp - 273.15) + "°C"; // display temperature
            document.getElementById('pression').innerHTML = "Pressure : " + weatherData.main.pressure + " hPa"; // display pressure

            document.getElementById('wind').innerHTML = "Wind speed : " + weatherData.wind.speed + " m/s"; // display wind speed
            document.getElementById('humidity').innerHTML = "Humidity : " + weatherData.main.humidity + " %"; // display humidity
            document.getElementById('feels').innerHTML = "Feels like : " + Math.round(weatherData.main.feels_like - 273.15) + "°C"; // display feels like temperature
            document.getElementById('temp-min').innerHTML = "Minimal temperature : " + Math.round(weatherData.main.temp_min - 273.15) + "°C"; // display minimum temperature
            document.getElementById('temp-max').innerHTML = "Maximum temperature : " + Math.round(weatherData.main.temp_max - 273.15) + "°C"; // display maximum temperature
        },

        researchWeather : function(){
            var apiKey = 'ee07e2bf337034f905cde0bdedae3db8';

            var city = document.getElementById("city-input").value;
            fetch('https://api.openweathermap.org/data/2.5/weather?q=' + city+ '&appid=' + apiKey)
            .then(function(resp) { return resp.json() })
            .then((data) => {
                try{
                    this.displayWeatherData(data);
                    this.forecastWeather(city);
                }catch(error){
                    alert("City not found");
                }
            })
        },

        forecastWeather : function(city){
            fetch('http://api.openweathermap.org/data/2.5/forecast?q=' + city + '&APPID=ee07e2bf337034f905cde0bdedae3db8&units=metric')
            .then(function(resp) { return resp.json() })
            .then((data) =>{
                console.log(data);
                this.displayForecast(data);
            })

        },

        displayMoreInfo : function(){
            if(document.getElementById('bottom').style.display == 'none'){
                document.getElementById('bottom').style.display = 'block';
                document.getElementById('open-other-data').innerHTML = "click to hide";
            }else{
                document.getElementById('bottom').style.display = 'none';
                document.getElementById('open-other-data').innerHTML = "click to see more info";
            }
        },

        displayForecast : function(data){

            var list = document.getElementById('list-forecast');
            list.innerHTML = "";
            for(var i = 0; i < data.list.length; i++){
                var date = new Date(data.list[i].dt_txt);
                var formattedDate = date.getDate() + "/" + (date.getMonth() + 1) + "/" + date.getFullYear();
                var formattedTime = date.getHours();
                var temp = Math.round(data.list[i].main.temp);
                var feels = Math.round(data.list[i].main.feels_like);
                var humidity = data.list[i].main.humidity;
                var wind = data.list[i].wind.speed;
                var pressure = data.list[i].main.pressure;
                var description = data.list[i].weather[0].description;

                var li = document.createElement('li');
                li.innerHTML =`
                    <li id="forecast-1" class="list-group-item bg-light">
                        <div class="row border-bottom border-dark border-1">
                            
                            <div class="col-3 mb-1 mt-2">
                                <h5>`+ formattedDate +`</h5>
                                <p>`+ formattedTime +`H</p>
                            </div>
                            <div class="col-2 mb-1 mt-2 ">
                                <img src='http://openweathermap.org/img/w/`+ data.list[i].weather[0].icon +`.png'  >
                            </div>
                            <div class="col-2 mt-2">
                                <h5>`+ temp +`°C</h5>
                                <p>Feels like `+ feels +`°C</p>
                            </div>
                            <div class="col-2 mt-2">
                                <h5>`+ humidity +`%</h5>
                                <p>Wind `+ wind +` m/s</p>
                            </div>
                            <div class="col-2 mt-2  ">
                                <h5>`+ pressure +` hPa</h5>
                                <p>`+ description +`</p>
                            </div>
                        </div>

                    </li>
                `;
                list.appendChild(li);
            }
        }



           
    }
}

</script>