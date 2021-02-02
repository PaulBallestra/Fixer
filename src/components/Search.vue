<template>

        <div class="w-75 ml-auto mr-auto">

            <!-- Formulaire Search -->
            <form>
                <p class="text-center mb-2"><strong> Convert Euros into ?</strong></p>
                <ion-item>
                    <ion-label> Euros </ion-label>
                    <ion-input placeholder="50.00" class="ion-text-center" id="inputValueToConvert" inputmode="numeric" type="number"></ion-input>
                </ion-item>
                <ion-item class="mt-2">
                    <ion-label>Currency</ion-label>
                    <ion-select ok-text="OK" cancel-text="Cancel" id="selectedCurrency" placeholder="USD">
                        <ion-select-option v-for="(currency, name) in currencies" :key="currency" :value="currency">{{name}}</ion-select-option>
                    </ion-select>
                </ion-item>
                <ion-button expand="block" v-on:click="convertEurosIntoDevise" class="mt-2"> 💸 Convert 💰</ion-button>
            </form>

        </div>

</template>

<script>
    import { IonInput, IonItem, IonLabel, IonButton } from '@ionic/vue';
    import { defineComponent } from 'vue';
    import { axios } from '@/plugins/axios'

    export default defineComponent({

        name: 'Search',
        data(){
            return{
                currencies: null
            }
        },
        components: {
            IonInput,
            IonItem,
            IonButton,
            IonLabel,
        },
        methods: {

            convertEurosIntoDevise: async function(e){

                let amountToConvert = document.getElementById('inputValueToConvert').value
                let selectedCurrency = document.getElementById('selectedCurrency').value

                e.preventDefault()

                this.bus.emit("displayResult", {'amount': amountToConvert, 'currency': selectedCurrency}); //Appel de la fonction pour relier le composant result a celui ci

            }

        },
        mounted(){

            axios.get(`http://data.fixer.io/api/latest?access_key=`+process.env.VUE_APP_FIXER_API_KEY).then((response) => {

                this.currencies = response.data.rates //on ajoute dans le tableaux toutes les valeurs mais que leur noms

            }).catch(function(error){

                console.log(error)

            });
        }
    });
</script>