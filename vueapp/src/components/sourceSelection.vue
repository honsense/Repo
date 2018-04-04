<template>
    <div class="sourceSelection">
        sourceSelection
        <select class="form-control" v-on:change="sourceChanged">
            <option v-bind:value="source.id" v-for="source in sources">{{source.name}}</option>
        </select>
    </div>
</template>

<script>
export default {
    name: 'sourceSelection',
    data() {
        return{
            sources: [],
            source: ''
        }
    },
    methods: {
        sourceChanged: function(e){
        for (var i=0; i<this.sources.length; i++) {
            if (this.sources[i].id == e.target.value){
                this.source = this.sources[i];
                }
            }
        }
    },
    created: function () {
        this.$http.get('http://newsapi.org/v1/sources?lang=en')
        .then(response => {
        this.sources = response.data.sources;
        });
    }
}
</script>

<style scoped>

</style>