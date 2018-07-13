<template>
<!--Header Start-->
<div id="performer-list" class="performer-list">
 <div class="performer-header">
<!--Search filter starts -->
  <div class="search-wrapper">
    <label>Search title:</label>
    <input type="text" v-model="search" placeholder="Search title.."/>
             <input type="button" value="Clear Search"  @click="clearSearch">
  </div>
  </div>
  <!--Header End-->
 <label>Sort by:</label>
    <div class="btn-group" data-toggle="buttons" >
        <label class="colbtn btn-outline-dark">
            <input type="radio" name="grp" value="Code" v-model="checkedCol" @click="sortObject"   checked>Code
        </label>
        <label class="colbtn btn-outline-dark ">
            <input type="radio" name="grp" value="FirstName" v-model="checkedCol" @click="sortObject">FirstName
        </label>
        <label class="colbtn btn-outline-dark">
            <input type="radio" name="grp" value="LastName" v-model="checkedCol"  @click="sortObject">LastName
        </label>
        <label class="colbtn btn-outline-dark">
            <input type="radio" name="grp" value="Date Of Birth" v-model="checkedCol"  @click="sortObject">Date Of Birth
        </label>
        <br>
         <label for="item.checkedCol">Actve Sort: {{checkedCol}}</label>
        <br>
        <input type="button" value="Clear Active Sort"  @click="clearSort">
    </div>
<br>
<label class="btn btn-outline-dark ">
            <input type="checkbox" name="selectAll"  v-model="selectmeArray" @click="selectAll">Select All Records
        </label>
  <div class="wrapper">
  <!--Listing starts-->
  <div v-if="checkedCol">
      <div class="performer-item" v-for="(item) in ordered"  v-bind:key="item.Code">
                 <!--MAKE AGE a NUMBER FIELD-->
       <input type="checkbox" name="select-me"  v-model="selectmeArray"  @click="postPost">
        <input type="number" v-model="item.Code" text="item.Code" value="item.Code" @click="postPost">
        <label for="item.FirstName">{{item.FirstName}}</label>
        <label for="item.LastNamee">{{item.LastName}}</label>
        <label for="item.DateOfBirth">{{item.DateOfBirth}}</label>
    </div>
</div>
<div v-else>
    <div class="performer-item" v-for="(item) in filteredList"  v-bind:key="item.Code">
                <!--MAKE AGE a NUMBER FIELD-->
       <input type="checkbox" name="select-me"  v-model="selectmeArray"  @click="postPost">
        <input type="number" v-model="item.Code" text="item.Code" value="item.Code" @click="postPost">
        <label for="item.FirstName">{{item.FirstName}}</label>
        <label for="item.LastNamee">{{item.LastName}}</label>
        <label for="item.DateOfBirth">{{item.DateOfBirth}}</label>
  </div>
  </div>
  <infinite-loading force-use-infinite-wrapper="true" @infinite="infiniteHandler" ref="infiniteLoading">
     <span slot="no-more">
        Listing completed.
    </span>
  </infinite-loading>
  </div>
</div>
</template>

<script>
//import libraries from node_modules etc
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
import ClientTable from 'vue-tables-2';
import InfiniteLoading from 'vue-infinite-loading';
Vue.use(VueAxios, axios)
Vue.use(ClientTable);
import VueLodash from 'vue-lodash'
const options = { name: 'lodash' } // customize the way you want to call it
Vue.use(VueLodash, options) // options is optional
Object.defineProperty(Vue.prototype, '$lodash', { value: lodash });
import lodash from 'lodash';   
import _ from 'lodash'; 

const dataApi = 'http://api.grw.local/performance_data.json';

export default {
  data() {
      return {
        checkedCol: '',
        columns: ['Code', 'FirstName', 'LastName','DateOfBirth'],
        tableData: [],
        errors : [],
       selectmeArray: [],
       selectAllRecords:'',
        orderedList: [],
        sortKey: '',
        reverse: false,
        search: '',
      }},
 
 computed: {
   //for filtering our list (returns data objects)
    filteredList () {
      return this.tableData.filter(tableData => {
        return tableData.FirstName.toLowerCase().includes(this.search.toLowerCase())
      })
    },
    /*for ordering the JSON data - I use LODASh here 
    ..like the custom dynamic column(object attribute )
     name to order on
     */
    ordered () {
      return _.orderBy(this.filteredList, this.checkedCol)
    }
  },
  methods: {
    sortObject: function() {
      var key, a = [];
      for (key in this.tableData) {
        if (this.tableData.hasOwnProperty(key)) {
            a.push(key);
        }
      }
      a.sort();
      for (key = 0; key < a.length; key++) {
        this.orderedList[a[key]] = this.tableData[a[key]];
      }
      return this.orderedList;
    },
    sortBy: function(checkedCol) {
      this.reverse = (this.checkedCol == checkedCol) ? ! this.reverse : false;
      this.checkedCol = checkedCol;
    },
    
    clearSort: function() {
      this.checkedCol = '';
    },
    selectAll: function() {
      if (this.selectAllRecords) {
       
       this.selectmeArray= '';

      }
    },
    clearSearch: function() {
      this.search = '';
    },
    postPost() { // Pushes posts to the server when called.
    //I have implemented CORS allow * in the API/V1 web.config file but this issue persists
      axios.post(dataApi, {
        body: this.tableData,
    })
    .then(data => {
      this.tableData = this.tableData.concat(data.hits);
      })
    .catch(e => {
      this.errors.push(e)
    })
  },
//Handle the infinite scrolling
  infiniteHandler($state) {
      axios.get(dataApi, {
        params: {
          page: this.tableData.length / 20 + 1,
        },
      }).then(({ data }) => {
        if (data.hits.length) {
          this.tableData = this.tableData.concat(data.hits);
          $state.loaded();
          if (this.filteredList.length / 20 === 10) {
            $state.complete();
          }
        } else {
          $state.complete();
        }
    });
  },
  changeFilter() {
    this.tableData = [];
    this.$nextTick(() => {
      this.$refs.infiniteLoading.$emit('$InfiniteLoading:reset');
    });
  }},
  components: {
    InfiniteLoading
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
item {
  box-sizing: border-box;
  background-color: var(--serenity__2016);
  padding: 1rem;
  margin-bottom: 2px;
  color: #fff;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
html, body {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin-top: 16px;
  margin-bottom: 16px;
}

div#app {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  
  .search-wrapper {
    position: relative;
    label {
      position: absolute;
      font-size: 12px;
      color: rgba(0,0,0,.50);
      top: 8px;
      left: 12px;
      z-index: -1;
      transition: .15s all ease-in-out;
    }
    input {
      padding: 4px 12px;
      color: rgba(0,0,0,.70);
      border: 1px solid rgba(0,0,0,.12);
      transition: .15s all ease-in-out;
      background: white;
      &:focus {
        outline: none;
        transform: scale(1.05);
        & + label  {
          font-size: 10px;
          transform: translateY(-24px) translateX(-12px);
        }
      }
      &::-webkit-input-placeholder {
          font-size: 12px;
          color: rgba(0,0,0,.50);
          font-weight: 100;
      }
    }
  }

  .wrapper {
    display: flex;
    max-width: 444px;
    flex-wrap: wrap;
    padding-top: 12px;
    
  }

  .performer-item {
    box-shadow: rgba(0, 0, 0, 0.117647) 0px 1px 6px, rgba(0, 0, 0, 0.117647) 0px 1px 4px;
    max-width: 124px;
    margin: 12px;
    transition: .15s all ease-in-out;
    &:hover {
      transform: scale(1.1);
    }
    a {
      text-decoration: none;
      padding: 12px;
      color: #03A9F4;
      font-size: 24px;
      display: flex;
      flex-direction: column;
      align-items: center;
      img {
        height: 100px;
      }
      small {
        font-size: 10px;
        padding: 4px;
      }
    }
  }
}
.performer-item {
    border: 1px solid #88DFE7;
    border-radius: 5px;
   
}

.performer-item > div {
    border: 1px solid #88DFE7;
    border-radius: 5px;
    
    padding: 1em;
    color: #d9480f;
}
.performer-item {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
}

</style>
