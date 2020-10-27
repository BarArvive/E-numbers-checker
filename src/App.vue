<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchEnumber"/>
      </div>
      <div class="ENumber-wrap" v-if="typeof enumber_info.id != 'undefined' && this.valid == true">
        <div class="name-box">
          <div class="name">{{enumber_info.name}}</div>
          <div class="type">{{enumber_info.function}}</div>
        </div>

        <div class="info-box">
          <div class="safe">{{enumber_info.status}}</div>
          <div class="info">{{enumber_info.info}}</div>
          <div class="foods">Found in: {{enumber_info.foods}}</div>
        </div>
        </div>
        <div class="hello-home" v-if="typeof enumber_info.code == 'undefined' && this.valid == undefined">
          <div class="name">Enter an E number and check about it!</div>
        </div>
        <div class="not-found-wrap" v-if="typeof enumber_info.msg != 'undefined' || this.valid == false">
          <div class="name">Oops... E number was not found. Please try again.</div>
        </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
    api_key: 'PRIVATE_KEY',
    url_base: 'https://e-additives.herokuapp.com/additives/',
    query: '',
    enumber_info: {},
    valid: undefined
    }
  },
  methods: {
    fetchEnumber (e) {
      if (e.key == "Enter") {
        if (this.validation(this.query)) {
        this.valid = true;
        fetch(`${this.url_base}${this.query.match(/[1-9][0-9][0-9][a-hA-H]?/i)}`,
        {headers: {
          'X-Authorization': `EAD-TOKENS apiKey=${this.api_key}`,
          'User-Agent': 'NoAgent'
          }})
        .then(res => {
            return res.json();
        }).then(this.setResults);
        }
        else {
          this.valid = false;
        }
      }
    },
    setResults (results) {
      this.enumber_info = results;
    },
    validation(e) {
      const v = e.match(/[eE]?[1-9][0-9][0-9][a-hA-H]?/i);
      console.log(v);
      return (v!= undefined && v != null && v[0] == e );
    }

  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/food-bg3.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;

}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75))
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 18px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.90);
  border-radius: 16px 0px 16px 0px;
}

.name-box .name {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.name-box .type {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.info-box {
  text-align: center;
}
.info-box .safe {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 36px;
  font-weight: 250;
  text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
  margin: 25px 0px;
}
.info-box .info {
  color: #FFF;
  font-size: 24px;
  font-weight: 200;
  font-style: italic;
  margin: 20px 0px;
  text-shadow: 3px 2px rgba(0, 0, 0, 0.25);
}

.info-box .foods {
  color: #FFF;
  font-size: 20px;
  font-weight: 200;
  font-style: italic;
  text-shadow: 3px 2px rgba(0, 0, 0, 0.25);
}


.hello-home .name {
  color: #FFF;
  text-align: center;
  font-size: 30px;
  font-weight: 400;
  margin: 20px 20px;
  text-shadow: 3px 2px rgba(0, 0, 0, 0.25);
}


.not-found-wrap .name {
  color: #FFF;
  text-align: center;
  font-size: 30px;
  font-weight: 400;
  margin: 20px 20px;
  text-shadow: 3px 2px rgba(0, 0, 0, 0.25);
}

</style>
