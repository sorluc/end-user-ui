<!--
Copyright (c) 2020 ForgeRock. All rights reserved.
@author sorluc
This software may be modified and distributed under the terms
of the MIT license. See the LICENSE file for details.
-->

<template>
    <div v-if="dataloaded" class="card">
        <div class="card-body" style="text-align:center">
            <h4>{{$t('pages.dashboard.widgets.counter.title')}} {{details}}</h4>
            <VueJsCounter start="0" :end="count" duration="5000" thousand="" decimal=","></VueJsCounter>
            {{$t('pages.dashboard.widgets.counter.description')}}
        </div>
    </div>
</template>

<script>
import VueJsCounter from 'vue-js-counter';

/**
 * @description Widget that provides a welcome message for the managed resource, also provides a button to directly access editing the resources profile.
 *
 **/
export default {
    name: 'Counter',
    props: ['userDetails', 'details', 'url'],
    components: {
        VueJsCounter
    },
    data () {
        return {
            dataloaded: false,
            deferedCount: '0'
        };
    },
    mounted () {
        this.loadData();
    },
    methods: {
        loadData () {
            this.getRequestService().get(this.url)
                .then(({ data }) => {
                    this.displayNotification('message', 'Dataloaded for Counter ' + this.details);
                    this.deferedCount = data.resultCount;
                    this.dataloaded = true;
                })
                .catch((error) => {
                    console.log(error.response.data);
                    if (error.response.data.reason === 'Forbidden') {
                        this.displayNotification('error', 'Your are not allowed to access ' + this.details + ' counter : ' + error.response.data.message);
                        this.dataloaded = false;
                    } else {
                        this.displayNotification('warning', 'Counter not available: Problem to load data for ' + this.details + ' - Details: ' + error.response.data.message);
                        this.dataloaded = true;
                    }
                });
        }
    },
    computed: {
        count () {
            return this.deferedCount;
        }
    }
};
</script>

<style lang="scss" scoped></style>
