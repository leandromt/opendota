<template>
  <div id="app">
    <h1 class="centralizado">{{ titulo }}</h1>
    <ul class="lista-heroes">
      <li v-for="hero of heroes" v-bind:key="hero.id">
        <div class="painel-hero">
          <p><strong>ID:</strong> <span>{{ hero.id }}</span></p>
          <p><strong>Name:</strong> <span>{{ hero.name }}</span></p>
          <p><strong>Name 2:</strong> <span>{{ hero.localized_name }}</span></p>
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

  data () {
    return {
      titulo: 'All Heroes Dota 2',
      heroes: []
    }
  },


  created() {

    this.$http.get('https://api.opendota.com/api/heroes/')
      .then(res => res.json())
      .then(heroes => this.heroes = heroes, err => console.log(err));

  }


}
</script>

<style scoped>
  .centralizado {
    text-align: center;
  }
  li {
    padding: 20px;
    display: inline-block;
    width: 25%;
  }
  .painel-hero {
    padding: 10px;
    background-color: darkgrey;
  }
  * {
    box-sizing: border-box;
  }

</style>
