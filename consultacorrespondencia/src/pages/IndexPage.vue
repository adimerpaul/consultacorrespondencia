<template>
  <q-page class="">
    <div class="row">
      <div class="col-12">
        <q-form @submit.prevent="buscar">
          <div class="row">
            <div class="col-12 col-md-10 q-pa-xs">
<!--              <q-select use-input label="Ingresar cite/codigo" v-model="codigo" :options="mails" outlined @filter="filterFn"/>-->
              <q-input  outlined dense label="Ingrese CITE/ Codigo  /Tramite" v-model="codigo"/>
            </div>
            <div class="col-12 col-md-2 q-pa-xs flex flex-center">
              <q-btn label="Buscar" color="primary" icon="send" type="submit" />
            </div>
          </div>
        </q-form>
      </div>
      <div class="col-12 col-md-12">
        <q-banner dense class="bg-primary text-white">
          Datos de correspondencia
        </q-banner>
        <q-form>
          <div class="row">
            <div class="col-3 q-pa-xs"><q-input dense label="tipo" v-model="email.tipo" outlined/></div>
            <div class="col-3 q-pa-xs"><q-input dense label="ref" v-model="email.ref" outlined/></div>
            <div class="col-3 q-pa-xs"><q-input dense label="remitente" v-model="email.remitente" outlined/></div>
            <div class="col-3 q-pa-xs"><q-input dense label="cargo" v-model="email.cargo" outlined/></div>
            <div class="col-3 q-pa-xs"><q-input dense label="institucion" v-model="email.institucion" outlined/></div>
            <div class="col-3 q-pa-xs"><q-input dense label="fecha" v-model="email.fecha" outlined/></div>
            <div class="col-3 q-pa-xs"><q-input dense label="Fojas" v-model="email.folio" outlined/></div>
            <div class="col-3 q-pa-xs"><q-input dense label="estado" v-model="email.estado" outlined/></div>
            <div class="col-12 q-pa-xs"><q-btn type="a" :href="url+'/../imagenes/'+email.archivo" target="__blank" v-if="email.archivo!='' && email.archivo!=undefined" class="full-width" color="primary" icon="file_download"  :label="email.archivo" outlined/></div>
          </div>
        </q-form>
      </div>
      <div class="col-12 col-md-12">
        <q-banner dense class="bg-amber text-white">
          Datos de historial
        </q-banner>
        <q-table dense :columns="columns" :rows="email.logs">
          <template v-slot:body-cell-de="props">
            <q-td :props="props">
              <div class="text-caption"  v-if="props.row.user!=undefined">{{ props.row.user.name}}</div >
            </q-td>
          </template>
          <template v-slot:body-cell-a="props">
            <q-td :props="props">
              <div class="text-caption" >{{ props.row.user2.name}}</div >
            </q-td>
          </template>
          <template v-slot:body-cell-unidad="props">
            <q-td :props="props" >
              <div class="row">
                <div class="col-12">
<!--                  <q-input dense autogrow outlined type="textarea" :model-value="props.row.user2.unit.nombre" />-->
                  {{ props.row.user2.unit.nombre }}
                </div>
              </div>
            </q-td>
          </template>
          <template v-slot:body-cell-accion="props">
            <q-td :props="props" >
              <div class="row">
                <div class="col-12">
<!--                  <q-input dense autogrow outlined type="textarea" :model-value="props.row.accion" />-->
                  {{props.row.accion}} {{props.row.archivo}}
                </div>
              </div>
            </q-td>
          </template>
          <template v-slot:body-cell-estado="props">
            <q-td :props="props" >
              <q-badge :color="props.row.estado=='REMITIDO'||props.row.estado=='ARCHIVADO'?'positive':'negative'">{{props.row.estado}}</q-badge>
            </q-td>
          </template>
        </q-table>
        <!--      <table class="full-width">-->
        <!--        <tr>-->
        <!--          <th>#</th>-->
        <!--          <th>A</th>-->
        <!--          <th>DE</th>-->
        <!--          <th>Unidad</th>-->
        <!--          <th>Accion</th>-->
        <!--          <th>Estado</th>-->
        <!--        </tr>-->
        <!--        <tr v-for="(l,index) in email.logs" :key="l.id">-->
        <!--          <td>{{index+1}}</td>-->
        <!--          <td>{{l.user2.name}}</td>-->
        <!--          <td v-if="l.user!=undefined">{{l.user.name}}</td>-->
        <!--          <td v-else></td>-->
        <!--          <td>{{l.user2.unit.nombre}}</td>-->
        <!--          <td>{{l.accion}}</td>-->
        <!--          <td><q-badge :color="l.estado=='REMITIDO'||l.estado=='ARCHIVADO'?'positive':'negative'">{{l.estado}}</q-badge></td>-->
        <!--        </tr>-->
        <!--      </table>-->
        <!--      <pre>{{email.logs}}</pre>-->
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      columns:[
        {label:'de',field:'de',name:'de',align:'left'},
        {label:'a',field:'a',name:'a',align:'left'},
        {label:'unidad',field:'unidad',name:'unidad',align:'left'},
        {label:'accion',field:'accion',name:'accion'},
        {label:'estado',field:'estado',name:'estado'},
      ],
      url:process.env.API,
      codigo:'',
      email:{},
      mails:[],
      mails2:[],
    }
  },
  created() {
    // this.$q.loading.show()
    // this.$axios.get(process.env.API+'/todos').then(res=>{
    //   this.mails=[{label:process.env.API}]
    //   res.data.forEach(r=>{
    //     // console.log(r)
    //     r.label=r.codigo+' '+r.citecontrol+' '+r.remitente+' '+r.ref
    //     this.mails.push(r)
    //     // this.mails2.push(r)
    //   })
    //   this.mails2=this.mails
    //   this.codigo=this.mails[0]
    //   this.$q.loading.hide()
    // }).catch(err=>{
    //   this.$q.loading.hide()
    //   this.$q.notify({
    //     message:err.response.data.message,
    //     color:'red',
    //     icon:'error'
    //   })
    // })
  },
  methods:{
    buscar(){
      this.$q.loading.show()
      this.email={}
      // console.log(this.codigo)
      this.$axios.post(process.env.API+'/buscar',{codigo:this.codigo}).then(res=>{
        console.log(res.data)
        if (res.data.length>0)
          this.email=res.data[0]
        this.$q.loading.hide()
      })
        .catch(err=>{
          this.$q.loading.hide()
          console.error(err.response.data.message)
      })
    },
    filterFn (val, update) {
      if (val === '') {
        update(() => {
          this.mails = this.mails2
          // here you have access to "ref" which
          // is the Vue reference of the QSelect
        })
        return
      }
      update(() => {
        const needle = val.toLowerCase()
        this.mails = this.mails2.filter(v => v.label.toLowerCase().indexOf(needle) > -1)
      })
    }
  }
}
</script>
