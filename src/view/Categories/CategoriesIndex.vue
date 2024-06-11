<template>
  <div class="container">
    <h2 class="my-4">Categorias</h2>
    <div class="mb-3">
      <router-link to="/Categories/create" class="btn btn-primary">Adicionar Categoria</router-link>
    </div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Informação Extra</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="category in categories" :key="category.id">
          <td>{{ category.id }}</td>
          <td>{{ category.name }}</td>
          <td>{{ category.extra_info }}</td>
          <td>
            <router-link :to="`/Categories/${category.id}`" class="btn btn-warning btn-sm">Editar</router-link>
            <button @click="deleteCategory(category.id)" class="btn btn-danger btn-sm">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-if="errorMessage" class="alert alert-danger mt-3">{{ errorMessage }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      categories: [],
      errorMessage: ''
    };
  },
  created() {
    this.fetchCategories();
  },
  methods: {
    fetchCategories() {
      axios.get('http://localhost:8080/api/categories')
        .then(response => {
          this.categories = response.data._embedded.categoriesDtoList;
        })
        .catch(error => {
          console.error("Erro ao buscar categorias:", error);
          this.errorMessage = "Erro ao buscar categorias.";
        });
    },
    deleteCategory(id) {
      axios.delete(`http://localhost:8080/api/categories/${id}`)
        .then(response => {
          this.fetchCategories();
        })
        .catch(error => {
          console.error("Erro ao excluir categoria:", error);
          this.errorMessage = "Erro ao excluir categoria.";
        });
    }
  }
};
</script>

<style scoped>
.error {
  color: red;
}
</style>
