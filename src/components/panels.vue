<template>
  <div class="hello">
    
    <b-container>
      <b-row>
        <b-col>
          <b-card  v-bind:class="{ 'invisible' : bioconnection === false }"> 
            <h4><em>{{ searchResults.search.left }}</em> is in the same <strong>{{searchResults.taxonomicCategory}}</strong> (<em>{{searchResults.taxonomicValue}}</em>) as <em>{{ searchResults.search.right }}</em></h4>
          </b-card>
        </b-col>
      </b-row>
      <b-row>
        <b-col><SearchPanel uri="ws://localhost:3333/search" side="left"></SearchPanel></b-col>
        <b-col><SearchPanel uri="ws://localhost:3333/search" side="right"></SearchPanel></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import SearchPanel from './search.vue'

export default {
  name: 'Panels',
  props: {
    uri : String
  },
  components: {
    SearchPanel
  },
  data : function() {
    return {
      search : {
        left : false,
        right : false
      },
      connected : false,
      bioconnection : false,
      searchResults : {
        search : {}
      }
    }
  },
  created : function() {
    this.$root.$on('search-select', (side,animalName) => {
      this.search[side] = animalName;
      if (
        (this.search.left !== false) && 
        (this.search.right !== false) && 
        (this.status === "connected") &&
        (this.socket.readyState === 1)) {
          this.socket.send(JSON.stringify(this.search));
        } else {
          console.log('could not send');
        }
      }
    );
    this.socket = new WebSocket(this.uri);
    this.socket.onopen = () => {
      this.status = "connected";
      this.socket.onmessage = ({data}) => {
        this.bioconnection = true;
        this.searchResults = JSON.parse(data);
      };
      this.socket.onclose = () => {
        this.status = 'disconnected';
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .card {
    text-align: center;
    margin-bottom: 1em;
    margin-top: 1em;
  }
</style>
