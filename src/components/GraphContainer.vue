<template>
    <div>
        <h1>Average time or trip per user type</h1>
        <div v-for="key in averageTimePerCustomerTypeKeys" :key="key">
            <h4>Type: {{ key }}</h4>
            <p>Number of trips: {{ averageTimePerCustomerType[key].totalTrip | formatNumber }}</p>
            <p>Total time: {{ averageTimePerCustomerType[key].totalTime | formatNumber }}</p>
            <p>Average time per trips: {{ averageTimePerCustomerType[key].value | formatToMinutes }}</p>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
    name: "GraphContainer",
    data: function() {
        return {
            data: [],
            averageTimePerCustomerType: []
        };
    },
    computed: {
        averageTimePerCustomerTypeKeys: function() {
            return Object.keys(this.averageTimePerCustomerType);
        }
    },
    created() {
        this.fetchData();
    },
    methods: {
        fetchData() {
            this.error = this.post = null;
            this.loading = true;
            axios
                .get(
                    "https://data.cityofchicago.org/resource/fg6s-gzvg.json?from_station_id=418&$limit=10000&$offset=000"
                )
                .then(response => {
                    console.log(response.data[0]);
                    this.data = response.data;
                    this.getAverageTimePerCustomerType();
                });
        },
        getAverageTimePerCustomerType() {
            let average = {};
            this.data.forEach(trip => {
                let diff = moment(trip.stop_time).diff(moment(trip.start_time));
                if (diff < 300 * 60000) {
                    if (!average[trip.user_type]) {
                        average[trip.user_type] = {
                            totalTime: diff,
                            totalTrip: 1,
                            value: diff
                        };
                    } else {
                        average[trip.user_type].totalTime += diff;
                        average[trip.user_type].totalTrip++;
                        average[trip.user_type].value =
                            average[trip.user_type].totalTime /
                            average[trip.user_type].totalTrip;
                    }
                }
            });
            this.averageTimePerCustomerType = average;
        }
    },
    filters: {
        formatToMinutes(millis) {
            var minutes = Math.floor(millis / 60000);
            var seconds = ((millis % 60000) / 1000).toFixed(0);
            return minutes + ":" + (seconds < 10 ? "0" : "") + seconds;
        },
        formatNumber(num) {
            return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
        }
    }
};
</script>
<style scoped>
</style>