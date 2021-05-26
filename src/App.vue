<template>
<div class="app container" :class="Mode">
<div class="Header" >
    <h1><b>TODO</b></h1>
    <div v-if="this.Mode == 'light'" >
            <img src="./assets/images/icon-moon.svg" alt="Moon Icon" @click="Mode = 'dark'" style="cursor: pointer;">
    </div>
    <div v-if="this.Mode == 'dark'" >
            <img src="./assets/images/icon-sun.svg" alt="Sun Icon" @click="Mode = 'light'" style="cursor: pointer;">
    </div>
</div>
<ul>
    <form @submit.prevent="addNewTodo(newToDo)" class="TodoInput" :class="(Mode == 'light') ? 'LightToDoElem' : ''">
<div>
  <span  class="InputDot Flex-1" :class="(Mode == 'light') ? 'LightCircle' : ''"></span>
  <input v-model="newToDo" name="newTodo" class="TodoInputField Flex-2" placeholder="Create a new todo..." :class="(Mode == 'light') ? 'LightToDoElem' : ''">
</div>

</form>
</ul>



<ul>


  <div v-for="todo in ToDos" :key="todo">
      <li v-if="todo.completed == Filter || Filter == 'all'" @mouseover="todo.Xshow = true" @mouseleave="todo.Xshow = false"
      :class="(Mode == 'light') ? 'LightToDoElem' : ''"
      >
        <div v-if="todo.completed == 'active'" class="Flex-1 " >
              <span class="dot" @click="todo.completed = 'completed'" :class="(Mode == 'light') ? 'LightCircle' : ''">
              </span>
        </div>
        <div v-if="todo.completed == 'completed'" class="Flex-1">
              <span class="dot checked" @click="todo.completed = 'active'" :class="(Mode == 'light') ? 'LightCircle' : ''">
                <img src="./assets/images/icon-check.svg" alt="">
              </span>
        </div>

        <div v-if="todo.completed == 'active'" class="Flex-2">
            <p >{{todo.description}}</p>
        </div>
        <div v-if="todo.completed == 'completed'" class="Flex-2" >
              <p style="  text-decoration: line-through;">{{todo.description}}</p>
        </div>

        

        <div class="Flex-3">
            <img src="./assets/images/icon-cross.svg" alt="" @click="ToDos.splice(todo, 1)"  v-if="todo.Xshow">
        </div>
      </li>
  
  </div>



<div class="filter" :class="(Mode == 'light') ? 'LightToDoElem' : ''">
    <p>{{countActive}} items left</p>
    <div class="CenterFilter">
        <p @click="this.Filter = 'all'" class="FilterOptions" :class="(Filter == 'all') ? 'activeFilter' : ''">All</p>
        <p @click="this.Filter = 'active'" class="FilterOptions" :class="(Filter == 'active') ? 'activeFilter' : ''">Active</p>
        <p @click="this.Filter = 'completed'" class="FilterOptions" :class="(Filter == 'completed') ? 'activeFilter' : ''">Completed</p>
    </div>
    <div>
        <p class="FilterOptions">Clear Completed</p>
    </div>

</div>

  
</ul>



</div>

</template>

<script>

export default {
  name: 'App',
  data(){
    return{
          ToDos: [
        { description: "Complete online Javascript course", completed: 'active', Xshow: false},
        { description: "Jog around the park 3x", completed: 'active', Xshow: false },
        { description: "10 minutes meditation", completed: 'active', Xshow: false },
        { description: "Read for 1 hour", completed: 'active', Xshow: false },
        { description: "Pick up groceries", completed: 'active', Xshow: false },
        { description: "Complete Todo App on Frontend Mentor", completed: 'active', Xshow: false },

      ],
      newToDo: '',
      Filter: 'all',
      Mode: 'dark',
    }
  },
  methods:{
    addNewTodo(newToDo){
      this.ToDos.push({ description: newToDo, completed: 'active', Xshow: false });
      this.newToDo = ''
    },

    setAllCompleted(ToDosArray){
      for(i = 0; i < this.ToDos.length; i++){
        this.ToDos.set(ToDosArray.completed, this.ToDos[i], 'completed')
        }
      }
    },
    
    computed:{
      countActive(){
      var activeNum = this.ToDos.filter(x => x.completed == 'active').length;
      return activeNum
      },

    }

  }

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@400;700&display=swap');

    *{
        font-family: 'Josefin Sans', sans-serif;
        box-sizing: border-box;
    }

        .container{
            background: url("./assets/images/bg-desktop-dark.jpg");
            text-align: center;
            color: white;
            background-color: rgb(24, 24, 36);
            margin: 0;
            padding: 0;
            background-repeat: no-repeat;
            background-size: 100%;
            min-width: 100vw;
            min-height: 100vh;
            background-attachment: fixed;
            position: fixed;
            margin: auto auto;
    }

    .xButton{
      background: url("./assets/images/icon-cross.svg");
      background-repeat: no-repeat;
      background-size: 100%;
      cursor: pointer; 
    }

    li{
      list-style-type: none;
      max-width: 700px;
      width: 100%;
      background: rgb(37, 39, 60);
      margin: auto auto;
      padding: 10px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-top: 0.01em solid rgb(78, 77, 77);
        }
        
  .dot {
  height: 25px;
  width: 25px;
  border: 1px solid white;
  border-radius: 50%;
  display: inline-block;
  cursor: pointer; 
}
.InputDot{
  height: 25px;
  width: 25px;
  border: 1px solid white;
  border-radius: 50%;
  display: inline-block;
  margin-left: 15px;
}
.checked{
  background: linear-gradient(hsl(192, 100%, 67%), hsl(280, 87%, 65%))
}
.Flex-1{
  flex:0.3;
  justify-content: flex-start;
}
.Flex-2{
  flex:3;
    text-align: left;

}
.Flex-3{
  flex:0.2
  
}
.filter{
      max-width: 700px;
      width: 100%;
      display: flex;
      background: rgb(37, 39, 60);
      margin: auto auto;
      padding: 10px 20px;
      justify-content: space-between;
      border-top: 0.01em solid rgb(78, 77, 77);

}
.TodoInput{
      max-width: 700px;
      width: 100%;
      background: rgb(37, 39, 60);
      margin: auto auto;
      padding: 10px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      

}
.TodoInputField{
      background: rgb(37, 39, 60);
    border: none;
    outline: none;
    color: white;
    margin-left: 10px;

}
.light{
      background: url("./assets/images/bg-desktop-light.jpg");
      background-repeat: no-repeat;
      background-size: 100%;
      background-color: white;

}
.Header{
      display: flex;
      max-width: 700px;
      width: 100%;
      margin: 0 auto;
      margin-top: 5%;
      padding: 10px 20px;
      align-items: center;
      justify-content: space-between;
}
.activeFilter{
  color: hsl(220, 98%, 61%);
  opacity: 1;

}

.FilterOptions{
  margin: 0 10px;
  opacity: 0.5;
  cursor: pointer; 

}

p.FilterOptions:hover{
    opacity: 1;

}
.CenterFilter{
  display: flex;
}
@media (max-width: 750px){
    .container {
        background: url("./assets/images/bg-mobile-dark.jpg");
        background-repeat: no-repeat;
        background-size: 100%;    
        background-color: rgb(24, 24, 36);
    }
    .light{
      background: url("./assets/images/bg-mobile-light.jpg");
      background-repeat: no-repeat;
      background-size: 100%;
      background-color: white;

}
}
.LightToDoElem{
  background: #ffffff;
  color: black;
  border-top: 0px solid gray;

}

span.LightCircle:hover{
  border: 1px solid black;

}
</style>
