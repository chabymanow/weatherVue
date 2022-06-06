<template>
    <div class="weatherDiv">
        <div class="tempDiv">
            <div class="desc">{{ desc }}</div>
            <div class="temp">{{ temp_cel }}&#8451;</div>
        </div>
        <div class="info">
            <div class="date">{{ date }}</div>
            <div class="location">{{ city }}, {{ location }}</div>
            <div>Feels like {{ feels }} &#8451;</div>
        </div>
        <div class="iconDiv"><img class="icon" :src="iconCode" alt="Wheather Icon"></div>
        <div class="logo">
            <a href="https://www.weatherapi.com/" title="Free Weather API"><img src='//cdn.weatherapi.com/v4/images/weatherapi_logo.png' alt="Weather data by WeatherAPI.com" border="0"></a>
        </div>
    </div>
</template>

<script>
    import axios from "axios";
    import weatherIcons from "@/assets/weather_conditions.json";

    export default{
        name: "WeahterDiv",

        data() {
            return {
                temp_cel: "",
                temp_far: "",
                desc: "",
                location:"",
                city: "",
                date: "",
                feels: "",
                iconCode:"",
                iconFile:"",
                weatherData:[],
                weatherIcons,
                api_key: "GET_YOUR_API_KEY" //https://www.weatherapi.com/
            }
        },
        props:{
            main_city: {type: String, default: "London"},
        },
        mounted(){
            this.getData();
        },
        methods: {
            async getData(){
                const url="http://api.weatherapi.com/v1/current.json?key="+this.api_key+"&q="+this.main_city;
                try{
                    const response = await axios.get(url)
                    console.log(url)
                    this.temp_cel  = response["data"]["current"]["temp_c"];
                    this.temp_far  = response["data"]["current"]["temp_f"];
                    this.feels  = response["data"]["current"]["feelslike_c"]
                    this.location = response["data"]["location"]["country"];
                    this.city = response["data"]["location"]["name"];
                    this.date = response["data"]["current"]["last_updated"];
                    this.desc = response["data"]["current"]["condition"]["text"];
                    this.iconCode = response["data"]["current"]["condition"]["icon"];
                    this.iconFile = weatherIcons.map((o) => o).indexOf(this.iconCode);
                }catch(e){
                    console.log(e);
                    alert("Error: "+e.response.status+"\nMessage: "+e.response.data.error.message);
                }
                //const results = response.data.results
                
            }
        },
        created () {
            setInterval(() => {
                this.getData();
            }, 1800000)
}
    }
</script>

<style scoped>
    .weatherDiv{
        width: 500px;
        height: 150px;
        margin: 0 auto;
        margin: 40px 40px;
        padding: .5em 1em;
        outline: 1px solid #aaa8a8;
        outline-offset: 15px;
        box-shadow: 0 0 20px rgba(0, 0, 0, .8);
        display: flex;
        flex-direction: row;
        background: #2980b9;  /* fallback for old browsers */
        background: -webkit-linear-gradient(to right, #2c3e50, #2980b9);  /* Chrome 10-25, Safari 5.1-6 */
        background: linear-gradient(to right, #2c3e50, #2980b9); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

        border-radius: 20px;
        align-items: center;
        position: relative;
    }

    .iconDiv{
        position: absolute;
        top: -30px;
        right: -20px;
    }

    .logo{
        position: absolute;
        bottom: 10px;
        right: 10px;
        opacity: .5;
    }

    .icon{
        width: 150px;
        height: 150px;
    }

    .tempDiv{
        width: 30%;
    }

    .temp{
        font-size: 4em;
        color: #FAFAFA;
    }

    .desc{
        font-size: 1.6em;
        color: #FAFAFA;
    }

    .info{
        color: #FAFAFA;
        font-size: 1.2em;
        min-width: 50%;
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        gap: .7em;
    }

    .location{
        font-size: .7em;
    }
</style>