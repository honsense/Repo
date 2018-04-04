<template>
    <div id="app">
        <md-table v-model="sources" md-card md-sort="name" md-sort-order="asc" @md-selected="onSelect">
            <md-table-toolbar>
                <h1 class="md-title">Selection Colors</h1>
            </md-table-toolbar>
            <div>
                <md-progress-bar md-mode="indeterminate" v-show="progress"></md-progress-bar>
            </div>
            <md-table-row slot="md-table-row" slot-scope="{ item }" md-selectable="single">
                <!--<md-table-cell md-label="ID" md-sort-by="Id" md-numeric>{{ item.Id }}</md-table-cell>-->
                <md-table-cell md-label="Reference" md-sort-by="REFERENCE">{{ item.REFERENCE }}</md-table-cell>
                <md-table-cell md-label="Requester" md-sort-by="REQUESTER">{{ item.REQUESTER }}</md-table-cell>
                <md-table-cell md-label="Comments" md-sort-by="COMMENTS">{{ item.COMMENTS }}</md-table-cell>
                <md-table-cell md-label="Date Requested" md-sort-by="DATE_REQUESTED">{{ item.DATE_REQUESTED }}</md-table-cell>
            </md-table-row>
        </md-table>
        <RequestForm :showReqform.sync="showReqform" :Id="Id" :reference.sync="reference" :requester.sync="requester" :comments.sync="comments" :observations="observations"></RequestForm>
    </div>
</template>

<script>
import RequestForm from '../components/RequestForm'

export default {
    name: 'sourceSelection',
    components:{
        RequestForm
    },
    data() {
        return{
            sources: [],
            observations: [],
            source: '',
            showReqform: false,
            reference: '',
            requester: '',
            comments: '',
            Id: '',
            progress: true
        }
    },
    methods: {
        onSelect: function(item){
            this.showReqform = true;
            this.Id = item.Id;
            this.reference = item.REFERENCE;
            this.requester = item.REQUESTER;
            this.comments = item.COMMENTS;
        },
        refresh: function () {
            this.$http.get('http://hon.local/select.php')
            .then(response => {
            this.sources = response.body;
            });
            this.$http.get('http://hon.local/obsselect.php')
            .then(response => {
            this.observations = response.body;
            this.progress=false
            })
        }
    },
    created: function () {
        this.$http.get('http://hon.local/select.php')
        .then(response => {
        this.sources = response.body;
        });
        this.$http.get('http://hon.local/obsselect.php')
        .then(response => {
        this.observations = response.body;
        this.progress=false;
        })
    }
}
</script>

<style scoped>

        #app{
            margin-top: 60px;
            margin-left: 30px;
            margin-right: 30px;
            margin-bottom: 60px;
        }

</style>