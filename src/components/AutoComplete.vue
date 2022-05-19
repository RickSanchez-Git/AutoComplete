<template>
    <div class = "container">
        <p class = "title"> Компания </p>
        <input class = "inputClass text" v-model="companyName" placeholder="Название компании">
        <ul v-if = "autoCompleteList.length">
            <li v-for="item in autoCompleteList" :key = "item.name">
                <div class = "companyContainer">
                    <img :src = item.logo onerror="this.style.visibility='hidden'">
                    <div class = "companyInfoContainer">
                        <div class = "companyName text">
                            {{item.name}}
                        </div>
                        <div class = "companyDomain text" @click="hrefTranslocation(item.domain)"> <!-- opening new window -->
                            {{item.domain}}                                                        <!-- in case to reopen page use href=... -->
                        </div>
                    </div>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'AutoComplete',
  data () {
      return {
          companyName: "",
          autoCompleteList: [],
          timeout: setTimeout(1000)
      }
  },
  methods: {
      getInfo () {
          axios
            .get(`https://autocomplete.clearbit.com/v1/companies/suggest?query=${this.companyName}`)
            .then(resp => {
                this.autoCompleteList = resp.data;
            })
            .catch(err => {
                this.autoCompleteList = [];
                console.log(err);
            })
      },
      hrefTranslocation (url) {
          window.open("https://" + url);
      }
  },
  watch: {
      companyName: function (input) {
          if (input.length % 3 == 0) {
              this.getInfo();
          } else {
              this.timeout = setTimeout(() => {
                this.getInfo();
              }, 1000);
          }
      }
  }
}
</script>

<style scoped>
img {
    width: 54px;
    height: 100%;
}
li {
    padding-top: 16px;
    padding-bottom: 5px;
    padding-left: 16px;
}
ul {
    box-sizing: border-box;
    width: 332px;
    left: 41px;
    top: 85px;

    background: #FFFFFF;
    border: 1px solid #E5E5E5;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
    border-radius: 0px 0px 4px 4px;
    margin: 0px;
    padding: 0px;
    list-style-type: none;
    padding-bottom: 24px;
}
.text {
    font-style: normal;
    font-weight: 400;
    font-size: 14px;
    line-height: 16px;

    color: #000000;
}
.companyName {
    font-size: 16px;
    padding-bottom: 4px;
}
.companyDomain {
    cursor: pointer;
    font-size: 14px;
    color:grey;
}
.container {
    text-align: left;
    display: flex;
    flex-direction: column;
    margin: 1px 5px;
}
.inputClass {
    box-sizing: border-box;
    width: 332px;
    height: 33px;
    left: 41px;
    top: 53px;

    background: #FFFFFF;
    border: 1px solid #E5E5E5;
    border-radius: 4px 4px 0px 0px;
    outline: none;
    padding-left: 14px;
}
.title {
    width: 58px;
    height: 14px;
    left: 41px;
    top: 31px;

    font-style: normal;
    font-weight: 400;
    font-size: 12px;
    line-height: 14px;
    color: #484848;
}
.companyContainer {
    display: flex;
    align-items: center;
}
.companyInfoContainer {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    padding-left: 14px;
}
</style>