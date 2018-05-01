<template>
  <div id="app">
    <h1 class="centralizado">{{ titulo }}</h1>
    <ul class="lista-heroes">
      <li v-for="hero of heroes" v-bind:key="hero.id">
        <div class="painel-hero">
          <img class="img-responsiva" :src="heroImg(hero.id)" :alt="hero.localized_name">
          <p><strong>ID:</strong> <span>{{ hero.id }}</span></p>
          <p><strong>Name:</strong> <span>{{ hero.localized_name }}</span></p>
          <p><strong>Attribute:</strong> <span>{{ hero.primary_attr }}</span></p>
          <p><strong>Attack:</strong> <span>{{ hero.attack_type }}</span></p>
        </div>
      </li>
    </ul>  
  </div>
</template>

<script>
export default {

  name: 'app',

  created() {

    if(localStorage.getItem('localHeroStats')){

      let localHeroStats = localStorage.getItem('localHeroStats');
      this.heroes = JSON.parse(localHeroStats);
      console.log('Consumiu os dados do LocalStorage');

    } else {

      //https://api.opendota.com/api/heroStats/
      // Retorna uma promessa
      let promise = this.$http.get('https://api.opendota.com/api/heroStats/');

      // Resposta da promessa (res)
      // Res.body retorna os dados
      // Salva no localStorage
      /*promise.then(res => {

        res.json().then(heroes => {

          localStorage.setItem('localHeroStats', JSON.stringify(heroes));

        });

      });*/

      promise.then(res => {
        
        this.heroes = res.body;
        localStorage.setItem('localHeroStats', JSON.stringify(res.body));
        console.log('Consumiu os dados da API');
        
      
      }, err => console.log(err));


    }

  },

  data () {
    return {
      titulo: 'DotA 2 - All Heroes',
      subtitulo: 'All Heroes',
      heroes: []
    }
  },
  
  methods: {
    heroImg(id){
      return 'src/assets/heroes/' + id + '.png';
    }
  }


}
</script>

<style scoped>

  * {
    box-sizing: border-box;
    font-size: 16px;
  }

  .centralizado {
    text-align: center;
    color: crimson;
    font-size: 2.6em;
    padding: 1.2em;
  }

  li {
    padding: 20px;
    display: inline-block;
    width: 20%;
  }
  
  .painel-hero {
    padding: 10px;
    background-color: #8080805e;
    transition-property: all;
    transition-duration: .2s;
    color: #fff;
  }
  .painel-hero:hover {
    background-color: #7070705e;
  }

  .img-responsiva {
    width: 100%;
  }

  

</style>
