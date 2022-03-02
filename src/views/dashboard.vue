<template>
  <div class="content">
    <div class="header-content">
      <div class="text-header">
        <span>気づきを与えてくれたメンバー</span>
      </div>
    </div>
    <div class="search">
      <div class="text-search">
        <span>先輩や同僚、上司を検索</span>
      </div>
      <div class="box">
        <input :class="checkClassInput" type="search" placeholder="検索してください" v-model="inputSearch"
               @input="showSelection()">

        <div class="icon-search">
          <i class="fa fa-search icon" aria-hidden="true"></i>
        </div>
      </div>
      <div :class="classSelection">
        <div :class="classChooseSearch" v-for="(searchResult,index) in searchElement" :key="index" v-if="index===0">
          <div :class="classList" v-if="classList">
            <a href="#" style="text-decoration: none;color: #486581;">{{ searchResult.name }}</a>
          </div>
        </div>
        <div class="" v-for="(searchResult,index) in searchElement" :key="index" v-if="index!==0">
          <div :class="classList" v-if="classList">
            <a href="#" style="text-decoration: none;color: #486581;">{{ searchResult.name }}</a>
          </div>
        </div>
      </div>
    </div>
    <div id="text-line">
      <span>気づきを与えてくれた先輩や同僚、上司の一言</span>
    </div>
    <div id="description">
      <span>テキストテキストテキストテキストテキスト</span>
    </div>
    <div id="hr"></div>
    <div id="icon-add">
      <div class="circle">
        <div class="b">
          <i class="fa fa-plus" aria-hidden="true" style="color: #ffffff"></i>
        </div>
      </div>
    </div>
    <div class="add">
      <span>メンバーを追加する</span>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      inputSearch: '',
      classInput: '',
      classSelection: '',
      classChooseSearch: '',
      classList: '',
      searchResults: [],
      searchElement:[]
    }
  },
  created() {
    axios.get('https://api.instantwebtools.net/v1/airlines').then(res => {
      console.log(res.data[1].name)
      this.searchResults = res.data
      this.searchElement = this.searchResults
    })
  },
  computed: {
    checkClassInput() {
      if (this.inputSearch.length === 0) {
        return 'box-search'
      }
      return 'click-search'
    },

  },
  methods: {
    searchKeyWord(keyWord){
      return this.searchResults.filter(dog => {
        return dog.name.toLowerCase().indexOf(keyWord) !== -1 ;
      })
    },
    showSelection(val) {
      this.classSelection = 'select-search'
      this.classChooseSearch = 'choose-search'
      this.classList = 'list'
      let keyWord = this.inputSearch.toLowerCase();
      console.log(keyWord)
      this.searchElement = (keyWord) ? this.searchKeyWord(keyWord) : this.searchResults;
    }
  }
}
</script>

<style scoped>

</style>