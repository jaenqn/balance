<template>
  <div>

    <div class="row">
      <div class="col-sm-12 col-md-12">
        <div class="panel panel-default">
          <div class="panel-heading">Gestion clientes
            <!-- <div class="panel-action">
              <a href="#" data-perform="panel-collapse"><i class="ti-minus"></i></a>
              <a href="#" data-perform="panel-dismiss"><i class="ti-close"></i></a>
            </div> -->
          </div>
          <div class="panel-wrapper collapse in">
            <div class="panel-body">
              <form  id="frmRegistrarClientes" class="form-horizontal" role="form">
                <div class="col-sm-6">
                  <div class="form-group">
                    <label for="txtNroRuc" class="col-sm-3 control-label">R.U.C</label>
                    <div class="col-sm-9">
                      <input type="text" class="form-control" v-model="txtNroRuc" value="" id="txtNroRuc" placeholder="00000000000" >
                    </div>
                  </div>
                </div>
                <div class="col-sm-6">
                  <div class="form-group">
                    <label for="txtRazonSocial" class="col-sm-3 control-label">Razón social</label>
                    <div class="col-sm-9">
                      <input type="text" class="form-control" v-model="txtRazonSocial" value="" id="txtRazonSocial" placeholder="Nombre de empresa o cliente">
                    </div>
                  </div>
                </div>
                <div class="col-sm-6">
                  <div class="form-group">
                    <label for="txtDireccion" class="col-sm-3 control-label">Dirección</label>
                    <div class="col-sm-9">
                      <input type="text" class="form-control" v-model="txtDireccion" value="" id="txtDireccion" placeholder="Ingresar texto">
                    </div>
                  </div>
                </div>
                <div class="col-sm-6">

                  <div class="form-group">
                    <label for="txtTelefono" class="col-sm-3 control-label">Teléfono</label>
                    <div class="col-sm-9">
                      <input type="text" class="form-control" v-model="txtTelefono" value="" id="txtTelefono" placeholder="Teléfono">
                    </div>
                  </div>
                </div>
                <div class="col-sm-6">

                  <div class="form-group">
                    <label for="txtCelular" class="col-sm-3 control-label">Celular</label>
                    <div class="col-sm-9">
                      <input type="phone" class="form-control" v-model="txtCelular" value="" id="txtCelular" placeholder="Celular">
                    </div>
                  </div>
                </div>
                <div class="col-sm-6">

                  <div class="form-group">
                    <label for="txtCorreo" class="col-sm-3 control-label">Correo</label>
                    <div class="col-sm-9">
                      <input type="email" class="form-control" v-model="txtCorreo" value="" id="txtCorreo" placeholder="example@correo.com">
                    </div>
                  </div>
                </div>
                <div class="col-sm-6">

                  <div class="form-group">
                    <label for="txtReferente" class="col-sm-3 control-label">Responsable</label>
                    <div class="col-sm-9">
                      <input type="text" class="form-control" v-model="txtReferente" value="" id="txtReferente" placeholder="Responsable">
                    </div>
                  </div>
                </div>
                <div class="clearfix"></div>
                <div class="col-sm-6">

                  <div class="form-group">
                    <div class="col-sm-9 col-sm-offset-3">
                      <button type="submit" class="btn btn-primary"  @click.prevent="fnOnClick_btnGuardar" id="btnGuardar">Registrar</button>
                      <button type="button" class="btn btn-danger" :class="{ hidden: !isEdit }" @click="fnOnClick_btnCancelar" id="btnCancelar">Cancelar</button>

                    </div>
                  </div>
                </div>
              </form>

            </div>
            <!-- <div class="panel-footer"> Panel Footer </div> -->
          </div>
        </div>

      </div>
    </div>
    <div class="row">
      <div class="col-sm-12 col-md-12">
        <div class="panel panel-default">
          <div class="panel-heading">Lista de clientes
            <!-- <div class="panel-action">
              <a href="#" data-perform="panel-collapse"><i class="ti-minus"></i></a>
              <a href="#" data-perform="panel-dismiss"><i class="ti-close"></i></a>
            </div> -->
          </div>
          <div class="panel-wrapper collapse in">
            <div class="panel-body">
              <div id="container-table">
                <div class="table-responsive" >
                  <table class="table table-dashed table-registros color-bordered-table info-bordered-table" id="dt-clientes">
                      <thead>
                          <tr>
                            <th>RAZON SOCIAL</th>
                            <th>RUC</th>
                            <th>DIRECCIÓN</th>
                            <th>TELÉFONOS</th>
                            <th>CORREO</th>
                            <th>NRO DEUDAS</th>
                            <th>MONTO DEUDA</th>
                            <th>ACCIONES</th>
                          </tr>
                      </thead>
                      <tbody></tbody>
                  </table>
                </div>
              </div>

            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>
<script>
  import ClientesController from '../controllers/ClientesController'
  import {toolsSwal} from '../tools/'
  import dt from 'datatables.net-bs4'
  dt()
  var swal = null
  var app = {
    tableClientes: null,
    containerTable: null,
    dataTableClientes: null,
    btnGuardar: null,
    btnCancelar: null,
    frmRegistrarClientes: null,
    fn: {
      cancelarEditar () {
        console.log(app.frmRegistrarClientes)
        console.log('zzzzzzzzzzz')
        app.frmRegistrarClientes.reset()
        console.log('xccccccccccccccccccc')
        app.frmRegistrarClientes.reset()
        console.log('nnnnnnnnnnnnnnn')
        // setTimeout(()=>{
        //   app.frmRegistrarClientes.reset()
        //   console.log('xxx')
        // }, 5000)
        app.btnGuardar.innerText = 'Registrar'
        // app.btnCancelar.classList.add('hidden')
      }
    }
  }
  var optionData = (dataArray)=> {
    return {
          dom: 'tp',
          processing: true,
          data: dataArray,
          order: [0, 'desc'],
          pageLength: 10,
          columns: [
            {data: 'cli_razon_social'},
            {data: 'cli_ruc'},
            {data: 'cli_direccion'},
            {data: 'cli_telefono'},
            {data: 'cli_correo'},
            {data: 'cli_id'},
            {data: 'cli_id'},
            {data: 'cli_id', render: (d, t, r) => {
              return `<div class="btn-group">
                      <button type="button" class="btn btn-default btn-acciones" data-accion="ver" data-id="${d}"><i class="fa fa-eye"></i></button>
                      <button type="button" class="btn btn-default btn-acciones" data-accion="editar" data-id="${d}"><i class="fa fa-pencil"></i></button>
                      <button type="button" class="btn btn-default btn-acciones" data-accion="eliminar" data-id="${d}"><i class="fa fa-trash"></i></button>
                    </div>`
            }}
          ],
          columnDefs: [{
            targets: [1, 5, 6, 7],
            className: 'td-center'
          }]
        }
  }

  export default {
    name: 'gestion_clientes',
    data () {
      return {
        txtNroRuc: '',
        txtRazonSocial: '',
        txtDireccion: '',
        txtTelefono: '',
        txtCelular: '',
        txtCorreo: '',
        txtReferente: '',
        txtId: 0,
        isEdit: false
      }
    },
    methods: {
      loadClientes (filter = {fn: 'default'}) {
        console.log('go to load')
        var self = this
        ClientesController.listar(datares => {
          console.log(datares)
          if($.fn.DataTable.isDataTable('#dt-clientes'))
            app.dataTableClientes.destroy()
          app.dataTableClientes = app.tableClientes.DataTable(optionData(datares))
          app.tableClientes.on('click', '.btn-acciones', self.fnOnClick_btnAcciones)

        }, filter)
      },
      fnOnClick_btnAcciones (e) {
        var _this = this
        console.log(e.currentTarget.dataset.accion)
        var dataSet =  e.currentTarget.dataset
        switch (dataSet.accion) {
          case 'ver':
            break
          case 'editar':

            //obtener valores de cliente
            ClientesController.clientePorId(dataSet.id, data => {
              if (data.success) {
                _this.isEdit = true

                _this.txtRazonSocial = data.data.cli_razon_social
                _this.txtReferente = data.data.cli_referente
                _this.txtNroRuc = data.data.cli_ruc
                _this.txtCorreo = data.data.cli_correo
                _this.txtTelefono = data.data.cli_telefono
                _this.txtCelular = data.data.cli_celular
                _this.txtDireccion = data.data.cli_direccion
                _this.txtId = data.data.cli_id

                // app.btnCancelar.classList.remove('hidden')
                app.btnGuardar.innerText = 'Actualizar'

              }
            })
            break
          case 'eliminar':
            break
        }

      },
      fnOnClick_btnGuardar () {
        console.log('guardar')
        var _this = this
        if (_this.isEdit) {
          swal(toolsSwal.accionEditar()).then(() => {
            ClientesController.modificarClientePorId(this.txtId, {
              cli_razon_social: this.txtRazonSocial,
              cli_referente: this.txtReferente,
              cli_ruc: this.txtNroRuc,
              cli_correo: this.txtCorreo,
              cli_telefono: this.txtTelefono,
              cli_celular: this.txtCelular,
              cli_direccion: this.txtDireccion
            }, res => {
              if (res) {
                _this.isEdit = false
                swal('Éxito', 'Los cambios fueron actualizados', 'success')

              }
              _this.loadClientes()
              // app.btnCancelar.classList.add('hidden')
              app.btnGuardar.innerText = 'Registrar'
            })
          }, () => {
            //cancelar mconfirmación
          })

        } else {

        }
      },
      fnOnClick_btnCancelar () {
        this.isEdit = false
        app.frmRegistrarClientes.reset()
        app.btnGuardar.innerText = 'Registrar'

      }
    },
    mounted () {
      this.loadClientes()
      app.tableClientes = $('#dt-clientes')
      app.btnCancelar = document.getElementById('btnCancelar')
      app.btnGuardar = document.getElementById('btnGuardar')
      app.frmRegistrarClientes = document.getElementById('frmRegistrarClientes')



      swal = this.$swal
    }
  }
</script>
