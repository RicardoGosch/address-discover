<template>
    <div class="container">
        <form action="#">
            <input type="text" placeholder="CEP" v-model="cep" >
            <select name="uf" id="uf" v-model="uf">
                <option value=undefined selected disabled>Selecione</option>
                <option v-for="state in states" v-bind:value="state.uf" v-bind:key="state.uf">{{ state.name }}</option>
            </select>
            <input type="text" v-model="city" placeholder="Cidade">
            <input type="text" v-model="address" placeholder="Rua">
            <button type="submit" @click.prevent='search'>Descobrir</button>
            <button type="reset" @click.prevent='reset'>Limpar</button>
        </form>
    </div>
</template>

<script>
import axios from 'axios';
export default {
	name: 'AddressDiscover',
	data () {
		return {
            cep: undefined,
            uf: undefined,
            city: undefined,
            address: undefined,
            states: [
                {uf: 'AC', name: 'Acre'},
                {uf: 'AL', name: 'Alagoas'},
                {uf: 'AP', name: 'Amapá'},
                {uf: 'AM', name: 'Amazonas'},
                {uf: 'BA', name: 'Bahia'},
                {uf: 'CE', name: 'Ceará'},
                {uf: 'DF', name: 'Distrito Federal'},
                {uf: 'ES', name: 'Espírito Santo'},
                {uf: 'GO', name: 'Goiás'},
                {uf: 'MA', name: 'Maranhão'},
                {uf: 'MT', name: 'Mato Grosso'},
                {uf: 'MS', name: 'Mato Grosso do Sul'},
                {uf: 'MG', name: 'Minas Gerais'},
                {uf: 'PA', name: 'Pará'},
                {uf: 'PB', name: 'Paraíba'},
                {uf: 'PR', name: 'Paraná'},
                {uf: 'PE', name: 'Pernambuco'},
                {uf: 'PI', name: 'Piauí'},
                {uf: 'RJ', name: 'Rio de Janeiro'},
                {uf: 'RN', name: 'Rio Grande do Norte'},
                {uf: 'RS', name: 'Rio Grande do Sul'},
                {uf: 'RO', name: 'Rondônia'},
                {uf: 'RR', name: 'Roraima'},
                {uf: 'SC', name: 'Santa Catarina'},
                {uf: 'SP', name: 'São Paulo'},
                {uf: 'SE', name: 'Sergipe'},
                {uf: 'TO', name: 'Tocantins'},
            ]
        }
    },
    methods: {
        searchByCep: function(){
            let api = 'http://viacep.com.br/ws/'+this.cep+'/json/';
            let self = this;
            this.api(api, function(data){
                if(data.error) return;
                self.fill(data);
            });
        },
        searchByAddress: function(){
            let api = 'http://viacep.com.br/ws/'+this.uf+'/'+this.city+'/'+this.address+'/json/';
            let self = this;
            console.log(api);
            this.api(api, function(data){
                if(data.error) return;
                self.fill(data[0]);
            });
        },
        fill: function(data){
            this.uf = data.uf;
            this.city = data.localidade;
            this.address = data.logradouro;
            this.cep = data.cep;
        },
        api: function(url, callback) {
            axios.get(url)
            .then(function (response) {
                callback(response.data);
            })
            .catch(e => {
                console.error(e);
            });
        },
        search: function(){
            if(this.cep.length === 8 || this.cep.length === 9)
                // Search by cep
                this.searchByCep();
            else
                // Search by address
                this.searchByAddress();
        },
        reset: function(){
            this.cep = undefined;
            this.uf = undefined;
            this.city = undefined;
            this.address = undefined;
        }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
