<template>
  <div class="contenedor">
    <h1 align="center">Página de producto</h1>
    <div class="modal oculto"></div>
    <div v-if="datos" class="data">
      <div class="breadcrumbs"></div>
      <div class="fila">
        <div class="imagenes">
          <div class="featuredImg">
            <img :src="selectedImg" alt="Product image" />
          </div>
          <div class="carruselImg">
            <img
              v-for="img in datos.images"
              :key="img"
              :src="img"
              @click="selectedImg = img"
              alt="{{datos.title}}"
            />
          </div>
        </div>
        <div class="detalles">
          <div class="autor"></div>
          <h2>{{ datos.title }}</h2>
          <h3 class="precio">$ {{ datos.price }}</h3>
          <span>{{ datos.vendor }}</span>
          <hr />
          <div v-for="opcion in datos.options" class="fila">
            <!--select :name="opcion.name" :id="opcion.name">
              <option v-for="valor in opcion.values" :value="valor">{{ valor }}</option>
            </select-->
            <div v-if="opcion.name == 'Color'" class="fila">
              <h3>{{ opcion.name }}</h3>
              <div v-for="color in opcion.values" class="selector">
                <input type="radio" :value="color" id="color" v-model="Color" />
                <label @click="Color = color" :for="color" class="etiqueta">
                  {{ color }}
                </label>
              </div>
            </div>
            <div v-if="opcion.name == 'Size'" class="fila">
              <h3>{{ opcion.name }}</h3>
              <div v-for="talla in opcion.values" class="selector">
                <input type="radio" :value="talla" id="talla" v-model="Size" />
                <label
                  @click="
                    Size = talla;
                    this.addClass;
                  "
                  :for="talla"
                  class="etiqueta"
                >
                  {{ talla }}
                </label>
              </div>
            </div>
          </div>
          <hr />
          <input type="number" min="0" name="cantidad" id="cantidad" />
          <hr />
          <div class="fila">
            <button>Favorito</button>
            <button>Carrito</button>
          </div>
          <hr />
          <div class="descripcion">
            <div v-html="datos.description"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ShopifyResult",
  data: () => {
    return {
      datos: null,
      selectedImg: "",
      Color: "",
      Size: "",
      cantidad: 0,
    };
  },
  mounted() {
    const url =
      "https://graditest-store.myshopify.com/products/free-trainer-3-mmw.js";
    axios({
      method: "get",
      url,
      ResponseType: "json",
    })
      .then((result) => {
        console.log(result.data);
        this.datos = result.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  watch: {
    datos(newData, oldData) {
      if (newData && !oldData) {
        this.selectedImg = newData.images[0];
      }
    },
  },
};
</script>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
