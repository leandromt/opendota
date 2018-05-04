<template>
  <div id="app">
    <h1 class="centralizado">{{ titulo }}</h1>
    <ul class="lista-heroes">
      <li v-for="hero of heroes" v-bind:key="hero.id">
        <div class="painel-hero" v-on:mouseover="mouseOver(hero.id, hero.localized_name)" v-on:mouseleave="mouseLeave()">
          <img class="img-responsiva" :src="heroImg(hero.id)" :alt="hero.localized_name">
          <div class="painel-info" :class="getClassAttribute(hero.primary_attr)">
          <p><strong>ID:</strong> <span>{{ hero.id }}</span></p>
          <p><strong>Name:</strong> <span>{{ hero.localized_name }}</span></p>
          <p><strong>Attribute:</strong> <span>{{ hero.primary_attr }}</span></p>
          <p><strong>Attack:</strong> <span>{{ hero.attack_type }}</span></p>
          </div>
        </div>
      </li>
    </ul>

  </div>
</template>

<script>
export default {

  name: 'app',

  created() {

    document.body.style.backgroundImage = 'url(src/assets/img/bg/bg-dota2.jpg)';

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
      titulo_original: 'DotA 2 - All Heroes',
      subtitulo: 'All Heroes',
      heroes: []
    }
  },
  
  methods: {
    
    heroImg(id){
      return 'src/assets/img/heroes/' + id + '.png';
    },

    mouseOver(id, name){
      document.body.style.backgroundImage = 'url(src/assets/img/wallpapers/' + id + '.jpg)';
      this.titulo = name;
    },

    mouseLeave(){
      document.body.style.backgroundImage = 'url(src/assets/img/bg/bg-dota2.jpg)';
      this.titulo = this.titulo_original;
    },

    getClassAttribute(attribute){
      return 'painel-info-' + attribute;
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
    background-color: #80808060;
    transition-property: all;
    transition-duration: .2s;
    color: #fff;
    cursor: pointer;
  }

  .painel-hero:hover {
    background-color: #707070de;
  }

  .painel-info {
    position: relative;
  }
  
  .painel-info::before {
    display: block;
    content: "";
    position: absolute;
    width: 30px;
    height: 30px;
    right: 0;
    background-size: 100%;
  }

  .painel-info-str::before {
    background-image: url('./assets/img/attributes/str.png');
  }

  .painel-info-int::before {
    background-image: url('./assets/img/attributes/int.png');
  }

  .painel-info-agi::before {
    background-image: url('./assets/img/attributes/agi.png');
  }

  .img-responsiva {
    width: 100%;
  }



  
  /*  MEDIA QUERY  */
  @media (max-width: 1200px) {
      li {
        width: 33.33%;
      }
  }

  @media (max-width: 768px) {
      li {
        width: 50%;
      }
  }
  
  @media (max-width: 480px) {
      li {
        width: 100%;
      }
  }
  

</style>
