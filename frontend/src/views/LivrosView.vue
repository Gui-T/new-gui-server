<script>
  import axios from "axios";
  export default {
    data() {
      return {
        livros: [],
        categorias: [],
        autores: [],
        // preco: [],
        editoras: [],
        livro: {},
      };
    },
    async created() {
      await this.buscarTodosOsLivros();
      await this.buscarTodasAsCategorias();
      await this.buscarTodasAsEditoras();
      await this.buscarTodosOsAutores();
    },
    methods: {
      async buscarTodasAsCategorias() {
        const categorias = await axios.get(
          "https://livraria-server.herokuapp.com/categorias"
        );
        this.categorias = categorias.data;
      },
      async buscarTodasAsEditoras() {
        const editoras = await axios.get(
          "https://livraria-server.herokuapp.com/editoras"
        );
        this.editoras = editoras.data;
      },
      async buscarTodosOsAutores() {
        const autores = await axios.get(
          "https://livraria-server.herokuapp.com/autores"
        );
        this.autores = autores.data;
      },
      async buscarTodosOsLivros() {
        const livros = await axios.get(
          "https://livraria-server.herokuapp.com/livros"
        );
        this.livros = livros.data;
      },
      async salvar() {
        await axios.post(
          "https://livraria-server.herokuapp.com/livros",
          this.livro
        );
        await this.buscarTodosOsLivros();
      },
      async excluir(livro) {
        await axios.delete(
          `https://livraria-server.herokuapp.com/livros/${livro.id}`
        );
        const indice = this.livros.indexOf(livro);
        this.livros.splice(indice, 1);
      },
    },
  };
</script>
<template>
  <div class="form-input form-dark">
    <input type="text" placeholder="Nome do livro" v-model="livro.nome" />
    <select
      class="form-select"
      aria-label="Default select example"
      v-model="livro.categoria"
    >
      <option
        v-for="categoria in categorias"
        :key="categoria.id"
        :value="categoria.nome"
      >
        {{ categoria.nome }}
      </option>
    </select>
    <select
      class="form-select"
      aria-label="Default select example"
      v-model="livro.editora"
    >
      <option
        v-for="editora in editoras"
        :key="editora.id"
        :value="editora.nome"
      >
        {{ editora.nome }}
      </option>
    </select>
    <select
      class="form-select"
      aria-label="Default select example"
      v-model="livro.autor"
    >
      <option v-for="autor in autores" :key="autor.id" :value="autor.nome">
        {{ autor.nome }}
      </option>
    </select>
    <!-- <input type="number" placeholder="Insira o preço" v-model="preco" /> -->
    <button @click="salvar">salvar</button>
  </div>
  <div class="list-items">
    <table class="table table-dark table-striped">
      <thead>
        <tr>
          <th scope="col">Nome</th>
          <th scope="col">Categoria</th>
          <th scope="col">Editora</th>
          <th scope="col">Autor</th>
          <th id="excluir" scope="col">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="livro in livros" :key="livro.id">
          <td>{{ livro.nome }}</td>
          <td>{{ livro.categoria }}</td>
          <td>{{ livro.autor }}</td>
          <td>{{ livro.editora }}</td>
          <!-- <td>R${{ livro.preco }}</td> -->
          <td>
            <button class="btn btn-fff btn-sm" @click="excluir(livro)">
              Excluir
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<style scoped>
#excluir {
  padding-left: 15px;
}
.table {
  background-color: white;
}
td button {
  background-color: rgb(255, 255, 255);
}
</style>
