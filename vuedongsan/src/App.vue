<template>
  <div>
    
    <!-- 모달 -->
    <!-- <div class="start" :class="{end : modal}">
      <Modal  @closeModal="modal = false" :rooms="rooms" :modalclick="modalclick" :modal="modal"/>
    </div> -->

    <transition name="fade">
      <Modal  @closeModal="modal = false" :rooms="rooms" :modalclick="modalclick" :modal="modal"/>
    </transition>
    

    <div class="menu">
      <!-- <a v-for="작명 in 3" :key="작명">Home</a> -->
      <a v-for="작명 in menus" :key="작명">{{ 작명 }}</a>
    </div>

    <!-- 배너 -->
    <Discount :count="count" v-if="showDiscount == true"/>

    <button @click="priceSort">가격순정렬</button>
    <button @click="printSort1">가격낮은순정렬</button>
    <button @click="printSort2">가나다순정렬</button>
    <button @click="sortBack">되돌리기</button>




    <!-- $event 안해도 i로 전달해도됨 -->
    <Card @openModal="modal = true; modalclick=$event" v-for="(room, i) in rooms" :key="i" :room="room"/>
    
  
  </div>
</template>

<script>

import data from './post.js'
import Discount from './components/Discount.vue'
import Modal from './components/Modal.vue'
import Card from './components/Card.vue'

export default {
  name: 'App',
  data() {
    return {
      // number : 0,     
      modalclick: 0,
      roomsorigin: [...data],
      rooms : data,
      numbers :[0, 0, 0],
      menus: ['Home', 'Shop', 'About'],
      products : ['역삼동원룸', '천호동원룸', '마포구원룸'],
      modal : false,
      object : {name:'kim', age:20},
      showDiscount: true,
      count: 30,
    }
  },
  methods : {
    // increase(){
    //   this.number += 1;
    // }

    priceSort() {
      this.rooms.sort((a, b)=>{
        return a.price - b.price
      })
    },
    sortBack() {
      this.rooms = [...this.roomsorigin];
    },
    printSort1() {
      this.rooms.sort((a, b)=>{
        return b.price - a.price
      })
    },
    printSort2() {
      this.rooms.sort((a, b)=>{
        if(a.title.toLowerCase() > b.title.toLowerCase())
          return 1;
        else if(a.title.toLowerCase() < b.title.toLowerCase())
          return -1;
        else
          return 0;
      })
    }
  },
  // mounted() {
  //   setTimeout(()=>{
  //     this.showDiscount = false;
  //   }, 2000)
  // },

  mounted(){
    let counted = setInterval(()=>{
      this.count -= 1;
      if(this.count == 0) {
        this.showDiscount = false;
        clearInterval(counted);
      }
    }, 1000)
    
  },
  components: {
    // Discount : Discount,
    Discount, // key와 value가 똑같을 경우 한글자로 축약 가능
    Modal,
    Card
  },
}
</script>

<style>
body {
  margin:0;
}

div {
  box-sizing: border-box;
}

.room-img {
  width:100%;
  margin-top:40px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.menu {
  background:darkgoldenrod;
  padding: 15px;
  border-radius: 5px;
}

.menu a {
  color:#fff;
  padding:10px;
}
.start {
  opacity: 0;
  transition: all 1s;
}
.end {
  opacity: 1;
}

.fade-enter-from{
  opacity:0
}
.fade-enter-active{
  transition:all 1s
}
.fade-enter-to{
  opacity:1;
}

.fade-leave-from{
  transform: translateY(-100%);
}
.fade-leave-active{
  transition:all 1s
}
.fade-leave-to{
  transform: translateY(0px);

}
</style>


<!-- 14강 라이프사이클 훅 -->
<!-- 
   1. 컴포넌트는 웹페이지에 표시되기까지 일련의 step을 거침
      그 step을 라이프사이클이라고 함
   2. (1). create -> 데이터만 있는 단계
   3. (2). mount -> template 사이에 있던걸 실제 html로 바꿔줌   
   4. (3). 컴포넌트 생성 -> index.html에 장착  
   5. (4). update -> data가 변하면 component가 재렌더링됨.
   6. (5). unmount -> 컴포넌트가 삭제되는 단계
   -> 라이프사이클 훅을 이용해서 원하는 시점에 코드를 실행 시킬 수 있음.
   -> mounted() {}, 원하는 훅을 methods 이런곳 다음에 그냥 훅을 쓰면 됨.
      * setTimeout(()=>{}) 함수 사용할때는 그냥 함수말고 화살표함수 사용하는게 좋음
        : 화살표 함수는 바깥에 있는 this를 제대로 가져다 쓸 수 있음.
        : 안에서 this 사용하려면 화살표 함수 사용해야 에러 안나고 사용 가능!
   -> 모든 컴포넌트에 훅을 보낼 수 있음 modal.vue에 적으면 modal.vue가 
      마운트 되고 난 다음에 실행
   -> 서버에서 데이터 가져올 때도 라이프사이클 훅안에서 코드를 짬
      :created()나 mounted() 둘 중 하나 선택해서 사용
-->