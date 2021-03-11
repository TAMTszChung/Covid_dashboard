<template>
    <div id="app">
        <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
            <a class="navbar-brand" href="#">Covid-19 Summary</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarText" aria-controls="navbarText" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarText">
                <ul class="navbar-nav mr-auto">
                    <li class="nav-item">
                        <a class="nav-link" href="#top">Summary in China</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#case4">{{ case4 }}</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#case1">{{ case1 }}</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#case2">{{ case2 }}</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#case3">{{ case3 }}</a>
                    </li>
                </ul>
                <span class="navbar-text">
                    Stay Caution, Stay Save.
                </span>
            </div>
        </nav>

        <div class="container">
            <div id="top" class="row mt-5">
                <div class="col text-center">
                    <h1>Covid-19 Summary in China</h1>
                </div>
            </div>

            <div id="case4" class="row mt-5" v-if="caseActive.length > 0">
                <div class="col">
                    <h2>{{ case4 }}</h2>
                    <line-chart :chartData="caseActive" :options="chartOptions" label="Active Cases" :chartColors="ActiveChartColor"></line-chart>
                </div>
            </div>

            <div id="case1" class="row mt-5" v-if="caseConfirmed.length > 0">
                <div class="col">
                    <h2>{{ case1 }}</h2>
                    <line-chart :chartData="caseConfirmed" :options="chartOptions" label="Confirmed Cases" :chartColors="confirmedChartColor"></line-chart>
                </div>
            </div>

            <div id="case2" class="row mt-5" v-if="caseDeaths.length > 0">
                <div class="col">
                    <h2>{{ case2 }}</h2>
                    <line-chart :chartData="caseDeaths" :options="chartOptions" label="Death Cases" :chartColors="deathChartColor"></line-chart>
                </div>
            </div>

            <div id="case3" class="row mt-5" v-if="caseRecovered.length > 0">
                <div class="col">
                    <h2>{{ case3 }}</h2>
                    <line-chart :chartData="caseRecovered" :options="chartOptions" label="Recovered Cases" :chartColors="recoveredChartColor"></line-chart>
                </div>
            </div>

            <div class="row mt-5 bg-warning text-center" v-else>
                <div class="col">
                    <h1 class="">API currently Unavailable</h1>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
import LineChart from "./components/LineChart";

    export default {
        name: 'App',
        components: {
            LineChart
        },

        data() {
            return {
                case1: "Confirmed Cases",
                case2: "Death Cases",
                case3: "Recovered Cases",
                case4: "Active Cases",

                caseConfirmed: [],
                confirmedChartColor: {
                    borderColor: "#077187",
                    backgroundColor: "#74A57F",
                    pointBorderColor: "#0E1428",
                    pointBackgroundColor: "#AFD6AC"
                },

                caseDeaths: [],
                deathChartColor: {
                    borderColor: "#251F47",
                    backgroundColor: "#858EAB",
                    pointBorderColor: "#260F26",
                    pointBackgroundColor: "#858EAB"
                },

                caseRecovered: [],
                recoveredChartColor: {
                    borderColor: "#78F4F41",
                    backgroundColor: "#BBE5ED",
                    pointBorderColor: "#784F41",
                    pointBackgroundColor: "#BBE5ED"
                },

                caseActive: [],
                ActiveChartColor: {
                    borderColor: "#4E5E66",
                    backgroundColor: "#31E981",
                    pointBorderColor: "#4E5E66",
                    pointBackgroundColor: "#31E981"
                },

                chartOptions: {
                    responsive: true,
                    maintainAspectRatio: false
                }
            };
        },

        async created() {
            const { data } = await axios.get("https://api.covid19api.com/total/country/china");

            data.forEach(d => {
                const date = moment(d.Date).format("MM/DD/YY");
                const {
                    Confirmed,
                    Deaths,
                    Recovered,
                    Active
                } = d;

                this.caseConfirmed.push({ date, total: Confirmed });
                this.caseDeaths.push({ date, total: Deaths });
                this.caseRecovered.push({ date, total: Recovered });
                this.caseActive.push({ date, total: Active });
            })

        }
    };
</script>

<style>
    @import './assets/styles.css';
</style>
