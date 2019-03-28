<template>
  <div class="SearchPanel">
    <b-card 
      v-bind:class="{ 'invisible' : selected === false }"
      sub-title="Selected"
    >
      <b-card-text>{{ selected }}</b-card-text>

    </b-card>

    <b-form-input 
      v-model="searchstring" 
      type="text" 
      placeholder="Search for an animal..."
      @keyup.native="sendSearch" 
    />
    <b-list-group >
      <b-list-group-item 
        v-for="result in results" 
        :key="result.id"
        v-on:click="selectSearchItem(result)"
      >
        <p><strong>{{ result.fullname}}</strong></p>
        <p>{{ result.description }}</p>
      </b-list-group-item>

    </b-list-group>
  </div>
</template>

<script>
export default {
  name: 'SearchPanel',
  props: {
    uri: String,
    side : String
  },
  methods : {
    sendSearch : function() {
      if (this.socket.readyState === 1) {
        this.socket.send(this.searchstring);
      }
    },
    selectSearchItem : function(result) {
      this.selected = result.fullname; 
      this.searchstring = ''; 
      this.results = [];

      this.$root.$emit('search-select',this.side,result.fullname);
    }
  },
  created : function() {
    this.socket = new WebSocket(this.uri);
    this.socket.onopen = () => {
      this.status = "connected";
      this.socket.onmessage = ({data}) => {
        this.results = JSON.parse(data);
      };
      this.socket.onclose = () => {
        this.status = 'disconnected';
      }
    }
  },
  data : function() {
    return {
      searchstring : "",
      results: [],
      status : 'disconnected',
      selected : false
    };
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  margin-bottom: 1em;
}
.card-body {
  padding: 0.5em 1em;
}
.card-subtitle {
  font-size: 0.8em;
}
.list-group {
  margin-top: 1em;
}
li {
  cursor: pointer;
}
</style>
