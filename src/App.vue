<template>
    <label for="maxCP" class="max-cp">
        <input type="checkbox" id="maxCP" />
        <small>
            Maximum Combat Points
        </small>
    </label>
    <input type="text" class="input" placeholder="Pokemon or type" @input="change" v-model="search" />
    <div class="loader" v-bind:style="{ display: loader_display }"></div>
    <ul class="suggestions">

        
        <div v-for="poke in resultsJSON" v-bind:key="poke">
            
            <li>
                <img
                    v-bind:src="poke.img"
                />
                <div class="info">
                    <h1>{{poke.Name}}</h1>
                    <span v-for="type in poke.Types" v-bind:key="type">
                        <span :class="['type ' + type.toLowerCase()]">{{type}}</span>
                    </span>
                </div>

            </li>
        </div>
        
        
        
        <li>
            <img
                src="http://assets.pokemon.com/assets/cms2/img/pokedex/full/025.png"
                alt="Pikachu"
            />
            <div class="info">
                <h1><span class="hl">Pika</span>chu</h1>
                <span class="type electric">Electric</span>
                <span class="type normal">Normal</span>
            </div>
        </li>
        <li>
            <img
                src="https://cyndiquil721.files.wordpress.com/2014/02/missingno.png"
                alt="No results"
            />
            <div class="info">
                <h1 class="no-results">
                    No results
                </h1>
            </div>
        </li>
    </ul>
</template>

<script>
// eslint-disable-next-line no-unused-vars
const URL_PATH =
    'https://gist.githubusercontent.com/bar0191/fae6084225b608f25e98b733864a102b/raw/dea83ea9cf4a8a6022bfc89a8ae8df5ab05b6dcc/pokemon.json';

export default {
    name: 'App',
    
    data () {
        return {
            search: '',
            resultsJSON : {},
            loader_display: 'none'
        }
    }
    ,
    methods: {
        change(){
            
            // Loader ON
            this.loader_display = 'block'

            fetch(URL_PATH).then(response => response.json()).then(
                json =>{

                    // Find by name and by type
                    let by_name = json.filter(poke => poke.Name.toLowerCase().includes(this.search.toLowerCase()));
                    let by_type = json.filter(poke => poke.Types.findIndex(e => e.toLowerCase().includes(this.search.toLowerCase()))!=-1);
                    
                    // Sort by_name
                    by_name = by_name.sort(this.sortByProp('Name'));

                    // Join name and type results
                    let results = by_name.concat(by_type);

                    //  Use the first 4 results
                    this.resultsJSON = results.slice(0,4);
                    
                    console.log(this.resultsJSON);   
                    
                    // Loader OFF
                    this.loader_display = 'none'
                        
                }
            )
        },
        sortByProp(prop){  
            return function(a,b){  
                if(a[prop] > b[prop])  
                    return 1;  
                else if(a[prop] < b[prop])  
                    return -1;  
            
                return 0;  
            }  
        }
    },
    computed: {
        filteredList() {

            return this.database.filter(pokemon => {
                return pokemon.name.toLowerCase().includes(this.search.toLowerCase())
            })
        },

        test(){
            return 'test'
        }
    },
    created() {
        
    }
    
    
};
</script>
