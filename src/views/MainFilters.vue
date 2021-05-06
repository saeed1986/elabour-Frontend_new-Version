<template>
    <div class="row" style="margin-top: 100px">
        <div class="col-md-3 offset-md-3" >
            <table class="table table-striped table-dark">
                <tbody>
                <tr>
                    <th>City selection</th>
                </tr>
                <tr v-for="(city,index) in cities" :key="city">
                    <td>
                        <div class="checkbox">
                            <label :for="'city-'+index">
                                <input type="checkbox" v-model="citiesFilteredList" :id="'city-'+index" :value="city">
                                <span>{{city}}</span>
                            </label>
                        </div>
                    </td>
                </tr>
                </tbody>
            </table>
        </div>
        <div class="col-md-3">
            <table class="table table-striped table-dark">
                <tbody>
                <tr>
                    <th>education selection</th>
                </tr>
                <tr v-for="(edu,index) in educations" :key="edu">
                    <td>
                        <div class="checkbox">
                            <label :for="'edu-'+index">
                                <input type="checkbox" :id="'edu-'+index" :value="edu" v-model="educationsFilteredList" >
                                <span>{{edu}}</span>
                            </label>
                        </div>
                    </td>
                </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    import {base} from '../storage/Data.json'
    export default {
        name: 'MainFilters',
        data() {
            return base;
        },
        methods: {
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
        },
        created() {
            this.selectCities();
            this.selectEducations();
        }

    }
</script>

<style scoped>

</style>
