<template>
  <div class="w-[80%] bg-slate-100">
    <div class="max-w-xl mx-auto my-10 p-5 rounded-md shadow-md bg-blue-300">
      <h2 class="text-2xl font-semibold mb-5">Add Product</h2>

      <form @submit.prevent="addProduct" enctype="multipart/form-data ">
        <!-- Product Name -->
        <div class="mb-4">
          <label for="name" class="block text-gray-600 text-sm font-medium mb-2"
            >Product Name</label
          >
          <input
            type="text"
            id="name"
            v-model="product.name"
            name="name"
            class="w-full px-4 py-[4px] border rounded-md focus:outline-none focus:border-blue-500"
          />
          <p
            v-if="error.name"
            class="text-white bg-red-600 text-sm py-[3px] px-4 rounded-md"
          >
            {{ error.name }}
          </p>
        </div>

        <!-- Description -->
        <div class="mb-4">
          <label
            for="description"
            class="block text-gray-600 text-sm font-medium mb-2"
            >Description</label
          >
          <textarea
            id="description"
            name="description"
            v-model="product.description"
            rows="4"
            class="w-full px-4 h-[50px] border rounded-md focus:outline-none focus:border-blue-500"
          ></textarea>
          <p
            v-if="error.name"
            class="text-white bg-red-600 text-sm py-[3px] px-4 rounded-md"
          >
            {{ error.description }}
          </p>
        </div>

        <!-- Price -->
        <div class="mb-4">
          <label
            for="price"
            class="block text-gray-600 text-sm font-medium mb-2"
            >Price</label
          >
          <input
            type="number"
            id="price"
            name="price"
            v-model="product.price"
            class="w-full px-4 py-2 border rounded-md focus:outline-none focus:border-blue-500"
          />
        </div>

        <!-- image -->
        <div class="mb-4">
          <label
            for="price"
            class="block text-gray-600 text-sm font-medium mb-2"
            >Image</label
          >
          <input
            type="text"
            id="image"
            name="image"
            v-model="product.image"
            class="w-full px-4 py-2 border rounded-md focus:outline-none focus:border-blue-500"
          />
          <p
            v-if="error.name"
            class="text-white bg-red-600 text-sm py-[3px] px-4 rounded-md"
          >
            {{ error.image }}
          </p>
        </div>

        <div class="mb-4">
          <label
            for="category"
            class="block text-gray-600 text-sm font-medium mb-2"
            >Category</label
          >
        </div>
        <div>
          <select
            id="category"
            name="category"
            v-model="product.category"
            class="w-full px-4 py-2 border rounded-md focus:outline-none focus:border-blue-500"
        
          >
            <option v-for="item in data" :key="item._id" :value="item._id">
              {{ item.name }}
            </option>
          </select>
        </div>

        <!-- Submit Button -->
        <button
          type="submit"
          class="bg-blue-500 text-white px-4 py-2 rounded-md hover:bg-blue-700 focus:outline-none focus:bg-blue-700"
        >
          Add Product
        </button>
      </form>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      data: [],
      selectedCategoryId: null,
      error: {},
      product: {
        name: "",
        description: "",
        price: "",
        image: "",
        category: "657bd5c00f2a19ba866208c2",
      },
    };
  },
  mounted() {
    axios
      .get("https://kidtoy.onrender.com/categories")
      .then((response) => {
        this.data = response.data;
      })
      .catch((error) => {
        console.error("Error fetching data:", error);
      });
  },
  methods: {
    addProduct() {
      axios
        .post("https://kidtoy.onrender.com/add", this.product)
        .then((response) => {
          alert("Product added successfully");
          this.$router.push({ name: "product" });
          console.log(response);
        })
        .catch((error) => {
          console.error(error);
          if (error.response.data) {
            this.error = error.response.data;
          }
        });
    },
  },
};
</script>
