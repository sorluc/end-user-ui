<template>
    <div class="card mb-3">
        <div class="card-body py-4">
            <h5 class="mb-4 card-title">{{$t('pages.profile.location.title')}}</h5>
            <google-map
                :center="{lat:coordinates.lat,lng: coordinates.lng}"
                :zoom="15"
                :options="{
                    zoomControl: true,
                    mapTypeControl: false,
                    scaleControl: false,
                    streetViewControl: false,
                    rotateControl: false,
                    fullscreenControl: true,
                    disableDefaultUI: false}"
                style="width: 100%; height: 450px">
                <google-marker
                    v-if="coordinates"
                    label="★"
                    :position="{lat: coordinates.lat,lng: coordinates.lng}"
                ></google-marker>
            </google-map>
        </div>
    </div>
</template>

<script>
var coordinatesParis = { lat: 0, lng: 0 },
    addressObj = {
        address_line_1: '',
        address_line_2: '',
        city: 'Paris',
        state: '',
        zip_code: '',
        country: 'France'
    };
export default {
    name: 'GeoLoc',
    data () {
        return {
            coordinates: coordinatesParis
        };
    },
    mounted () {
        this.$geocoder.send(addressObj, response => {
            this.coordinates.lat = response.results[0].geometry.location.lat;
            this.coordinates.lng = response.results[0].geometry.location.lng;
            this.$getLocation({})
                .then(coordinates => {
                    console.log(coordinates);
                    this.coordinates = coordinates;
                })
                .catch(error => {
                    console.log('Unable to get user location');
                    console.log(error);
                });
        });
    }
};
</script>

