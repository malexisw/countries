<template>
    <div class="home">
        <div :class="mode">
            <div class="header">
                <div class="content">
                    <h1 class="sitename">
                        Where in the world?
                    </h1>
                    <div class="mode" @click="themeChange()">
                        Dark Mode
                    </div>
                </div>
            </div>
            <div class="filter">
                <input type="text" class="search" placeholder="Search for a country..." v-model="search">

                <select name="region" id="region-select" @change="filterRegion($event)">
                    <option value="" disabled selected hidden>Filter by Region</option>
                    <option v-for="(country,index) in region" :key="index" :value="region[index]">{{ country }}</option>
                </select>
            </div>
            <div class="grid">
                <div class="card" v-for="(country, index) in filteredCountries" :key="index">
                    <img :src="country.flag">

                    <div class="info">
                        <h1 class="name">
                            {{ country.name }}
                        </h1>
                        <p class="pop">
                            Population: {{ country.population }}
                        </p>
                        <p class="region">
                            Region: {{ country.region }}
                        </p>
                        <p class="capital">
                            Capital: {{ country.capital }}
                        </p>
                    </div>
                </div>
            </div>
        </div>
        
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'Home',
    data(){
        return{
            countries: [],
            region: ['Africa', 'Americas', 'Asia', 'Europe', 'Oceania'],
            search: '',
            theme: 'light'
        }
    },
    mounted () {
        axios
        .get('https://restcountries.eu/rest/v2/all')
        .then(response => (this.countries = response.data))
    },
    computed: {
        filteredCountries(){
            return this.countries.filter((country) => {
                const name = country.name.toLowerCase();
                const region = country.region.toLowerCase();
                const capital = country.capital.toLowerCase();

                const searchName = this.search.toLowerCase();

                return name.includes(searchName) || region.includes(searchName) || capital.includes(searchName);
            })
        },
        mode(){
            return this.theme
        }
    },
    methods:{
        filterRegion(region){
            axios.get('https://restcountries.eu/rest/v2/region/' + region.target.value)
            .then(response => (this.countries = response.data))
        },

        themeChange(){
            this.theme = this.theme == 'light' ? 'dark' : 'light';
        }
    }
}
</script>

<style scoped>
    .light{
        background: #eee;
    }

    .light .header{
        background: #fff;
    }

    .light .grid .card{
        background:  #fff;
        box-shadow: 0px 0px 10px rgb(170, 170, 170);
    }

    .light img{
        box-shadow: 0px 4px 5px rgb(170, 170, 170);
    }

    .dark{
        background: #0e0e10;

        color: #eee;
    }

    .dark .header{
        background: #212125;
    }

    .dark .grid .card{
        background: #212125;
        box-shadow: 0px 0px 10px rgb(78, 78, 78);
    }

    .dark img{
        box-shadow: 0px 0px 10px rgb(78, 78, 78);
    }

    .dark input,.dark select{
        background: #212125;
        color: #eee;
    }
    
    .header{
        height: 80px;
        width: 100%;
    }

    .content{
        display: flex;
        justify-content: space-between;
        align-items: center;

        width: 90%;
        height: 100%;
        margin: auto;
    }

    .mode{
        font-weight: 600;
        cursor: pointer;
    }

    h1.sitename{
        font-weight: 900;
    }

    h1{
        font-weight: 600;
    }

    .sitename{
        font-size: 20px;
    }

    .grid{
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        gap: 50px;
        grid-row: minmax(100px auto);

        width: 90%;
        margin: auto;

        padding: 50px 0;
    }

    .card{
        border-radius: 10px;
    }

    .filter{
        width: 90%;
        margin: auto;
        margin-top: 50px;

        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    
    .search{
        width: 20%;
        padding: 15px;
        border-radius: 10px;

        outline: none;
    }

    #region-select{
        outline: none;
        padding: 15px;

        border-radius: 10px;
        cursor: pointer;
    }

    .info{
        margin: 25px;
    }

    .name{
        height: 35px;
    }

    img{
        border-top-left-radius: 10px;
        border-top-right-radius: 10px;

        max-width: 100%;
        max-height: 100%;
        width: auto;
        height: auto;
    }
</style>