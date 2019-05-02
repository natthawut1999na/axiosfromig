<template>
  <div id="IG">
    <form class="searchForm" @submit="submitSearch">
      <input type="search" class="search" v-model="searchQuery">
      <i class="fa fa-search"></i>
    </form>
    <div v-if="a">
      <h1>{{"#"+name}}</h1>
      <h1>{{count+ " โพสต์"}}</h1>
      <div class="pichead" v-for="b in profile" :key="b.id">  
        <img :src="b.profile_pic_url">
      </div>
    </div>
    <ul class="fixpic">
      <div v-for="c in ig" :key="c.id">
        <img class="pic" :src="c.node.display_url">
        <div class="fa">
          <i class="far fa-heart"></i>
        </div>
      </div>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "IG",
  data() {
    return {
      ig: [],
      searchQuery: "",
      a: false,
      profile:[]
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    submitSearch(submit) {
      submit.preventDefault();
      this.load();
    },
    load() {
      axios
        .get(
          "https://www.instagram.com/explore/tags/" +
            this.searchQuery +
            "/?__a=1"
        )
        .then(result => {
          this.profile = result.data.graphql;
          this.name = result.data.graphql.hashtag.name;
          this.count = result.data.graphql.hashtag.edge_hashtag_to_media.count;
          this.ig = result.data.graphql.hashtag.edge_hashtag_to_media.edges;
          this.a = true;
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>

<style lang="scss" scoped>
#IG {
  width: 100%;
  table {
    width: 100%;
  }
  .pichead {
    position: absolute;
    width: 152px;
    height: 152px;
    margin-top: -150px;
    margin-left: 350px;
    
    
    img{
      border-radius: 100px;
     -webkit-tap-highlight-color: transparent;
    }
  }
  .fixpic {
    margin: 0;
    padding: 0;
    width: 100%;
    position: relative;
  }
  .pic {
    width: 31.1%;
    height: 400px;
    margin: 1%;
    padding: 1px;
    float: left;
    display: grid;
    grid: 150px / auto auto auto;
    grid-gap: 10px;
    opacity: 1;
    -webkit-transition: 0.1s ease-in-out;
    transition: 0.1s ease-in-out;
  }
  .pic:hover {
    opacity: 0.5;
  }

  .searchForm {
    margin-bottom: 2.6rem;
  }
  form {
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    transition: all 1s;
    width: 50px;
    height: 50px;
    background: black;
    box-sizing: border-box;
    border-radius: 25px;
    border: 4px solid black;
    padding: 5px;
  }

  input {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 42.5px;
    line-height: 30px;
    outline: 0;
    border: 0;
    display: none;
    font-size: 1em;
    border-radius: 20px;
    padding: 0 20px;
  }

  .fa {
    box-sizing: border-box;
    padding: 10px;
    width: 42.5px;
    height: 42.5px;
    position: absolute;
    top: 0;
    right: 0;
    border-radius: 50%;
    color: white;
    text-align: center;
    font-size: 1.2em;
    transition: all 1s;
  }

  form:hover {
    width: 200px;
    cursor: pointer;
  }

  form:hover input {
    display: block;
  }

  form:hover .fa {
    background: white;
    color: black;
  }
}
</style>
