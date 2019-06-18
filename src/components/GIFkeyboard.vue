<template>
  <div class="gif_keyboard">
    <h1 class="page_title">アニメ系のGIFアニメ検索 -Tenor API-</h1>
    <p class="description"><a href="https://tenor.com/gifapi/" target="_blank">Tenor API</a>使いました。</p>

    <div class="container">
      <div class="serach_box">
        <form class="serach_form" @submit.prevent="searchKeyword">
          <input type="text" placeholder="+animeのキーワードで検索されます" class="keyword">
          <p class="notice">
            ※日本語+animeだと検索精度が落ちる。<br>
            　例）「暑い」と「hot」だとhotの方がより「anime」に即した結果が返る。<br>
            ※日本語の場合はダイレクトに作品名や人物名を入れた方が精度が良い。<br>
            　例）ドラゴンボール
          </p>
        </form>
      </div>

      <div class="result_contents">
        <ul class="result_list" v-if="isResult">
          <li v-for="(item, index) in results" :key='index' :class="index">
            <figure><img :src="item.media[0].gif.url"></figure>
          </li>
        </ul><!-- result_list -->

        <h2 class="title">CATEGORY</h2>
        <ul class="category_list">
          <li v-for="(item, index) in tags" :key='index' :class="index">
            <a href="javascript:void(0)" :data-path="item.path" v-on:click="thumbClick(item.searchterm)">
              <figure><img :src="item.image" :alt="item.name"></figure>
              <span>{{ item.name }}</span>
            </a>
          </li>
        </ul><!-- category_list -->
      </div>
    </div><!-- container -->
  </div>
</template>

<script>
import axios from 'axios';

const ApiKey = 'DW7T9XEGPO3V';
const Locale = ''
const anonid = "https://api.tenor.com/v1/anonid?key=" + ApiKey;

export default {
  name: 'GIFkeyboard',
  data () {
    return {
      tags: {},
      results: {},
      isResult: false
    }
  },
  methods: {
    getData(url, callback){
      axios.get(url)
      .then(function (response) {
        console.log(response);
        callback(response)
      })
      .catch(function (error) {
        console.log(error);
      });
    },

    tenorCallback_anonid(responsetext){
      const response_objects = responsetext.data;
      const anon_id = response_objects.anon_id;

      this.grab_data(anon_id);
    },

    tenorCallback_categories(responsetext){
      const response_objects = responsetext.data;
      const categories = response_objects.tags;
      this.tags = categories;
    },

    grab_data(anon_id){
      const cat_url = "https://api.tenor.com/v1/categories?key=" + ApiKey + '&locale=' + Locale + '&contentfilter=high&Type=trending';

      this.getData(cat_url, this.tenorCallback_categories);

      return;
    },

    thumbClick(keyword) {
      this.isResult = false;
      window.scrollTo(0,0);
      const url = 'https://api.tenor.com/v1/search?tag=anime '+ keyword +'&platform=web&locale=ja' + '&key=' + ApiKey;
      this.getData(url, this.getResultData);
    },

    searchKeyword() {
      const _input = document.querySelector('.keyword');

      this.isResult = false;
      window.scrollTo(0,0);
      const url = 'https://api.tenor.com/v1/search?tag=anime '+ _input.value +'&platform=web&locale=ja' + '&key=' + ApiKey;
      this.getData(url, this.getResultData);
    },

    getResultData(responsetext){
      const response_objects = responsetext.data;
      const result = response_objects.results;
      this.results = result;
      this.isResult = true;
    }
  },
  mounted() {
    // カテゴリーデータ取得
    this.getData(anonid, this.tenorCallback_anonid);
  }
}
</script>

<style scoped lang="scss">
.page_title {
  font-weight: bold;
  font-size: 2.4rem;
  margin-bottom: 20px;
}

.description {
  margin-bottom: 50px;
}

.title {
  font-size: 2rem;
  margin-bottom: 30px;
}

.btn_trigger {
  background-color:rgb(21, 156, 180);
  border-radius: 5px;
  padding: 10px;
  color: #fff;
  margin-bottom: 20px;
}

.serach_box {
  margin-bottom: 50px;

  input {
    border-bottom: 1px solid rgb(1, 126, 175);
    padding: 10px;
    display: block;
    font-size: 2rem;
    width: 100%;
  }

  .notice {
    font-style: italic;
    margin-top: 15px;
    line-height: 1.5;
  }
}


.category_list {
  display: flex;
  flex-wrap: wrap;

  li {
    margin: 0 10px 20px;
    width: 10%;

    a {
      background-color: #eee;
      border-radius: 5px;
      display: block;
      overflow: hidden;
      position: relative;

      &:hover {
        opacity: 0.7;
      }

      figure {
        img {
          vertical-align: bottom;
          width: 100%;
        }
      }

      span {
        display: block;
        text-align: center;
        padding: 5px;
      }
    }
  }
}


.result_list {
  display: flex;
  flex-wrap: wrap;
  border-bottom: 1px solid #333;
  padding-bottom: 50px;
  margin-bottom: 50px;

  li {
    margin: 0 10px 20px;
    width: 20%;

    img {
      width: 100%;
    }
  }
}
</style>
