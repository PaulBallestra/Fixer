<template>
    <ion-page>
        <ion-header :translucent="true">
            <ion-toolbar>
                <ion-title class="ion-text-center"> Fixer App </ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content>
            <div id="container">

                <Search></Search>
                <Result style="display: block;"></Result>

            </div>
        </ion-content>
    </ion-page>
</template>

<script>
    import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';
    import { defineComponent } from 'vue';
    import Search from '../components/Search.vue';
    import Result from '../components/Result.vue';

    export default defineComponent({
        name: 'Home',
        data(){
            return{
                displayResult: 'none'
            }
        },
        components: {
            IonContent,
            IonHeader,
            IonPage,
            IonTitle,
            IonToolbar,
            Search,
            Result
        },
        mounted(){

        }
    });

</script>

<style scoped>
  #container {
    text-align: center;

    position: absolute;
    left: 0;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
  }

  #container strong {
    font-size: 20px;
    line-height: 26px;
  }

  #container p {
    font-size: 16px;
    line-height: 22px;

    color: #8c8c8c;

    margin: 0;
  }

  #container a {
    text-decoration: none;
  }
</style>