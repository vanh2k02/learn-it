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
        <a class="value-input" v-for="(name,index) in names" :key="index">
          <div class="value">
            {{ name }}
          </div>
          <a href="#" class="icon-delete" @click="deleteValue(index)"><i class="fa fa-times" aria-hidden="true"></i></a>
        </a>
        <div class="icon-search">
          <i class="fa fa-search icon" aria-hidden="true"></i>
        </div>
      </div>
      <div :class="classSelection" v-if="showOrHideSelection">
        <div :class="classChooseSearch" v-for="(searchResult,index) in searchElement" :key="index" v-if="index===0">
          <a href="#" style="text-decoration: none;color: #486581;"
             @click="chooseSelection(searchResult.name)">
            <div :class="classList" v-if="classList">
              {{ searchResult.name }}
            </div>
          </a>
        </div>

        <div class="" v-for="(searchResult,index) in searchElement" :key="index" v-if="index!==0">
          <a href="#" style="text-decoration: none;color: #486581;"
             @click="chooseSelection(searchResult.name)">
            <div :class="classList" v-if="classList">
              {{ searchResult.name }}
            </div>
          </a>
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
      searchElement: [],
      showOrHideSelection: '',
      names: []
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
        this.showOrHideSelection = ''
        return 'box-search'
      }
      this.showOrHideSelection = 'show'
      return 'click-search'
    },

  },
  methods: {
    searchKeyWord(keyWord) {
      return this.searchResults.filter(key => {
        return key.name.toLowerCase().indexOf(keyWord) !== -1;
      })
    },
    showSelection(val) {
      this.classSelection = 'select-search'
      this.classChooseSearch = 'choose-search'
      this.classList = 'list'
      let keyWord = this.inputSearch.toLowerCase();
      this.searchElement = (keyWord) ? this.searchKeyWord(keyWord) : this.searchResults;
    },
    chooseSelection(val) {
      console.log(val)
      this.names.push(val)
      this.inputSearch = ''
    },
    deleteValue(val) {
      this.names.splice(val, 1)
    }
  }
}
</script>

<style scoped>

</style>