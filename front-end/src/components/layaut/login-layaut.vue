<template>
<main>
    <section class="material-half-bg">
      <div class="cover"></div>
    </section>
    <section class="login-content">
        <div class="logo">
            <h1 class="text-center">{{ nombre | uppercase}}</h1>
        </div>

        <router-view ></router-view>
    </section>
    </main>
</template>

<script>
  import axios from 'axios'
    export default {
        name:'login-layaut',
        components: {
          
        },
        data () {
            return {
               
            }
        },
        created()
        { 
           
        },
        computed:
        {
            settings()
            {
                return this.$store.getters.settings;
            },
            nombre()
            {
               
                    return this.$store.getters.settings.nombre;
                
            }
        },
        filters: 
        {
            uppercase(data) 
            {
                return data && data.toUpperCase();
            }
        }
        
    }
</script>

<style scope>
   .logo
   {
    margin-right: 20px;
    margin-right: 20px;
   }
</style>