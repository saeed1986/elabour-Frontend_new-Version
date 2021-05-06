<template>
    <div class="row">
        <aside class="col-md-3 offset-md-1">
            <div class="card bg-dark">
                <div class="row mx-auto">
                    <button class="btn text-white btn-lg" @click="showWordCloud= !showWordCloud">Show Word Cloud</button>
                </div>
                <div class="row">
                    <wordcloud
                            :data="defaultWords"
                            nameKey="name"
                            valueKey="value"
                            :color="myColors"
                            :showTooltip="true"
                            :wordClick="wordClickHandler"
                            v-if="showWordCloud"
                    >
                    </wordcloud>
                </div>
            </div>

            <h3 class="text-center bg-danger text-white" style="padding: 9px;border-radius: 4px">Additional
                Filters</h3>
            <div class="card bg-danger text-white" style="margin-bottom: 150px    ">
                <article class="card-group-item">
                    <header class="card-header">
                        <h6 class="title">Birthday</h6>
                    </header>
                    <div class="filter-content">
                        <div class="card-body">
                            <div class="form-row">
                                <div class="form-group col-md-5">
                                    <datepicker :format="format" v-model="startDate" input-class="form-control"
                                                placeholder="Select Start Date" name="startDate"></datepicker>
                                </div>
                                <div class="form-group col-md-2">
                                    <label class="form-control" style="text-align: center">TO</label>
                                </div>
                                <div class="form-group col-md-5 text-right">
                                    <datepicker :format="format" v-model="endDate" name="endDate"
                                                input-class="form-control"
                                                placeholder="Select End Date"></datepicker>
                                </div>
                            </div>
                            <div class="form-row">
                                <button class="btn btn-primary" @click="clearDate()">Clear date</button>
                            </div>
                        </div> <!-- card-body.// -->
                    </div>
                </article> <!-- Birthday// -->
                <article class="card-group-item">
                    <header class="card-header">
                        <h6 class="title">Education </h6>
                    </header>
                    <div class="filter-content">
                        <div class="card-body">
                            <div class="custom-control custom-checkbox" v-for="(education,index) in educations"
                                 :key="education">
                                <span class="float-right badge badge-light round"
                                      v-text="counter('education',education)"></span>
                                <input type="checkbox" class="custom-control-input" :id="'edu-'+index"
                                       :value="education" v-model="educationsFilteredList">
                                <label class="custom-control-label" :for="'edu-'+index">{{education}}</label>
                            </div>
                        </div> <!-- card-body.// -->
                    </div>
                </article> <!-- Education// -->
                <article class="card-group-item">
                    <header class="card-header">
                        <h6 class="title">Job </h6>
                    </header>
                    <div class="filter-content">
                        <div class="card-body">
                            <div class="custom-control custom-checkbox" v-for="(job,index) in jobs" :key="job">
                                <span class="float-right badge badge-light round" v-text="counter('job',job)"></span>
                                <input type="checkbox" class="custom-control-input" v-model="jobsFilteredList"
                                       :value="job" :id="'job-'+index">
                                <label class="custom-control-label" :for="'job-'+index">{{job}}</label>
                            </div>
                        </div> <!-- card-body.// -->
                    </div>
                </article> <!-- Jobs// -->
                <article class="card-group-item">
                    <header class="card-header">
                        <h6 class="title">Blood </h6>
                    </header>
                    <div class="filter-content">
                        <div class="card-body">
                            <div class="custom-control custom-checkbox" v-for="(bld,index) in blood" :key="bld">
                                <span class="float-right badge badge-light round" v-text="counter('blood',bld)"></span>
                                <input type="checkbox" class="custom-control-input" v-model="bloodFilteredList"
                                       :value="bld" :id="'bld-'+index">
                                <label class="custom-control-label" :for="'bld-'+index">{{bld}}</label>
                            </div>
                        </div> <!-- card-body.// -->
                    </div>
                </article> <!-- Blood// -->
                <article class="card-group-item">
                    <header class="card-header">
                        <h6 class="title">City </h6>
                    </header>
                    <div class="filter-content">
                        <div class="card-body">
                            <div class="custom-control custom-checkbox" v-for="(city,index) in cities" :key="city">
                                <span class="float-right badge badge-light round" v-text="counter('city',city)"></span>
                                <input type="checkbox" class="custom-control-input" v-model="citiesFilteredList"
                                       :value="city" :id="'city-'+index">
                                <label class="custom-control-label" :for="'city-'+index">{{city}}</label>
                            </div>
                        </div> <!-- card-body.// -->
                    </div>
                </article> <!-- city// -->
            </div> <!-- card.// -->
        </aside>
        <div class="col-md-7 ">
            <div class="result">
                <h3 v-if="resultsFound >0 && resultsFound <500">{{resultsFound}} search results found</h3>
                <h3 v-if="resultsFound === 500">all persons</h3>
                <h3 v-if="resultsFound ===   0 ">not result</h3>
            </div>
            <div class="card-columns">
                <div :class="'card ' + randomBG()" v-for="person in filteredPersons" :key="person.id">
                    <div class="card-header">
                        {{person.first_name}} - {{person.last_name}}
                    </div>
                    <ul class="list-group list-group-flush">
                        <li class="list-group-item"><span>Status: </span> {{person.status}}</li>
                        <li class="list-group-item"><span>birthday: </span>{{person.birthday}}</li>
                        <li class="list-group-item"><span>job: </span>{{person.job}}</li>
                        <li class="list-group-item"><span>education: </span> {{person.education}}</li>
                        <li class="list-group-item"><span>blood: </span> {{person.blood}}</li>
                        <li class="list-group-item"><span>city: </span> {{person.city}}</li>
                    </ul>
                </div>
            </div>
            <div class="row">
                <div class="clearfix btn-group col-md-2 offset-md-5">
                    <button type="button" class="btn btn-sm btn-outline-light" v-if="currentPage != 1"
                            @click="currentPage--" v-text="'<<'"></button>
                    <button type="button" class="btn btn-sm btn-outline-light" :key="pageNumber"
                            v-for="pageNumber in pages.slice(currentPage-1, currentPage+5)"
                            @click="currentPage = pageNumber"> {{pageNumber}}
                    </button>
                    <button type="button" @click="currentPage++" v-if="currentPage < pages.length"
                            class="btn btn-sm btn-outline-light"> >>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {base} from '../storage/Data.json'
    import Datepicker from 'vuejs-datepicker';
    import moment from 'moment';
    import wordcloud from 'vue-wordcloud'


    export default {
        name: 'search',
        data() {
            return base;
        },
        components: {Datepicker, wordcloud},
        computed: {
            filteredPersons() {
                let result = this.getByStatus(
                    this.getByBirthday(
                        this.getByCity(
                            this.getByBlood(
                                this.getByJob(
                                    this.getByEduction(
                                        this.getByFirstName(
                                            this.persons, this.search),
                                        this.educationsFilteredList),
                                    this.jobsFilteredList),
                                this.bloodFilteredList),
                            this.citiesFilteredList),
                        this.startDate, this.endDate),
                    this.statusFiltered);

                this.setPages(result);
                this.setResultFound(result);
                this.additionalFiltersCountsHandler(result);
                return this.paginate(result);
            },
        },
        methods: {
            selectJobs() {
                this.persons.map(person => {
                    if (this.jobs.includes(person.job) === false) this.jobs.push(person.job);
                })
            },
            selectBlood() {
                this.persons.map(person => {
                    if (this.blood.includes(person.blood) === false) this.blood.push(person.blood);
                })
            },
            selectCities() {
                this.persons.map(person => {
                    if (this.cities.includes(person.city) === false) this.cities.push(person.city);
                })
            },
            selectEducations() {
                this.persons.map(person => {
                    if (this.educations.includes(person.education) === false) this.educations.push(person.education);
                })
            },
            getByFirstName(persons, firstName) {
                if (firstName === "") {
                    return persons;
                }
                return persons.filter(person => {
                    return person.first_name.toLowerCase().startsWith(firstName.toLowerCase())
                })
            },
            getByEduction(persons, eduction) {
                if (!eduction.length > 0) return persons;
                return persons.filter(person => {
                    return eduction.includes(person.education)
                })
            },
            getByJob(persons, job) {
                if (!job.length > 0) return persons;
                return persons.filter(person => {
                    return job.includes(person.job)
                })
            },
            getByBlood(persons, blood) {
                if (!blood.length > 0) return persons;
                return persons.filter(person => {
                    return blood.includes(person.blood)
                })
            },
            getByCity(persons, city) {
                if (!city.length > 0) return persons;
                return persons.filter(person => {
                    return city.includes(person.city)
                })
            },
            getByBirthday(persons, date1, date2) {
                if (date1 === ""|| date2 === "") return persons;
                if (date1 === "Invalid date"|| date2 === "Invalid date") return persons;
                return persons.filter(person => {
                    return person.birthday > date1 && person.birthday < date2
                })
            },
            getByStatus(persons, status) {
                if (status === 'all') return persons;
                return persons.filter(person => {
                    return person.status.includes(status.toLowerCase())
                })
            },
            randomBG() {
                return this.background[Math.floor(Math.random() * this.background.length)];

            },
            counter(type, value) {
                let result = this.additionalFiltersCounts.filter(person => {
                    return person[type].includes(value)
                });
                return result.length
            },
            setResultFound(list) {
                this.resultsFound = list.length;
            },
            setPages(list) {
                let numberOfPages = Math.ceil(list.length / this.perPage);
                this.pages = [];
                for (let index = 1; index <= numberOfPages; index++) {
                    this.pages.push(index);
                }
            },
            paginate(persons) {
                let page = this.currentPage;
                let perPage = this.perPage;
                let from = (page * perPage) - perPage;
                let to = (page * perPage);
                return persons.slice(from, to);
            },
            additionalFiltersCountsHandler(list) {
                this.additionalFiltersCounts = list
            },
            wordClickHandler(name) {
                this.search=name;
            },
            clearDate(){
                this.startDate = "";
                this.endDate = "";
            }

        },
        watch: {
            startDate(newDate) {
                this.startDate = moment(newDate).format('YYYY-MM-DD');
            },
            endDate(newDate) {
                this.endDate = moment(newDate).format('YYYY-MM-DD');
            },

        },
        created() {
            this.selectJobs();
            this.selectCities();
            this.selectBlood();
            this.selectEducations();
        }

    }
</script>

<style scoped>

</style>

