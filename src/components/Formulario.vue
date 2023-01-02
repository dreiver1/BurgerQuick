<template>
    <div>
        <p>Componente de mensagem</p>
        <div>
            <form id="burger-form" @submit="createBurger">
                <div class="input-container">
                    <label for="nome">Nome do cliente: </label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite seu nome">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha seu pão:   </label>
                    <select id="pao" name="pao" v-model="pao">
                        <option value="">Selecione seu pão</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo" >{{pao.tipo}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha a sua carne:     </label>
                    <select id="carne" name="carne" v-model="carne">
                        <option value="">Selecione uma carne</option>
                        <option v-for="carne in carnes" v-bind:key="carne.id" :value="carne.tipo">{{carne.tipo}}</option>
                    </select>
                </div>
                <div class="input-container" id="opcionais-container">
                    <label for="opcionais" id="opcionais-title">Selecione os opciionais</label>
                    <div v-for="opcionais in opcionaisdata" :key="opcionais.id" :value="opcionais.tipo" class="checkbox-container">
                        <input type="checkbox" name="opcionais" :v-model="opcionais" :value="opcionais.tipo">
                        <span>{{opcionais.tipo}}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar meu Burger">
                </div>
            </form>
        </div>
    </div>
</template>
<script>
    export default{
        name: 'Formulario',
        data(){
          return{
            paes: null,
            carnes: null,
            opcionaisdata: null,
            pao: null,
            carne: null,
            opcionais: [],
            msg: null,
          }
        },
        methods: {
          async getIngredientes (){
            const req = await fetch('http://localhost:3000/ingredientes');
            const data = await req.json();
            console.log(data);
            this.carnes = data.carnes;
            this.paes = data.paes;
            this.opcionaisdata = data.opcionais;
          },
          async createBurger(e){
            e.preventDefault();

            const data = {
              nome: this.nome,
              pao: this.pao,
              carne: this.carne,
              opcionais: await Array.from(this.opcionais),
              status: 'solicitado'
            };

            const dataJson = JSON.stringify(data);

            const req = await fetch("http://localhost:3000/burgers", {
              method: "POST",
              headers: { "content-type": "application/json" },
              body: dataJson
            });

            const res = await req.json();

            console.log(res);
            
          }
        },
        mounted() {
          this.getIngredientes();
        }
    }
</script>
<style scoped>
#burger-form {
    max-width: 400px;
    margin: 0 auto;
  }
  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
  }
  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }
  input, select {
    padding: 5px 10px;
    width: 300px;
  }
  #opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
  }
  #opcionais-title {
    width: 100%;
  }
  .checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }
  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }
  .checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
  }
  .submit-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  .submit-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>