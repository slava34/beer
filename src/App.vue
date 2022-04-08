<template>
  <div id="app" v-if="user">
    <div class="container-fluid">
      <div class="row">
        <div class="left col-xl-3 col-lg-4 col-md-4 col-sm-12 col-12 pt-5">
          <img class="foto_profile" :src="user.avatar"  alt="Фото не найдено">
          <p class="text_ pt-3">{{ user.first_name }}</p>
          <p class="text_spec pt-3">Должность: {{ user.employment.title }}</p>
          <p class="text_spec pt-3">Возраст: {{ computeAge}}</p>
        </div>
        <div class="right col-xl-9 col-lg-8 col-md-8 col-sm-12 col-12 pt-5">
          <div class="frame">
            <h2><p class="text_right pt-3 pb-3">Нужно пиво?</p></h2>
            <h5 class="text_right pb-3"><i>Пейте пиво не спеша – пусть блаженствует душа!</i></h5>
            <button type="button" class="btn btn-outline-primary" @click="getRandomBeer">{{ getBeerButtonName }}</button>
            <h5><p class="text_right pt-4" v-if="beer">Рекомендованное пиво:</p></h5>
            <div class="text_right" v-if="beer">
              <p><strong>Название:</strong> {{ beer.name }}</p>
              <p><strong>Бренд:</strong> {{ beer.brand }}</p>
              <p><strong>Алкоголь:</strong> {{ beer.alcohol }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'App',
  data() {
    return {
      beer: null,
      user: null,
    }
  },
  computed: {
    computeAge() {
      const birthDay = this.$moment(this.user.date_of_birth);
      const now = this.$moment();
      return now.diff(birthDay, 'year')
    },
    getBeerButtonName() {
      return this.beer ? 'Поменять' : 'Подобрать'
    }
  },
  methods: {
    async getRandomBeer() {
      const { data } = await axios.get('https://random-data-api.com/api/beer/random_beer');
      this.beer = data;
      console.log(this.beer);
    },
    async getRandomUser() {
      const { data } = await axios.get('https://random-data-api.com/api/users/random_user');
      this.user = data;
      console.log(this.user);
    }
  },
  created() {
    this.getRandomUser();
  }
}
</script>

<style>
.foto_profile{
  border-radius: 200px;
  border: 1px solid white;
  width: 12em;
  height: 12em;
}

.text_{
  font-family: 'DM Sans', sans-serif;
  font-style: normal;
  font-weight: 700;
  font-size: 48px;
  line-height: 62px;
  color: #F7F7F7;
}

.text_spec{
  font-family: 'DM Sans', sans-serif;
  font-style: normal;
  font-weight: 700;
  font-size: 20px;
  line-height: 26px;
  color: #F7F7F7;
}

.text_right{
  font-family: "Monospace",  sans-serif;
  font-weight: 600;
  font-size: 1.4em;
  color: #4286f4;
}

.left{
  background-color: #4286f4;
  height: 100vh;
  text-align: center;
}

.right{
  text-align: left;
  height: 100vh;

}

.frame{
  border: 4px solid #4286f4;
  padding: 2em;
  border-radius: 15px
}

</style>
