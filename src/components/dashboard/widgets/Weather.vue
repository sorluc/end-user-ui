<!--
Copyright (c) 2020 ForgeRock. All rights reserved.
@author sorluc
This software may be modified and distributed under the terms
of the MIT license. See the LICENSE file for details.
-->

<template>
    <div class="card card-weather">
        <div class="card-body">
            <div class="weather-date-location">
                <h3>{{dayLong}}</h3>
                <p class="text-gray"> <span class="weather-date">{{date}}</span> -  <span class="weather-location">{{this.addressObj.city}}, {{this.addressObj.country}}</span> </p>
            </div>
            <div class="weather-data d-flex">
                <div class="mr-auto">
                    <h4 class="display-3">{{todayTemp}} <span class="symbol">°</span>C</h4>
                    <!--<p> <b>{{todayMain}}</b> - {{todayDesc}}</p>-->
                    <p> <b>{{todayDesc}}</b> </p>
                </div>
                <img :src="'http://openweathermap.org/img/wn/'+ todayIcon +'@2x.png'" v-bind:alt="todayMain"/>
            </div>
        </div>
        <div class="card-body p-0">
            <div class="d-flex weakly-weather">
                <div v-for="day in orderedDays" :key="day.dt" class="weakly-weather-item">
                    <p class="mb-1"> {{ getDay(new Date((day.dt + timezoneOffset) * 1000)) }} </p>
                    <img :src="'http://openweathermap.org/img/wn/'+ day.weather[0].icon +'.png'" v-bind:alt="day.weather[0].main"/><i class="mdi mdi-weather-snowy"></i>
                    <p class="mb-0"> {{ day.temp.day }}° </p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import _ from 'lodash';
import axios from 'axios';

/**
 * @description Widget that provides a welcome message for the managed resource, also provides a button to directly access editing the resources profile.
 *
 **/
export default {
    name: 'Weather',
    props: ['userDetails', 'details'],
    components: {
    },
    data () {
        return {
            date: '',
            dayLong: '',
            coordinates: { lat: 0, lng: 0 },
            addressObj: {
                address_line_1: '',
                address_line_2: '',
                city: 'Paris',
                state: '',
                zip_code: '',
                country: 'France'
            },
            todayTemp: '0',
            todayMain: 'Loading...',
            todayDesc: 'Loading...',
            todayIcon: '02d',
            todayWeather: '',
            dailyWeather: [],
            timezoneOffset: 0
        };
    },
    mounted () {
        console.log(this.addressObj);
        var currentDate = new Date();
        this.dayLong = _.startCase(currentDate.toLocaleString(this.$i18n.locale, { weekday: 'long' }));
        this.date = _.startCase(currentDate.toLocaleString(this.$i18n.locale, { year: 'numeric', month: 'long', day: 'numeric' }));
        axios.get('https://api.openweathermap.org/data/2.5/onecall?lat=48.8314905&lon=2.3295866&appid=' + this.openWeatherMapKey() + '&exclude=hourly,minutely&units=metric&lang=' + this.$i18n.locale)
            .then(result => {
                console.log(result.data);
                this.timezoneOffset = result.data.timezone_offset;
                this.todayTemp = result.data.current.temp;
                this.todayWeather = result.data.current.weather[0];
                this.todayMain = _.startCase(result.data.current.weather[0].main);
                this.todayDesc = _.capitalize(result.data.current.weather[0].description);
                this.todayIcon = result.data.current.weather[0].icon;
                this.dailyWeather = result.data.daily;
            }, error => {
                console.log(error);
            });
    },
    methods: {
        getDay (date) {
            return _.startCase(date.toLocaleString(this.$i18n.locale, { weekday: 'short' }));
        }
    },
    computed: {
        orderedDays: function () {
            return (_.orderBy(this.dailyWeather, 'dt')).slice(1);
        }
    }
};
</script>

<style lang="scss" scoped>
 .stretch-card>.card {
     width: 100%;
     min-width: 100%
 }

 .flex {
     -webkit-box-flex: 1;
     -ms-flex: 1 1 auto;
     flex: 1 1 auto
 }

 @media (max-width:991.98px) {
     .padding {
         padding: 1.5rem
     }
 }

 @media (max-width:767.98px) {
     .padding {
         padding: 1rem
     }
 }

 .padding {
     padding: 5rem
 }

 .grid-margin,
 .purchace-popup>div {
     margin-bottom: 25px
 }

 .card {
     border: 0;
     border-radius: 2px
 }

 .card-weather {
     background: #e1ecff;
     background-image: linear-gradient(to left bottom, #d6eef6, #dff0fa, #e7f3fc, #eff6fe, #f6f9ff)
 }

 .card {
     position: relative;
     display: flex;
     flex-direction: column;
     min-width: 0;
     word-wrap: break-word;
     background-color: #fff;
     background-clip: border-box;
     border: 1px solid rgba(0, 0, 0, 0.125);
     border-radius: 0.25rem
 }

 .card-weather .card-body:first-child {
     /* background: url(https://res.cloudinary.com/dxfq3iotg/image/upload/v1557323760/weather.svg) no-repeat center;
     background-size: cover */
     background-color: #4158D0;
     background-image: linear-gradient(43deg, #4158D0 0%, #C850C0 46%, #FFCC70 100%);
 }

 .card .card-body {
     padding: 1.88rem 1.81rem
 }

 .card-body {
     flex: 1 1 auto;
     padding: 1.25rem
 }

 .card-weather .weather-date-location {
     padding: 0 0 38px
 }

 .h3,
 h3,
 .h4,
 h4 {
     font-size: 1.56rem;
     font-family: "Poppins", sans-serif;
     font-weight: 500;
     color: #ffffff
 }

 #appContentWrapper > div > div > div:nth-child(2) > div > div:nth-child(1) > div.weather-data.d-flex > div > p {
     color: #ffffff
 }

 .text-gray,
 .card-subtitle,
 .new-accounts ul.chats li.chat-persons a p.joined-date {
     color: #ffffff
 }

 p {
     font-size: 13px
 }

 .card-weather .weather-data {
     /* padding: 0 0 4.75rem */
     padding: 0
 }

 .mr-auto,
 .mx-auto {
     margin-right: auto !important
 }

 .display-3 {
     font-size: 2.5rem
 }

 .card-weather .card-body {
     background: #ffffff
 }

 .card-weather .weakly-weather {
     background: #ffffff;
     overflow-x: auto
 }

 .card-weather .weakly-weather .weakly-weather-item {
     flex: 0 0 14.28%;
     border-right: 1px solid #f2f2f2;
     padding: 1rem;
     text-align: center
 }

 .mb-0,
 .my-0 {
     margin-bottom: 0 !important
 }

 .card-weather .weakly-weather .weakly-weather-item i {
     font-size: 1.2rem
 }
</style>
