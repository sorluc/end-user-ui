<!--
Copyright (c) 2020 ForgeRock. All rights reserved.
@author sorluc
This software may be modified and distributed under the terms
of the MIT license. See the LICENSE file for details.
-->

<template>
    <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-sm">
                    <div class="card">
                        <div class="card-body">
                            <h3>Compteurs de X</h3>
                            <VueJsCounter start="1900" end="1999" duration="500" thousand="" decimal=","></VueJsCounter>
                        </div>
                    </div>
                </div>
                <div class="col-sm">
                    <div class="card">
                        <div class="card-body">
                            <h3>Compteurs de Y</h3>
                            <VueJsCounter end="1000000" thousand=""></VueJsCounter>
                        </div>
                    </div>
                </div>
                <div class="col-sm">
                    <div class="card">
                        <div class="card-body">
                            <h3>Compteurs de Z</h3>
                            <VueJsCounter start="2020" end="1900" thousand=""></VueJsCounter>
                        </div>
                    </div>
                </div>
                <div class="col-sm">
                    <div class="card">
                        <div class="card-body">
                            <h3>Compteurs de X</h3>
                            <VueJsCounter start="1900" end="1999" duration="500" thousand="" decimal=","></VueJsCounter>
                        </div>
                    </div>
                </div>
                <div class="col-sm">
                    <div class="card">
                        <div class="card-body">
                            <h3>Compteurs de Y</h3>
                            <VueJsCounter end="1000000" thousand=""></VueJsCounter>
                        </div>
                    </div>
                </div>
                <div class="col-sm">
                    <div class="card">
                        <div class="card-body">
                            <h3>Compteurs de Z</h3>
                            <VueJsCounter start="2020" end="1900" thousand=""></VueJsCounter>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import _ from 'lodash';
import VueJsCounter from 'vue-js-counter';

/**
 * @description Widget that provides a welcome message for the managed resource, also provides a button to directly access editing the resources profile.
 *
 **/
export default {
    name: 'Counters',
    props: ['userDetails', 'details'],
    components: {
        VueJsCounter
    },
    data () {
        return {};
    },
    mounted () {
        console.log('THIS.$ROOT.USERsTORE.GETuSERsTATE()');
        console.log(this.$root.userStore.getUserState());
        console.log('THIS.$ROOT.USERsTORE.GETuSERsTATE()');
    },
    methods: {
        openProfile () {
            this.$router.push({ name: 'Profile', params: { openProfile: !this.$root.userStore.state.internalUser } });
        }
    },
    computed: {
        fullName () {
            let fullName = '';

            if (this.userDetails.givenName.length > 0 || this.userDetails.sn.length > 0) {
                fullName = _.startCase(this.userDetails.givenName + ' ' + this.userDetails.sn);
            } else {
                fullName = this.userDetails.userId;
            }

            return fullName;
        }
    }
};
</script>

<style lang="scss" scoped></style>
