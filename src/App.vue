<template>
  <div id="App"
    :class="[
      2<=now.getMonth()&&now.getMonth()<5 ? 'spr' : 
      5<=now.getMonth()&&now.getMonth()<8 ? 'smm' : 
      8<=now.getMonth()&&now.getMonth()<11 ? 'atm' : 'wnt']">
    <LoaderLayer v-if="loaderFlg"></LoaderLayer>
  </div>
</template>

<script>
import Dexie from 'dexie'
import LoaderLayer from './components/LoaderLayer'

export default {
  name: '#App',
  data() {
    return {
      loaderFlg: true,
      db: "",
      dbName: "tablebookDB",
      now: "",
    }
  },
  components: {
    LoaderLayer,
  },
  created: function(){
    this.now = new Date();
    this.hiddenLoader();
  },
  methods: {
    shownLoader() {
      this.loaderFlg = true;
    },
    hiddenLoader() {
      this.loaderFlg = false;
    },
    createDB() {
			this.db = new Dexie(this.dbName);
      this.createTable();
		},
		createTable() {
			this.db.version(1).stores({
				clmidx: "++cid, *clms, *clmspan",
        rowline: "++rid",
      });
      // clmidx: cid, clms:{clm*(*=number): name}, clmspan{name: [clm*, clm*, clm*]},
      // rowline: rid, clm*, clm*, clm*, ...
      // this.testAddDB();
    },
  },
}
</script>

<style>
@import "./css/app.css";
</style>
