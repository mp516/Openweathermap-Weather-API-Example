# Example to fetch data from api.openweathermap.org
$(function() {
    fetchApiData();
});

weatherApiObj = {
    city: '',
    pressure: '',
    humidity: '',
    wind_speed: '',
    dt:  '', 
    temperature: '',
    feels_like: ''
};

function fetchApiData(){
    city = $( "#city-select" ).val();

    let link = 'Provide your api key';
    let timeZone = 'America/New_York';
   
    $.get(link, function(data, status){
        setWeatherData(data, timeZone);
    });
}

function setWeatherData(data, timeZone){

    const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
    const img_url = "http://openweathermap.org/img/wn/";
    

    const pressure = data.current.pressure
    const humidity = data.current.humidity
    const wind_speed = data.current.wind_speed
    const temperature = data.current.temp.toString().split(".", 1);
    const feels_like = data.current.feels_like.toString().split(".", 1);

    $("#cr_pressure").text(pressure + ' mm Hg');
    $("#cr_humidity").text(humidity + ' % humidity');
    $("#cr_wind_speed").text(wind_speed + 'm/s NW');

    const dt = new Date(data.current.dt*1000).toLocaleString("en-US", {timeZone: timeZone});
    const curTime = dt.split(',')[1];
    const curDate = new Date(data.current.dt*1000).toDateString("en-US", {timeZone: timeZone});
    $("#cr_today").text(curDate + ' ,' + curTime);

    $("#cr_main_description").text(data.current.weather[0].main + ', ' + data.current.weather[0].description);

    document.getElementById('temperature').childNodes[0].nodeValue =  temperature;
    $("#feels_like").text('+' + feels_like + ' feels');

    $("#current_icon").attr("src", img_url + data.current.weather[0].icon + "@4x.png");

    //Day 1
    $("#day_1_temp").text(data.daily[1].temp.day.toString().split(".", 1));

    const dt_1 = new Date(data.daily[1].dt*1000);
    $("#day_1").text(days[dt_1.getDay()]);
    $("#day_1_icon").attr("src", img_url + data.daily[1].weather[0].icon + "@2x.png");

    //Day 2
    $("#day_2_temp").text(data.daily[2].temp.day.toString().split(".", 1));

    const dt_2 = new Date(data.daily[2].dt*1000);
    $("#day_2").text(days[dt_2.getDay()]);
    $("#day_2_icon").attr("src", img_url + data.daily[2].weather[0].icon + "@2x.png");

    //Day 3
    $("#day_3_temp").text(data.daily[3].temp.day.toString().split(".", 1));

    const dt_3 = new Date(data.daily[3].dt*1000);
    $("#day_3").text(days[dt_3.getDay()]);
    $("#day_3_icon").attr("src", img_url + data.daily[3].weather[0].icon + "@2x.png");

    //Set weather data
    weatherApiObj.city = $("#city-select").val();
    weatherApiObj.pressure = pressure;
    weatherApiObj.humidity = humidity;
    weatherApiObj.wind_speed = wind_speed;
    weatherApiObj.dt = dt;
    weatherApiObj.temperature = temperature;
    weatherApiObj.feels_like = feels_like;

}

function saveWeatherData()
{
    data = {
        city: weatherApiObj.city,
        pressure: weatherApiObj.pressure,
        humidity: weatherApiObj.humidity,
        wind_speed: weatherApiObj.wind_speed,
        current_date: weatherApiObj.dt,
        temperature: weatherApiObj.temperature,
        feels_like: weatherApiObj.feels_like,
        submit: ''
    };

    $.post("your-url", data,
    function(data, status){
        alert("Data inserted: " + status);
    });
}
