<template>
    <div>
        <md-dialog :md-active="showReqform">
            <md-dialog-title>Request Info</md-dialog-title>
            <div>
                <md-progress-bar md-mode="indeterminate" v-show="progress"></md-progress-bar>
            </div>


            <md-tabs md-dynamic-height>
                <md-tab md-label="Request">
                <md-field>
                    <label>Reference</label>
                    <md-input :value="reference" @input="$emit('update:reference', $event)"></md-input>
                </md-field>
                <md-field>
                    <label>Requester</label>
                    <md-input :value="requester" @input="$emit('update:requester', $event)"></md-input>
                </md-field>
                <md-field>
                    <label>Comments</label>
                    <md-input :value="comments" @input="$emit('update:comments', $event)"></md-input>
                </md-field>
                </md-tab>
                <md-tab md-label="Observations">
                        <md-table :value="observations" md-card @md-selected="onSelect">
                        <!--<md-table-toolbar>
                        <h1 class="md-title"></h1>
                        </md-table-toolbar>-->
                        <md-table-row slot="md-table-row" slot-scope="{ item }" md-selectable="single">
                            <md-table-cell md-label="ID" md-sort-by="id" md-numeric>{{ item.Id }}</md-table-cell>
                            <md-table-cell md-label="Reference" md-sort-by="REFERENCE">{{ item.REFERENCE }}</md-table-cell>
                            <md-table-cell md-label="Action" md-sort-by="ACTION">{{ item.ACTION }}</md-table-cell>
                            <md-table-cell md-label="Response" md-sort-by="RESPONSE">{{ item.RESPONSE }}</md-table-cell>
                            <md-table-cell md-label="Reviewer" md-sort-by="REVIEWER">{{ item.REVIEWER }}</md-table-cell>
                        </md-table-row>
                        </md-table>
                </md-tab>

            </md-tabs>
            <md-dialog-actions>
                <md-button class="md-primary" @click="$emit('update:showReqform', false)">Close</md-button>
                <md-button class="md-primary" @click="postData">Save</md-button>
            </md-dialog-actions>
        </md-dialog>
    </div>
</template>

<script>
export default {
    name: 'reqview',
    data() {
        return{
            progress:false
        }
    },
    props: ['showReqform','reference', 'requester', 'comments', 'observations', 'Id'],
    methods: {
        postData: function(){
            this.progress=true;
            console.log(this.Id, this.reference, this.requester, this.comments);
            this.$http.post(
                "http://hon.local/insert.php", {'mode':"add",'reference':this.reference, 'requester':this.requester, 'comments':this.comments}
            )
            .then(
                status => {
                    if(status.data = 'Data Inserted...'){
                        this.$emit('update:showReqform', false);
                        this.$parent.refresh();
                    }
                this.progress=false;
            });
        },
        onSelect: function(){

        }
    },
}
</script>
