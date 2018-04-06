<template>
    <div>
        <md-dialog :md-active="showObsform" class="Obsform">
            <md-dialog-title>Request Info</md-dialog-title>
            <div>
                <md-progress-bar md-mode="indeterminate" v-show="progress"></md-progress-bar>
            </div>

            <form nonvalidate @submit.prevent="validateUser">
                <md-card >
                    <md-field :class="getValidationClass('reference')">
                        <label for="reference">Reference</label>
                        <md-input name="reference" id="reference" v-model="form.reference"></md-input>
                    </md-field>
                    <md-field :class="getValidationClass('requester')">
                        <label for="requester">Requester</label>
                        <md-input name="requester" id="requester" v-model="form.requester"></md-input>
                    </md-field>
                    <md-field :class="getValidationClass('observation')">
                        <label for="observation">Observation</label>
                        <md-textarea name="observation" id="observation" v-model="form.observation" md-autogrow></md-textarea>
                    </md-field>
                    <md-field :class="getValidationClass('action')">
                        <label for="action">Action</label>
                        <md-textarea name="action" id="action" v-model="form.action" @input="getValidationClass('action')" md-autogrow></md-textarea><!--resulme here-->
                        <span class="md-error" v-if="!$v.form.action.required"></span>
                    </md-field>

                    <md-card-actions>
                        <md-button type="submit" class="md-primary">Submit</md-button>
                    </md-card-actions>
                </md-card>
            </form>

<!--            <md-dialog-actions>
                <md-button class="md-primary" @click="closeForm">Close</md-button>
                <md-button type="submit" class="md-primary" @click="postData">Save</md-button>
            </md-dialog-actions>
-->
        </md-dialog>
    </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import {
    required
} from 'vuelidate/lib/validators'
export default {
    name: 'obsview',
    mixins: [validationMixin],
    data() {
        return {
            form: {
                reference: null,
                requester: null,
                observation: null,
                action: null
            },
            progress:false
        }
    },

    props: ['showObsform','reference', 'requester', 'comments', 'observations', 'Id', 'mode'],
    validations: {
        form: {
            reference: {
                required
            },
            requester: {
                required
            },
            observation: {
                required
            },
            action: {
                required
            }
        },
        sending: false
    },
    methods: {
        getValidationClass (fieldName) {
            const field = this.$v.form[fieldName]

            if (field) {
                return {
                'md-invalid': field.$invalid && field.$dirty
                }
            }
        },
        saveUser() {
            this.sending = true
        },
        validateUser() {
            this.$v.$touch()

            if (!this.$v.$invalid) {
                this.saveUser()
            }
        },
        test: function(){
            if(this.$v.form[action].$dirty) {
                console.log('dirt');
            }
            else {
                console.log('no dirt');
            }
        },
        closeForm: function (){
            this.$emit('update:showObsform', false);
            this.$emit('update:showReqform', true);
        },
        postData: function() {
            ///
        }
    }
}
</script>

<style scoped>

        .Obsform{
            padding:20px;
        }
</style>