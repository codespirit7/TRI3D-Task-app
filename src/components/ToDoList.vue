<template>
    <div class="container">
      <div class="add-item">
        <div class="add-icon" @click="addItem" >
          <i class="fas fa-plus"></i>
        </div>
        <form class = "form" @submit.prevent="addItem">
          <input class = "input" type="text" placeholder="Add a to-do..." v-model="entry">
        </form>
        <div class="feature-icon" :class="{'red-bg': entryFavorite}">
          <i v-if="!entryFavorite" class="far fa-star" @click="entryFavorite = !entryFavorite"></i>
          <i v-else class="fas fa-star" @click="entryFavorite = !entryFavorite"></i>
        </div>
      </div>
      <div class="todo-list">
        <div class="item" :class="{'show': item.show}" v-for="(item, index) in todoList" :key="index">
          <div class="item-checkbox">
            <i v-if="!item.complete" class="fas fa-square" @click="completeItem(item)"></i>
            <i v-else class="fas fa-check-square"></i>
          </div>
          <div class="item-title">
            {{ item.title }}
          </div>
          <div class="feature-icon" :class="{'red-bg': item.favorite}">
            <i v-if="!item.favorite" class="far fa-star" @click="setFavoriteItem(item)"></i>
            <i v-else class="fas fa-star" @click="item.favorite = !item.favorite"></i>
          </div>
        </div>
      </div>
      <div class="show-completed" v-if="completedToDoList.length > 0">
        <div class="button" @click="showCompletedList = !showCompletedList">
          <span v-if="!showCompletedList">show</span><span v-else>hide</span> 
          completed to-do's
        </div>
      </div>
      <div class="todo-list complete-list" v-if="showCompletedList">
        <div class="item" :class="{'show': item.show}" v-for="(item, index) in completedToDoList" :key="index">
          <div class="item-checkbox">
            <i v-if="!item.complete" class="fas fa-square"></i>
            <i v-else class="fas fa-check-square" @click="uncompleteItem(item)"></i>
          </div>
          <div class="item-title">
            {{ item.title }}
          </div>
          <div class="feature-icon" :class="{'red-bg': item.favorite}">
            <i v-if="!item.favorite" class="far fa-star"></i>
            <i v-else class="fas fa-star"></i>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>


    export default {
      data: () => {
        return {
          entry: '',
          entryFavorite: false,
          todoList: [],
          completedToDoList: [],
          showCompletedList: false
        }
      },
      methods: {
        addItem() {
          if(this.entry !== '') {
            let date = new Date;
            let newEntry = {
              id: date.getTime(),
              title: this.entry,
              favorite: this.entryFavorite,
              complete: false,
              show: false
            }
  
            if(newEntry.favorite) {
              this.todoList.splice(0, 0, newEntry);
            }
            else {
              this.todoList.push(newEntry);
            }
  
            
              this.todoList.find(element => element.id === newEntry.id).show = true;
            
          }
  
          this.entry = '';
          this.entryFavorite = false;
        },
        setFavoriteItem(item) {
          item.favorite = !item.favorite;
          item.show = false;
  
          
            let index = this.todoList.findIndex(element => element.id === item.id);
            this.todoList.splice(index, 1);
            this.todoList.splice(0, 0, item);
            item.show = true;
          
        },
        
        completeItem(item) {
          item.complete = !item.complete;
          item.show = false;
  
          
            this.completedToDoList.push(item);
            let index = this.todoList.findIndex(element => element.id === item.id);
            this.todoList.splice(index, 1);
            item.show = true;
         
        },
        uncompleteItem(item) {
          item.complete = !item.complete;
          item.show = false;
  
          
            if(item.favorite) {
              this.todoList.splice(0, 0, item);
            }
            else {
              this.todoList.push(item);
            }
  
            let index = this.completedToDoList.findIndex(element => element.id === item.id);
            this.completedToDoList.splice(index, 1);
            item.show = true;
         
        } 
      }
    }
  </script>
  
  <style  scoped>
    .container {
      padding: 10px;
      width: calc(100% - 20px);
      min-height: calc(100% - 20px);
  
      .add-item {
        display: grid;
        grid-template-columns: 70px auto 70px;
        grid-template-rows: 60px;
        width: 100%;
        height: 60px;
        background-color: #000000;
        border-radius: 5px;
        overflow: hidden;
  
        .add-icon {
          grid-column-start: 1;
          grid-column-end: 1;
          display: flex;
          justify-content: center;
          align-items: center;
          color: #fff;
          font-size: 2rem;
          cursor: pointer;
        }
  
        .form{
            margin-top: 21px;
            font-size: medium;
            max-height: max-content;
        }

        .input{
            font-size: medium;
            border-radius: 8px;
            height: 24px;
            width: 200px;
            margin-top: -5px;
        }
  
      }
  
      .todo-list {
        padding-top: 20px;
  
        .item {
          display: grid;
          grid-template-columns: 70px auto 70px;
          grid-template-rows: 60px;
          width: 100%;
          height: 60px;
          margin-bottom: 2px;
          border: 2px solid black;
          border-radius: 5px;
          overflow: hidden;
          opacity: 0;
          transition: all .5s linear;
  
          .item-checkbox {
            grid-column-start: 1;
            grid-column-end: 1;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 1.6rem;
            color: white;
            cursor: pointer;
          }
  
          .item-title {
            grid-column-start: 2;
            grid-column-end: 2;
            display: flex;
            justify-content: flex-start;
            align-items: center;
            font-size: 1.6rem;
          }
  
          .feature-icon {
            margin-top: 12px;
          }
        }
  
        .show {
          opacity: 1;
        }
      }
  
      .complete-list {
        .item {
          position: relative;
          
          &::before {
            content: '';
            position: absolute;
            top: 30px;
            width: calc(100% - 120px);
            margin: 0 60px;
            border-bottom: 2px solid #555;
          }
        }
      }
  
      .show-completed {
        display: flex;
        justify-content: center;
        align-items: center;
        padding-top: 20px;
  
        .button {
          color: black;
          padding: 12px;
          font-size: 1.3rem;
          text-transform: uppercase;
          border-radius: 5px;
          overflow: hidden;
          cursor: pointer;
        }
      }
  
      .red-bg {
        color: red!important;
      }
    }
  </style>