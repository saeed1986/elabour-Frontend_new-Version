<template>
    <div id="app">
        <div class="home">
            <div class="row">
                <div class="input-group mx-auto" style="width: 70%">
                    <div class="input-group mb-3">
                        <div class="input-group-prepend">
                            <select name="" class="form-control type-change"  v-model="statusFiltered">
                                <option :selected="statusFiltered === 'all'" value="all" >All</option>
                                <option :selected="statusFiltered === 'live'" value="live" >Live</option>
                                <option :selected="statusFiltered === 'dead'" value="dead" >Dead</option>
                            </select>
                        </div>
                      <div class="search-c">
                          <autocomplete :search="handelSearch" @submit="handelSubmit" ></autocomplete>
                      </div>
                        <div class="input-group-append">
                            <router-link tag="button" :to="'search'" class="btn btn-danger" type="button">
                                <i class="fa fa-search"></i>
                            </router-link>
                        </div>
                    </div>
                </div>
            </div>
            <router-view/>
        </div>
    </div>
</template>

<style>
    .search-c{
        position: relative;
        -ms-flex: 1 1 auto;
        flex: 1 1 auto;
        width: 1%;
        margin-bottom: 0;
    }
</style>
<script>
    import {base} from './storage/Data.json'
    export default {
        name: 'home',
        data() {
            return base;
        },
        methods: {
            handelSearch(input) {
                this.search=input;
                if (input.length < 1) { return [] }
                if (this.search.length < 1) { return [] }
                return this.persons.filter(p => {
                    return p.first_name.toLowerCase()
                        .startsWith(this.search.toLowerCase())
                }).map(p=>{
                   return p.first_name
                })
            },
            handelSubmit(result){
                this.search = result
            }
        },
    }
</script>
