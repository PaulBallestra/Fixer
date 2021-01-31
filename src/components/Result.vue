<template>

    <!-- DIV result en fonction de la valeur de la devise -->
    <div class="w-75 ml-auto mr-auto">

        <ion-card class="p-2">

            <ion-card-title v-if="this.currencyName != null"> Euros Into {{currencyName}} </ion-card-title>
            <ion-card-title v-else> Euros Into ? </ion-card-title>

            <ion-card-title class="m-2" v-if="!isNaN(this.result)"> <strong> {{result}} {{currencySymbol}} </strong> </ion-card-title>
            <ion-card-title class="m-2" v-else> <strong> No currency </strong> </ion-card-title>

            <ion-card-subtitle> The other way is impossible because of the free API :) </ion-card-subtitle>

        </ion-card>

    </div>

</template>

<script>

    import { IonCard, IonCardTitle, IonCardSubtitle } from '@ionic/vue';
    import { defineComponent } from 'vue';
    import { axios } from '@/plugins/axios'

    export default defineComponent({

        name: 'Result',
        data(){
            return{
                isActive: false,
                result: null,
                currencyValue: null,
                currencyName: null,
                currencySymbol: null,
                amount: null,
            }
        },
        components: {
            IonCard,
            IonCardTitle,
            IonCardSubtitle,
        },
        mounted() {
            this.bus.on("displayResult", calcul => {

                this.isActive = true

                this.currencyValue = calcul.currency
                this.amount = calcul.amount

                this.result = (this.amount * this.currencyValue).toFixed(2); //2 décimales

                axios.get(`http://data.fixer.io/api/latest?access_key=`+process.env.VUE_APP_FIXER_API_KEY).then((response) => {
                    for (const [key, value] of Object.entries(response.data.rates)){
                        if(value === this.currencyValue){
                            //On chope le nom du symbols voulu par l'user
                            axios.get(`http://data.fixer.io/api/symbols?access_key=`+process.env.VUE_APP_FIXER_API_KEY).then((response) => {
                                for (const [key2, value2] of Object.entries(response.data.symbols)) {
                                    if(key2 === key){
                                        this.currencyName = value2
                                        this.currencySymbol = key2
                                    }
                                }
                            }).catch((error) => {
                                console.log(error)
                            });
                        }
                    }
                }).catch((error) => {
                    console.log(error)
                });
            });
        }
    });

</script>