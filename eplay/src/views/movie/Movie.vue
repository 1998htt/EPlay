<template>
  <div class="movie">
    <ul>
      <MovieList v-for="movie in movieList" :movie="movie" @click.native="getDetail(movie)"></MovieList>
    </ul>
    <div class="loading" v-show="isShow">
      <img src="../../assets/img/loading.gif" alt="" >
    </div>
    <div class="end" v-show="isEnd">
      已经到底了...
    </div>

  </div>
</template>
<script>
    import Axios from 'axios';
    import MovieList from '@/views/movie/MovieList.vue';
    export default {
      data(){
          return {
              movieList:[],
              isShow:false,
              isEnd:false
          }
      },
      created(){

          this.getData();
          // Axios.get('/movie.json')
          //     .then((res)=>{
          //         this.movieList = res.data.subjects;
          //         console.log(this.movieList);
          //     }),
          window.onscroll = ()=>{
              var scrollTop = document.documentElement.scrollTop;
              var scrollHeight = document.documentElement.scrollHeight;
              var clientHeight = document.documentElement.clientHeight;
              if(scrollTop + clientHeight == scrollHeight && !this.isEnd){
                    this.isShow = true;
                    this.getData();
              }
              // console.log(scrollTop,scrollHeight,clientHeight);

          }

      },
      methods:{
        getDetail(movie){
            this.$router.push('/getDetail/'+movie.id);
        },
         getData(){
             Axios.get('/movie.json')
                 .then((res)=>{
                     var arr = res.data.subjects.slice(this.movieList.length,this.movieList.length+5);
                     this.movieList = [...this.movieList,...arr];
                     this.isShow = false;
                     if(arr.length < 5){
                         this.isEnd = true;
                     }
                     console.log(this.movieList);
                 })
         }
      },
      components:{
          MovieList,
      }

  }
</script >
<style scoped lang="scss">
    .loading{
      text-align: center;
    }
    .end{
      text-align: center;
    }
</style>
