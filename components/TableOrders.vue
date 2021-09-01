<template>
  <div>
    <b-table :items="orders" :fields="fields" striped responsive="sm" />
  </div>
</template>

<script>
export default {
  props: {
    orders: {
      type: Array,
      default: () => {
        return []
      },
    },
  },
  data() {
    return {
      fields: [
        {
          key: 'id',
          label: '# Pedido',
        },
        {
          key: 'user',
          label: 'Usuario',
          formatter: value => {
            return value.name
          }
        },
        {
          key: 'foods',
          label: 'Comidas',
          formatter: value => {
            let food = ''
            value.forEach((val, index) => {
              if(index !== value.length-1 ){
                food += `${val.name} (${val.pivot.quantity}), `
              }else{
                food += `${val.name} (${val.pivot.quantity})`
              }
            });
            return food
          }
        },
        {
          key: 'created_at',
          label: 'Fecha',
        },
      ],
    }
  },
}
</script>
