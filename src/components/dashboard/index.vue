<!--
Copyright (c) 2020 ForgeRock. All rights reserved.

This software may be modified and distributed under the terms
of the MIT license. See the LICENSE file for details.
-->

<template>
    <b-container fluid>
        <b-row v-if="widgets.length">
            <div v-for="(widget, index) in widgets" :class="{'col-sm-4': widget.size === 'small', 'col-sm-6': widget.size === 'medium', 'col-sm-12': widget.size === 'large', 'mt-4': true}" :key="widget.type + index">
                <component :is="widget.type" :userDetails="userDetails" :details="widget.details" :url="widget.url"></component>
            </div>
        </b-row>
        <b-jumbotron v-else class="mt-4 text-center">
            <div class="d-flex justify-content-center mt-3">
                <i class="fa fa-binoculars align-self-center flex-fow-1 mr-4 mb-3" style="font-size:52px;"></i>
                <div class="flex-fow-1">
                    <h2>{{$t('pages.dashboard.noWidget')}}</h2>
                    <p v-html="$t('pages.dashboard.noWidgetSubText')"></p>
                </div>
            </div>
        </b-jumbotron>
    </b-container>
</template>

<script>
import Welcome from '@/components/dashboard/widgets/WelcomeWidget';
import Counter from '@/components/dashboard/widgets/Counter';
import Weather from '@/components/dashboard/widgets/Weather';
import Workflow from '@/components/dashboard/widgets/WorkflowControlWidget';
import GeoLoc from '@/components/dashboard/widgets/GeoLoc';

/**
 * @description Controlling component for the dashboard, loads widgets set and configured in the ui-dashboard config file.
 *
 * @fires GET config/ui/dashboard - Read of the config file ui-dashboard.json
 */
export default {
    name: 'Dashboard',
    components: {
        Welcome,
        GeoLoc,
        Counter,
        Weather,
        Workflow
    },
    data () {
        return {
            widgets: [],
            userDetails: this.$root.userStore.getUserState()
        };
    },
    mounted () {
        this.loadData();
    },
    methods: {
        loadData () {
            /* istanbul ignore next */
            this.getRequestService().get('config/ui/dashboard')
                .then(({ data }) => {
                    this.widgets = data.dashboard.widgets;
                    console.log(data);

                    if (this.$root.applicationStore.state.workflow) {
                        this.widgets.push({
                            type: 'Workflow',
                            size: 'large'
                        });
                    }
                })
                .catch((error) => {
                    this.displayNotification('error', error.response.data.message);
                });
        }
    }
};
</script>
