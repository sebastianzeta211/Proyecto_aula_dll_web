<template>
    <table class="table table-condensed table-bordered">
      <thead>
        <tr>
          <th>Código</th>
          <th>Descripción</th>
          <th>Categoría</th>
          <th v-for="atributo of constantes.atributos.producto">
            {{ atributo.nombre }}
          </th>
          <th class="text-right">Costo</th>
          <th v-for="atributo of constantes.atributos.variacion">
            {{ atributo.nombre }}
          </th>
          <th class="text-right">Cantidad</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <template v-for="[producto, detalles] of productos">
          <tr v-for="(detalle, index) of detalles">
            <td v-if="index === 0" :rowspan="detalles.length + 1">
              {{ producto.codigo }}
            </td>
            <td v-if="index === 0" :rowspan="detalles.length + 1">
              {{ producto.descripcion }}
            </td>
            <td v-if="index === 0" :rowspan="detalles.length + 1">
              {{ producto.categoria.nombre }}
            </td>
            <template v-if="index === 0">
              <td v-for="atributo of constantes.atributos.producto" :rowspan="detalles.length + 1">
                {{ producto.atributos[atributo.campo] }}
              </td>
            </template>
            <td class="text-right" v-if="index === 0" :rowspan="detalles.length + 1">
              <span v-if="producto.costo !== null">{{ producto.costo | money }}</span>
            </td>


            <td v-for="atributo of constantes.atributos.variacion">
              {{ detalle.variacion.atributos[atributo.campo] || '(Vacío)' }}
            </td>
            <td class="text-right">{{ accounting.formatNumber(detalle.cantidad) }}</td>


            <td
              class="text-center"
              v-if="index === 0" :rowspan="detalles.length + 1"
              style="width: 1%; white-space: nowrap;"
            >
              <button
                type="button"
                class="btn btn-default btn-xs"
                @click="editarProducto(detalle.variacion.producto)"
                title="Editar ingreso"
              >
                <i class="fa fa-edit"></i>
              </button>
              <a
                class="btn btn-success btn-xs" 
                :href="`/inventario/catalogos/productos/${ detalle.variacion.producto.productoid }/edit`"
                target="_blank"
                title="Editar producto"
              >
                <i class="fa fa-external-link"></i>
              </a>
            </td>
          </tr>
          <tr>
            <th :colspan="constantes.atributos.variacion.length + 1">
              Total producto:
              <span style="float: right">
                {{ accounting.formatNumber(detalles.reduce((carry, item) => carry + item.cantidad, 0)) }}
              </span>
            </th>
          </tr>
        </template>
      </tbody>
      <tfoot>
        <tr>
          <td id="totales" class="text-right" :colspan="4 + constantes.atributos.producto.length + constantes.atributos.variacion.length + 1">
            <strong>Productos: </strong><span>{{ accounting.formatNumber(totales.productos) }}</span>
            <strong>Variaciones: </strong><span>{{ accounting.formatNumber(totales.variaciones) }}</span>
            <strong>Unidades: </strong><span>{{ accounting.formatNumber(totales.unidades) }}</span>
          </td>
          <td></td>
        </tr>
      </tfoot>
    </table>
</template>