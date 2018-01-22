<template>
    <div class="container">
        <form @submit.prevent="search" class="form">
            <div class="form-group">
                <label class="form-label" for="cep">Descubra o endereço a partir do CEP:</label>
                <input class="form-item" type="text" placeholder="Ex.: 00000-000" id="cep" v-model="cep">
            </div>
            <div class="form-big-divisor">
                <span class="text">Ou</span>
            </div>
            <div class="form-group">
                <section>
                    <h2 class="form-label">Selecione o Estado:</h2>
                    <input type="button" class="form-item form-item-button" title="Selecione um Estado" @click.prevent="toggleStates = !toggleStates" v-bind:value="state ? state.name : 'Selecione'">
                    <div class="group-list" v-bind:class="{show: toggleStates}">
                        <input type="text" class="group-filter" placeholder="Filtre pelos estados" id="filter" v-model="filter" @input="filter = $event.target.value">
                        <div class="group-items">
                            <div class="group-item" v-for="s in filterStates()" v-bind:key="s.uf">
                                <input type="radio" class="form-hidden .radio" v-bind:id="s.uf" v-bind:value="s" v-bind:key="s.uf" v-model="state">
                                <label class="group-label" v-bind:for="s.uf">{{ s.name }}</label>
                            </div>
                        </div>
                    </div>
                </section>
            </div>
            <div class="form-group">
                <label for="city" class="form-label">Digite a Cidade:</label>
                <input  class="form-item" type="text" v-model="city" id="city" placeholder="Ex.: São Paulo">
            </div>
            <div class="form-group">
                <label for="address" class="form-label">Digite o Logradouro/Rua:</label>
                <input class="form-item" type="text" id="address" v-model="address" placeholder="Ex.: Rua São Gabriel">
            </div>

            <div class="form-group form-group__buttons">
                <button class="btn btn-success" type="submit" @click.prevent='search'>Descobrir</button>
                <button class="btn btn-warning" type="reset" @click.prevent='reset'>Limpar</button>
            </div>

        </form>
    </div>
</template>

<script>
import axios from "axios";
export default {
  name: "AddressDiscover",
  data() {
    return {
      cep: undefined,
      state: undefined,
      city: undefined,
      address: undefined,
      toggleStates: false,
      filter: undefined,
      states: [
        { uf: "AC", name: "Acre" },
        { uf: "AL", name: "Alagoas" },
        { uf: "AP", name: "Amapá" },
        { uf: "AM", name: "Amazonas" },
        { uf: "BA", name: "Bahia" },
        { uf: "CE", name: "Ceará" },
        { uf: "DF", name: "Distrito Federal" },
        { uf: "ES", name: "Espírito Santo" },
        { uf: "GO", name: "Goiás" },
        { uf: "MA", name: "Maranhão" },
        { uf: "MT", name: "Mato Grosso" },
        { uf: "MS", name: "Mato Grosso do Sul" },
        { uf: "MG", name: "Minas Gerais" },
        { uf: "PA", name: "Pará" },
        { uf: "PB", name: "Paraíba" },
        { uf: "PR", name: "Paraná" },
        { uf: "PE", name: "Pernambuco" },
        { uf: "PI", name: "Piauí" },
        { uf: "RJ", name: "Rio de Janeiro" },
        { uf: "RN", name: "Rio Grande do Norte" },
        { uf: "RS", name: "Rio Grande do Sul" },
        { uf: "RO", name: "Rondônia" },
        { uf: "RR", name: "Roraima" },
        { uf: "SC", name: "Santa Catarina" },
        { uf: "SP", name: "São Paulo" },
        { uf: "SE", name: "Sergipe" },
        { uf: "TO", name: "Tocantins" }
      ]
    };
  },
  methods: {
    searchByCep: function() {
      let api = "http://viacep.com.br/ws/" + this.cep + "/json/";
      let self = this;
      this.api(api, function(data) {
        if (data.error) return;
        self.fill(data);
      });
    },
    searchByAddress: function() {
      let api =
        "http://viacep.com.br/ws/" +
        this.state.uf +
        "/" +
        this.city +
        "/" +
        this.address +
        "/json/";
      let self = this;
      this.api(api, function(data) {
        if (data.error) return;
        self.fill(data[0]);
      });
    },
    fill: function(data) {
      let self = this;
      this.getState(data.uf, function(state) {
        self.state = state;
      });
      this.stat = data.uf;
      this.city = data.localidade;
      this.address = data.logradouro;
      this.cep = data.cep;
    },
    api: function(url, callback) {
      axios
        .get(url)
        .then(function(response) {
          callback(response.data);
        })
        .catch(e => {
          console.error(e);
        });
    },
    search: function() {
      if (this.cep && (this.cep.length === 8 || this.cep.length === 9))
        // Search by cep
        this.searchByCep();
      else
        // Search by address
        this.searchByAddress();
    },
    reset: function() {
      this.cep = undefined;
      this.state = undefined;
      this.city = undefined;
      this.address = undefined;
    },
    getState: function(state, callback) {
      this.states.forEach(s => {
        if (s.uf === state) callback(s);
      });
    },
    filterStates: function() {
      if (!this.filter) return this.states;

      let expression = RegExp(this.filter.trim(), "i");
      return this.states.filter(state => expression.test(state.name));
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form {
    width: 250px;
    max-width: 100%;
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
}
.form-group {
    width: 100%;
    margin-bottom: 23px;
    position: relative;
}
.form-label {
    font-size: 14px;
    font-weight: bold;
    color: white;
    margin: 0 0 5px 0;
    display: block;
    cursor: default;
}
.form-item {
    width: 100%;
    height: 50px;
    border-radius: 3px;
    font-size: 14px;
    color: #403f4c;
    border: none;
    padding: 0 15px;
    text-align: left;
    background-color: #efe9f4;
    font-family: "Lato", sans-serif;
    box-shadow: none;
}
.group-list {
    background-color: white;
    border-radius: 3px;
    margin: 3px 0 0 0;
    overflow: hidden;
    position: absolute;
    width: 100%;
    z-index: 10;
    box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.25);
    pointer-events: none;
    opacity: 0;
    transition: ease 0.5s;
}
.group-list.show {
    opacity: 1;
    pointer-events: all;
}
.group-filter {
    margin: 15px;
    width: calc(100% - 30px);
    height: 40px;
    padding: 0 15px;
    color: #403f4c;
    border-radius: 3px;
    border: none;
    background-color: #efe9f4;
}
.group-items {
    max-height: 40vh;
    transition: ease 0.5s;
    overflow-y: auto;
    height: 0;
}
.group-list.show .group-items {
    height: 100vh;
}

.group-item {
}
.group-item .group-label {
    color: #403f4c;
    min-height: 30px;
    line-height: 30px;
    padding: 0 30px;
    cursor: pointer;
    margin: 0;
    display: block;
    font-size: 14px;
    font-weight: bold;
}
.group-item input[type="radio"]:checked + .group-label {
    background-color: #efe9f4;
}
.form-hidden {
    position: absolute;
    left: -999pc;
}
.btn {
    height: 50px;
    width: 100%;
    margin: 4px 0;
    border: none;
    text-transform: uppercase;
    color: black;
    border-radius: 4px;
    cursor: pointer;
}
.btn.btn-success {
    background-color: #77cbb9;
    font-weight: bold;
}
.form-big-divisor {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 23px 0 46px 0;
}
.form-big-divisor::after,
.form-big-divisor::before {
    content: "";
    width: 50px;
    height: 1px;
    background-color: #2f2e38;
}
.form-big-divisor .text {
    text-transform: uppercase;
    font-size: 35px;
    color: #2f2e38;
    font-weight: bold;
    margin: 0 10px;
    cursor: default;
}
</style>
