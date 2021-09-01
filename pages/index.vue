<template>
  <b-container class="pt-5">
    <b-row>
      <b-col>
        <b-card>
          <h2 class="w-100 text-center">COMIDAS</h2>
          <b-row class="pt-4">
            <b-col
              v-for="(food, Index) in foods"
              :key="Index"
              md="4"
              lg="4"
              class="mb-2"
            >
              <b-link @click="selectedItem(food)">
                <ItemFoods :food="food" />
              </b-link>
            </b-col>
          </b-row>
        </b-card>
      </b-col>
      <b-col>
        <b-card>
          <h2 class="w-100 text-center">PEDIDO</h2>
          <b-row
            v-for="(item, index) in form.items"
            :key="index"
            class="pt-4"
          >
            <b-col cols="5">
              <div class="d-inline-flex align-items-center">
                <h5 class="mb-0">{{ item.name }}</h5>
              </div>
            </b-col>
            <b-col cols="3" class="align-contet-botton pading-quantity">
              <b-form-spinbutton
                v-model="form.items[index].quantity"
                :min="1"
                :max="100"
                class="spin-button-custom"
                size="sm"
              ></b-form-spinbutton>
            </b-col>
            <b-col cols="1" class="align-contet-botton pb-1">
              <b-link class="btn btn-danger" @click="deleteItem(index)">
                Eliminar
              </b-link>
            </b-col>
          </b-row>
          <b-button v-if="form.items.length" variant="success" class="mt-4" @click.prevent="saveOrder()">Crear Pedido</b-button>
        </b-card>
      </b-col>
      <b-col md="12" class="pt-5">
        <b-card>
          <TableOrders :orders="orders" />
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
export default {
  async asyncData({ app }) {
    let { data: foods } = await app.$axios.get('food')
    let { data: orders } = await app.$axios.get('order')
    foods = foods.data
    orders = orders.data
    return {
      foods,
      orders
    }
  },
  data() {
    return {
      orders: [],
      form: {
        id: null,
        items: [],
      },
    }
  },
  methods: {
    selectedItem(data) {
      const quantity = 1
      const searchItem = this.form.items.find((x) => x.id === data.id)
      let position = this.form.items.indexOf(searchItem)
      if (position >= 0) {
        this.form.items[position].quantity =
          this.form.items[position].quantity + 1
        this.$bvToast.toast('Item', {
          title: `Agregado con éxito`,
          variant: 'success',
          solid: true,
          autoHideDelay: 1000
        })
      } else {
        const newItem = {
          id: data.id,
          quantity,
          name: data.name,
        }
        this.form.items.push(newItem)
        this.$bvToast.toast('Agregado con éxito', {
          title: `Item`,
          variant: 'success',
          solid: true,
          autoHideDelay: 1000
        })
      }
      if (position < 0) {
        position = this.form.items.length - 1
      }
    },
    deleteItem(n) {
      this.form.items.splice(n, 1)
      this.$bvToast.toast('Eliminado con éxito', {
          title: `Item`,
          variant: 'success',
          solid: true,
          autoHideDelay: 1000
        })
    },
    saveOrder(){
      this.$axios.post('order', {
        data: this.form.items
      })
      .then(res => {
        this.$bvToast.toast('Pedido', {
          title: `Creado con éxito`,
          variant: 'success',
          solid: true,
          autoHideDelay: 1000
        })
        this.form.items = []
        this.orders.push(res.data.data)
        console.log(res)
      })
      .catch(err => {
        this.$bvToast.toast('Pedido', {
          title: `Error al crear`,
          variant: 'danger',
          solid: true,
          autoHideDelay: 1000
        })
        console.error(err);
      })
    }
  },
}
</script>
