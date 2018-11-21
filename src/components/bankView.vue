<template>
  <div>
    <md-app style="width:98%;margin:0 auto">
      <md-app-toolbar class="md-default">
        <span class="md-title">Bank Branches</span>
      </md-app-toolbar>
    </md-app>
    <div class="banks">
      <div class="bank-wrapper">
        <md-card md-with-hover>
          <div class="md-layout md-gutter">
            <div class="md-layout-item">
              <md-field>
                <label for="Cities">City</label>
                <md-select  name="city"  v-model="citySelect">
                  <md-option v-for="cityName in cities" :value="cityName" >{{cityName}}</md-option>
                </md-select>
              </md-field>
            </div>
            <div class="md-layout-item">
              <md-field md-clearable>
                <md-icon>search</md-icon>
                <md-input  v-model="searchValue" placeholder="Search"></md-input>
              </md-field>
            </div>
          </div>
        </md-card>
      </div>
      <div class="bank-table">
        <md-card md-with-hover>
          <md-table>
            <md-table-row>
              <md-table-head>IFSC</md-table-head>
              <md-table-head>Bank ID</md-table-head>
              <md-table-head>Name</md-table-head>
              <md-table-head>Branch</md-table-head>
              <md-table-head>Address</md-table-head>
              <md-table-head>City</md-table-head>
              <md-table-head>District</md-table-head>
              <md-table-head>State</md-table-head>
            </md-table-row>
            <md-table-row v-for="(bank,index) in details" :id="bank.ifsc">
              <md-table-cell>{{bank.ifsc}}</md-table-cell>
              <md-table-cell>{{bank.bank_id}}</md-table-cell>
              <md-table-cell>{{bank.bank_name}}</md-table-cell>
              <md-table-cell>{{bank.branch}}</md-table-cell>
              <md-table-cell>{{bank.address}}</md-table-cell>
              <md-table-cell>{{bank.city}}</md-table-cell>
              <md-table-cell>{{bank.district}}</md-table-cell>
              <md-table-cell>{{bank.state}}</md-table-cell>
            </md-table-row>
          </md-table>
        </md-card>
      </div>
      <div id="spinner" style="visibility:hidden">
        <md-progress-spinner :md-diameter="100"  :md-stroke="10" md-mode="indeterminate"></md-progress-spinner>
        <h3 style="color:white">Please Wait...</h3>
      </div>
    </div> 
  </div>
</template>

<script>
import Vue from 'vue'
import VueMaterial from 'vue-material'
import 'vue-material/dist/vue-material.min.css'

Vue.use(VueMaterial)

import axios from "axios";
export default {
  data () {
    return {
      searchValue:'',
      citySelect:'',
      cityUpper:'',
      cities:[
        'Bangalore',
        'Mumbai',
        'Chennai',
        'Delhi',
        'Hyderabad',
        'Mysore'
      ],
      details:[],
    }
  },
  methods:{
    
  },
  watch: {
    citySelect: function(){
      document.querySelector("#spinner").style.visibility="visible";
      this.details = [];
      this.cityUpper = this.citySelect.toUpperCase();
      axios.get(`https://vast-shore-74260.herokuapp.com/banks?city=${this.cityUpper}`)
      .then(response=>{
        this.details = response.data;
      })
    },
    searchValue: function(newVal,oldVal){
      newVal = newVal.toUpperCase();
      this.details.forEach((bank,index)=>{
        var sel = '#'+bank.ifsc;
        var ifsc = bank.ifsc;
        var name = bank.bank_name;
        var branch = bank.branch;
        var address = bank.address;
        var id = bank.bank_id;
        var city = bank.city;
        var state = bank.state;
        var district = bank.district;
        if(ifsc.indexOf(newVal)>-1){
          document.querySelector(sel).style.display="";
        }
        else if(name.indexOf(newVal)>-1){
          document.querySelector(sel).style.display="";
        }
        else if(branch.indexOf(newVal)>-1){
          document.querySelector(sel).style.display="";
        }
        else if(address.indexOf(newVal)>-1){
          document.querySelector(sel).style.display="";
        }
        else if(city.indexOf(newVal)>-1){
          document.querySelector(sel).style.display="";
        }
        else if(district.indexOf(newVal)>-1){
          document.querySelector(sel).style.display="";
        }
        else if(state.indexOf(newVal)>-1){
          document.querySelector(sel).style.display="";
        }
        else if(id==newVal)
          document.querySelector(sel).style.display="";
        else
          document.querySelector(sel).style.display="none";
      })
    },
    details: function(newVal,oldVal){
      if(newVal.length>5)
        document.querySelector("#spinner").style.visibility="hidden";
    }
  },
  computed:{
    
  }
}
</script>

<style scoped>

</style>
