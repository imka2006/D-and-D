<template>
  <div class="wrapper">
    <setion class="left">
      <img :src="bImg" alt="img" class="left-img">
      <ul class="left-list">
        <li class="left-one"></li>
        <li class="left-two"></li>
        <li class="left-three"></li>
        <li class="left-four"></li>
        <li class="left-five"></li>
        <li class="left-six"></li>
        <li class="left-seven"></li>
      </ul>
    </setion>


    <section class="hr">
      <div @drop="onDrop($event, item.list)" draggable="true" v-for="item in blocks" @dragstart="startDrag($event, item)"
        @dragenter.prevent @dragover.prevent :key="item.id" class="hr-block">
        <div @click="byGood(good)" v-for="good in getList(item.list)" :key="good" class="hr-img-wrapper">
          <img draggable="false" :src="good.img" class="hr-img" alt="svg">
          <span :key="good" class="hr-counter">{{ good.counter }}</span>
        </div>
      </div>
    </section>


    <section class="burger" :class="isActive ? ' active' : ''">
      <img @click="isActive = false" :src="closeImg" alt="close" class="burger-close">

      <img :src="info.img" alt="image" class="burger-img">

      <h2 class="burger-title">вы можете изменить количество</h2>
      <input type="number" class="burger-input" v-model="vall">
      <p v-if="lol" class="burger-descr">количество не может быть 0 или меньше а так же быть пустым</p>
      <p v-if="pop" class="burger-descr">количество не может превышать 64</p>
      <button @click="changeCounter()" class="burger-btn">изменить</button>
      <h2 class="burger-title">или можете удалить</h2>
      <button @click="deleteItem(info)" class="burger-btn">удалить</button>
    </section>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
export default {
  setup() {
    const bImg = ref("./img/B.svg")
    const goods = ref([
      {
        id: 0,
        img: "./img/almaz.png",
        counter: 3,
        list:0
      },
      {
        id: 1,
        img: "./img/kirka.png",
        counter: 1,
        list:1
      },
      {
        id: 2,
        img: "./img/lapata.png",
        counter: 4,
        list:2
      },
    ])
    const blocks = [
      {
        id: 0,
        list:0
      },
      {
        id: 1,
        list:1
      },
      {
        id: 2,
        list:2
      },
      {
        id: 3,
        list:3
      },
      {
        id: 4,
        list:4
      },
      {
        id: 5,
        list:5
      },
      {
        id: 6,
        list:6
      },
      {
        id: 7,
        list:7
      },
      {
        id: 8,
        list:8
      },
      {
        id: 9,
        list:9
      },
      {
        id: 10,
        list:10
      },
      {
        id: 11,
        list:11
      },
      {
        id: 12,
        list:12
      },
      {
        id: 13,
        list:13
      },
      {
        id: 14,
        list:14
      },
      {
        id: 15,
        list:15
      },
      {
        id: 16,
        list:16 
      },
      {
        id: 17,
        list:17
      },
      {
        id: 18,
        list:18
      },
      {
        id: 19,
        list:19
      },
      {
        id: 20,
        list:20
      },
      {
        id: 21,
        list:21
      },
      {
        id: 22,
        list:22
      },
      {
        id: 23,
        list:23
      },
      {
        id: 24,
        list:24
      },
    ]
    const isActive = ref(false)
    const info = ref([])
    const vall = ref(1)
    const lol = ref(false)
    const pop = ref(false)
    const closeImg = ref("./img/close.svg")


    const startDrag = (event, item) => {
      
      event.dataTransfer.dropEffect = "move"
      event.dataTransfer.effectAllowed = "move"
      event.dataTransfer.setData("itemID", item.id)
    }


    const getList = (list) => {
      return goods.value.filter((item) => item.list == list)
    }

    const onDrop = (event, list) => {
      const item = goods.value.find((item) => item.list == list)

      if (!item) {
        const itemID = event.dataTransfer.getData('itemID')
        const item = goods.value.find((item) => item.list == itemID)
        item.list = list
      }
        
    }


    const byGood = (item) => {
      info.value = item
      isActive.value = true
      vall.value = info.value.counter
    }

    const deleteItem = (info) => {
      goods.value = goods.value.filter((item) => item.id !== info.id);
      isActive.value = false
    }

    const changeCounter = () => {
      if (vall.value == "" || vall.value <= 0) {
        lol.value = true
        pop.value = false
      } else if (vall.value > 64) {
        pop.value = true
      } else {
        info.value.counter = vall.value
        lol.value = false
        pop.value = false
      }
    }
    return {
      bImg,
      blocks,
      onDrop,
      startDrag,
      goods,
      getList,
      isActive,
      byGood,
      info,
      vall,
      deleteItem,
      changeCounter,
      lol,
      pop,
      closeImg
    }
  }
}
</script>

<style lang="scss">
body {
  padding: 32px;
  margin: 0;
  background: #1E1E1E;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: sans-serif;
}

.wrapper {
  display: flex;
  gap: 25px;
}

a {
  text-decoration: none;
}

li {
  list-style: none;
}

h1,
h2,
h3,
h4,
h5,
h6,
p,
ul {
  margin: 0;
  padding: 0;
}

.hr {
  display: flex;
  flex-wrap: wrap;
  border-radius: 12px;
  overflow: hidden;
  max-width: 533px;
  border: 1px solid #4D4D4D;

  &-block {
    width: 105px;
    height: 100px;
    border: 1px solid #4D4D4D;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #262626;
    position: relative;
  }

  &-counter {
    font-weight: 500;
    font-size: 10px;
    line-height: 12px;
    color: #FFFFFF;
    padding: 2px 4px;
    position: absolute;
    bottom: -1px;
    right: -1px;
    opacity: 0.4;
    border: 1px solid #4D4D4D;
    border-top-left-radius: 6px;
  }

  &-img-wrapper {
    width: 100%;
    height: 100%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  &-img {
    width: 50%;
    height: 50%;
  }
}

.burger {
  position: fixed;
  right: 0;
  top: 0;
  bottom: 0;
  height: 100%;
  width: 0;
  background: rgba(38, 38, 38, 0.5);
  border-left: 1px solid #4D4D4D;
  backdrop-filter: blur(8px);
  overflow: hidden;
  transition: .3s;
  z-index: 1;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;

  &.active {
    width: 250px;
    transition: .3s;
    padding: 0 15px;
  }

  &-close {
    position: absolute;
    top: 8px;
    right: 8px;
  }

  &-img {
    margin-top: 55px;
    cursor: pointer;
  }

  &-input {

    &::-webkit-outer-spin-button,
    &::-webkit-inner-spin-button {
      -webkit-appearance: none;
    }

  }

  &-title {
    color: white;
    text-align: center;
    font-size: 20px;
  }

  &-btn {
    background: #FF8888;
    border-radius: 8px;
    font-weight: 400;
    font-size: 14px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #FFFFFF;
    width: 100%;
    height: 40px;
  }

  &-descr {
    color: rgb(236, 76, 76);
    font-size: 14px;
    text-align: center;
    max-width: 190px;
  }
}

.left {
  padding: 18px 14px 24px;
  width: 236px;
  height: 500px;
  background: #262626;
  border: 1px solid #4D4D4D;
  border-radius: 12px;
  box-sizing: border-box;

  &-list {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 16px;

    li {
      background: linear-gradient(90deg, #3C3C3C 0%, #444444 51.04%, #333333 100%);
      border-radius: 8px;
    }
  }

  &-one {
    width: 190px;
    height: 26px;
  }

  &-two {
    width: 155px;
    height: 10px;
  }

  &-three {
    width: 190px;
    height: 10px;
  }

  &-four {
    width: 170px;
    height: 10px
  }

  &-five {
    width: 160px;
    height: 10px;
  }

  &-six {
    width: 140px;
    height: 10px;
  }

  &-seven {
    width: 80px;
    height: 10px;
  }
}
</style>