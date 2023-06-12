<script>
import VeiculosApi from "@/api/veiculos";
const veiculosApi = new VeiculosApi();
import MarcasApi from "@/api/marcas";
const marcasApi = new MarcasApi();
import CoresApi from "@/api/cores";
const coresApi = new CoresApi();
import AcessoriosApi from "@/api/acessorios";
const acessoriosApi = new AcessoriosApi();
import CategoriasApi from "@/api/categorias";
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      veiculos: [],
      veiculo: {},
      marcas: [],
      categorias: [],
      cores: [],
      acessorios: [],
      preco: {},
      ano: {},
      descricao: {},
    };
  },
  async created() {
    this.veiculos = await veiculosApi.buscarTodasOsVeiculos();
    this.marcas = await marcasApi.buscarTodasAsMarcas();
    this.cores = await coresApi.buscarTodasAsCores();
    this.acessorios = await acessoriosApi.buscarTodasOsAcessorios();
    this.categorias = await categoriasApi.buscarTodasAsCategorias();
  },
  methods: {
    async salvar() {
      if (this.veiculo.id) {
        await veiculosApi.atualizarVeiculo(this.veiculo);
      } else {
        await veiculosApi.adicionarVeiculo(this.veiculo);
      }
      this.veiculo = {};
      this.veiculos = await veiculosApi.buscarTodasOsVeiculos();
    },
    editar(veiculo) {
      Object.assign(this.veiculo, veiculo);
    },
    async excluir(veiculo) {
      await veiculosApi.excluirVeiculo(veiculo.id);
      this.veiculos = await veiculosApi.buscarTodasAsveiculos();
    },
  },
};
</script>

<template>
  <h1>veiculo</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="veiculo.descricao" placeholder="Descrição" />
    <button @click="salvar">Salvar</button>
  </div>
  <hr />
  <ul>
    <li v-for="veiculo in veiculos" :key="veiculo.id">
      <span @click="editar(veiculo)">
        ({{ veiculo.id }}) - {{ veiculo.descricao }} - {{ veiculo.ano }} -
        {{ veiculo.preco }} - {{ veiculo.cor.descricao }}
      </span>
      <button @click="excluir(veiculo)">X</button>
    </li>
  </ul>
</template>

<style></style>
