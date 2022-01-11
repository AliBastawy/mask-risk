<template>
  <div id="app" :dir="direction">

    <div class="container">
      <div class="row">
        <div class="col-sm-2"></div>
        <div class="col-sm-8">{{ $t('covid-message') }}</div>
        <div class="col-sm-10">{{ $t('table-info') }}</div>
          <div class="col-sm-2"></div>
          <div class="col-sm-10">{{ $t('rows-info') }}</div>
          <div class="col-sm-2"></div>
          <div class="col-sm-10">{{ $t('column-info') }}</div>
      </div>

      <div class="row" dir="ltr">
        <div class="col-sm-2"></div>
        <div class="col-sm-3 form-check form-switch">
          <input
            v-model="checkWild"
            class="form-check-input"
            type="checkbox"
            id="wild-button"
            checked>
          <label
            class="form-check-label wild"
            for="wild-button"
          >
            Wild
          </label>
        </div>
        <div class="col-sm-3 form-check form-switch">
          <input
            v-model="checkDelta"
            class="form-check-input"
            type="checkbox"
            id="delta-button"
            checked>
          <label
            class="form-check-label delta"
            for="delta-button"
          >
            Delta
          </label>
        </div>
        <div class="col-sm-3 form-check form-switch">
          <input
            v-model="checkOmicron"
            class="form-check-input"
            type="checkbox"
            id="omicron-button"
            checked>
          <label
            class="form-check-label omicron"
            for="omicron-button"
          >
            Omicron
          </label>
        </div>
      </div>
    </div>
    <h1>{{ $t('good-person') }}</h1>
    <div class="wrapper">
      <div>
        <h1>
          {{ $t('infectious-person') }}
        </h1>
      </div>
      <div class="table-responsive-sm">
        <table class="table">
        <thead>
        <tr>
          <th></th>
          <th v-for="header in headers" :key="header">
            {{ $t(header) }}
          </th>
        </tr>
        </thead>
        <tbody v-for="(header, index) in headers" :key="header">
          <th scope="row" rowspan="4">{{ $t(header) }}</th>
          <tr>
            <td
              v-for="(head, headIndex) in headers"
              :key="headIndex"
              :style="`background-color: #${jsonData[index].color[head]}`"
            >
              <table>
                <tr v-if="checkWild || jsonData[index].wild.x[head] === '1.0X'"
                    class="wild">{{ $t(jsonData[index].wild.x[head]) }}</tr>
                <tr v-if="checkDelta" class="delta">{{ $t(jsonData[index].delta.x[head]) }}</tr>
                <tr v-if="checkOmicron" class="omicron">
                  {{ $t(jsonData[index].omicron.x[head]) }}
                </tr>
              </table>
            </td>
          </tr>
        </tbody>
      </table>
      </div>
    </div>
  </div>
</template>

<script>
import data from '../data.json';
import 'bootstrap/dist/css/bootstrap.min.css';

export default {
  name: 'App',
  data() {
    return {
      headers: [
        'Nothing',
        'Cloth',
        'SM',
        'SM,fit',
        'N95',
        'N95,fit',
      ],
      checkWild: true,
      checkDelta: true,
      checkOmicron: true,
      jsonData: [],
      location: '',
      windowWidth: 0,
    };
  },
  created() {
    this.jsonData = data;
    this.location = window.location.href.substring(window.location.href.lastIndexOf('/') + 1);
    if (this.location === 'ar') this.$i18n.locale = 'ar';
  },
  computed: {
    direction() {
      return this.location === 'ar' ? 'rtl' : 'ltr';
    },
  },
  mounted() {
    const topValue2 = (document.querySelector('.wrapper').offsetHeight - document.querySelector('.wrapper h1').offsetHeight) / 2;
    document.querySelector('.wrapper h1').style.top = `${topValue2}px`;
    window.addEventListener('resize', () => {
      this.windowWidth = window.innerWidth;
      const topValue = (document.querySelector('.wrapper').offsetHeight - document.querySelector('.wrapper h1').offsetHeight) / 2;
      document.querySelector('.wrapper h1').style.top = `${topValue}px`;
    });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  box-sizing: border-box;
}
.form-check .form-check-input {
  float: none;
}
input[type=checkbox] {
  margin-right: 10px;
}
.form-check-input:checked {
  border-color: transparent;
}
#wild-button.form-check-input:checked {
  background-color: #000;
}
#wild-button.form-check-input:focus {
  box-shadow: 0 0 0 0.25rem rgb(48 50 54 / 30%);
}
#delta-button.form-check-input:checked {
  background-color: #f00;
}
#delta-button.form-check-input:focus {
  box-shadow: 0 0 0 0.25rem rgb(54 48 51 / 30%);
}
.wild,
.delta,
.omicron {
  font-weight: bold;
}
.wild {
  color: #000;
}
.delta {
  color: #f00;
}
.omicron {
  color: #00f;
}
table {
  margin: auto;
}
.table>:not(caption)>*>* {
  padding: 10px;
  border-bottom: 0;
  border-right: 1px solid white;
}
.table>:not(:first-child) {
  border: 1px solid white;
}
th {
  font-size: 20px;
}
td.table-5-5 tr:nth-child(1) {
  color: #fff;
}
td.table-5-5 tr:nth-child(2) {
  color: #da85dd;
}
td.table-5-5 tr:nth-child(3) {
  color: #10d3c1;
}
table table {
  height: 72px;
}
.wrapper {
  position: relative;
}
.wrapper h1 {
  position: absolute;
}
#app[dir=ltr] .wrapper h1 {
  writing-mode: vertical-rl;
  text-orientation: mixed;
}
#app[dir=rtl] .wrapper h1 {
  writing-mode: vertical-rl;
  text-orientation: mixed;
}
#app[dir=ltr] .table-responsive-sm {
  padding-left: 30px;
}
#app[dir=rtl] .table-responsive-sm {
  padding-right: 44px;
}
@media screen and (max-width: 480px) {
  .table>:not(caption)>*>*, th {
    padding: 2px;
    font-size: 9px;
  }
  #app[dir=rtl] .wrapper h1, h1 {
    font-size: 14px;
  }
}
@media screen and (max-width: 600px) {
  #app[dir=rtl] .table-responsive-sm {
    padding-right: 33px;
  }
}
@media (min-width: 480px) and (max-width: 600px) {
  .table>:not(caption)>*>*, th {
    padding: 5px;
    font-size: 14px;
  }
  .wrapper h1 {
    font-size: 22px;
  }
  #app[dir=ltr] .table-responsive-sm {
    padding-left: 30px;
  }
}
@media (min-width: 480px) and (max-width: 520px) {
  #app[dir=ltr] .wrapper h1 {
    font-size: 19px;
  }
}
@media (min-width: 520px) and (max-width: 848px) {
  #app[dir=ltr] .wrapper h1 {
    font-size: 22px;
  }
}
@media (min-width: 480px) and (max-width: 800px) {
  #app[dir=rtl] .wrapper h1 {
    font-size: 23px;
  }
}
@media screen and (min-width: 600px) {
  #app[dir=ltr] .table-responsive-sm {
    padding-left: 40px;
  }
}
</style>
