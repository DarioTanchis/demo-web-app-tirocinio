<template>
    <Listings v-bind:listingsData="this.listings"/>
    {{ console.log(this.jwt === undefined || this.jwt === '' ? "Not logged" : "Logged")  }}
</template>

<script>
    import Listings from "@/components/Listings"
    import { useUserStore } from "@/stores/user"

    export default{
        name: 'Home',
        components:{
            Listings,
        },
        data(){
            return{
                jwt:'',
                listings: []
            }
        },
        async created(){
            this.listings = await this.getListings();
        },
        methods:{
            async getListings(){
                const res = await fetch("http://localhost:1337/api/listings?populate=images")
                
                const response = await res.json();

                return response.data;
            },
        },
        mounted() {
            try{
                const userStore = useUserStore();

                this.jwt = userStore.jwt;
            }
            catch(err){
                
            }
        },
    }
</script>

<style>
.mainDiv{
    position: relative;
}
</style>