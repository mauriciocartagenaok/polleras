<template>
	<main class="app-content">
		<app-title :title="[{title:'Aves',to:'resumen-aves'},{title:'Compra de aves'}]" :icon="'plus-square'"></app-title>
		<div class="row">
		<div class="col-md-2 col-sm-1"></div>
			<div class="col-md-8 col-sm-10">
	          <div class="tile">
	            <h3 class="tile-title">Compra de aves</h3>
	            <div class="tile-body">
	              <formulario :error="errores"  @submit.prevent="Enviar">
	              <div class="form-group">
	               	<select-galpon @change="galpon" :id_galpon="idgalpon"></select-galpon>
	               </div>
	                <div class="form-group">
	                  <label class="control-label">Cantidad </label>
	                  <div class="btn-group form-control form-group cantidad-max">
	                  <input class="form-control" v-model="aves.cantidad" name="cantidad" required type="text" placeholder="Cantidad">
	                   <button class="btn btn-primary btn-sm" @click.prevent="aves.cantidad=max_cantidad">Max</button>
	                   </div>
	                
	                </div>
	                 <div class="form-group">
	                  <label class="control-label">Costo por unidad </label>
	                  <input class="form-control" v-model="aves.costo" name="costo" required type="text" :placeholder="'Costo '+settings.moneda">
	                </div>
	                <div class="form-group">
	                  <label class="control-label">fecha</label>
	                  <input class="form-control" v-model="aves.fecha" name="fecha" required type="date" placeholder="fecha">
	                </div>
	               
	                <div class="form-group" >
	                  <label class="control-label">Detalles</label>
	                  <textarea class="form-control" v-model="aves.detalles" name="detalles" required placeholder="Detalles"></textarea>
	                  
	                </div>
	               
	                <div class="form-group">
	              <button class="btn btn-primary" type="submit"><i class="fa fa-fw fa-lg fa-check-circle"></i>Guardar</button>&nbsp;&nbsp;&nbsp;<button class="btn btn-secondary" type="button" @click.prevent="Cancelar"><i class="fa fa-fw fa-lg fa-times-circle"></i>Cancelar</button>
	            
	                </div>
	              </formulario>
	            </div>
	            
	          </div>
        	</div>
        	<div class="col-md-2 col-sm-1"></div>
		</div>
	</main>
</template>
<script>
import axios from 'axios';
import {fecha} from '@/assets/js/Date.js'
import select_galpon from '../galpones/select-galpon.vue'
	export default
	{
		name:'agregar-aves',
		props:['idgalpon'],
		components:{
          'select-galpon':select_galpon,
          
        },
		data()
		{
			
			return {
				max_cantidad:null,
				aves:
				{
					id_galpon:null,
					cantidad:null,
					costo:null,
					fecha:fecha(),
					//id_granjas:null,
					detalles:null,
					Submited:1
				},
				errores:{}
			
			

			}
		},
		created()
		{

			this.aves.id_galpon=this.idgalpon;
		},
		computed:
		{
			settings()
            {
                return this.$store.getters.settings;
            }
		},
		methods:
		{
			galpon(id_galpon,galpon)
			{
				this.max_cantidad=galpon.capacidad-galpon.aves;
				//console.log(this.max_cantidad)
				this.aves.id_galpon=id_galpon;
			},
			Cancelar()
			{
				
			},
			
			Enviar()
			{
				 this.$store.commit('loading',true);
				axios.post('/api/aves/insertar',this.aves)
                .then(request => 
                {
                	 this.$store.commit('loading',false);
                    if(request.data.insert)
                    {
                        //swal("Listo!", "El proyecto se ha almacenado ", "success");
                        swal(
                        {
                            title: "Listo!",
                            text: "se ha agregrado la compra de aves ",
                            type: "success",

                        },
                        ()=>this.$router.push({name:'resumen-aves'}));
                             
                    }else
                    {
                       this.errores=request.data.error;
                    }  
                }).catch(AxiosCatch);
			}
		}
	}

</script>