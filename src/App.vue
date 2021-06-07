<template>
<div class="app container" :class="Mode">
<div class="Header" >
    <h1><b>TODO</b></h1>
    <div v-if="this.Mode == 'light'" >
            <img src="./assets/images/icon-moon.svg" alt="Moon Icon" @click="Mode = 'dark'" style="cursor: pointer; margin-right: -100%">
    </div>
    <div v-if="this.Mode == 'dark'" >
            <img src="./assets/images/icon-sun.svg" alt="Sun Icon" @click="Mode = 'light'" style="cursor: pointer; margin-right: -100%">
    </div>
</div>
<ul>
    <form @submit.prevent="addNewTodo(newToDo)" class="TodoInput" :class="(Mode == 'light') ? 'LightToDoElem' : ''">
<div>
  <span  class="InputDot InputFlex-1" :class="(Mode == 'light') ? 'LightCircle' : ''"></span>
  <input v-model="newToDo" name="newTodo" class="TodoInputField InputFlex-2" placeholder="Create a new todo..." :class="(Mode == 'light') ? 'LightToDoInput' : ''">
</div>

</form>
</ul>



<ul>


  <div v-for="(todo, index) in ToDos" :key="todo">
      <li v-if="todo.completed == Filter || Filter == 'all'" @mouseover="todo.Xshow = true" @mouseleave="todo.Xshow = false"
      :class="(Mode == 'light') ? 'LightToDoElem' : ''"
      >
      
        <div v-if="todo.completed == 'active'" class="Flex-1 " >
              <span class="dot" @click="todo.completed = 'completed'" :class="(Mode == 'light') ? 'LightCircle' : ''">
              </span>
        </div>
        <div v-if="todo.completed == 'completed'" class="Flex-1 desktopCheckBox">
              <span class="dot checked" @click="todo.completed = 'active'" :class="(Mode == 'light') ? 'LightCircle' : ''">
                <img src="./assets/images/icon-check.svg" alt="Check icon" >
              </span>
        </div>
        <div v-if="todo.completed == 'completed'" class="Flex-1 mobileCheckBox">
              <span class="dot checked" @click="todo.completed = 'active'" :class="(Mode == 'light') ? 'LightCircle' : ''">
                <img src="./assets/images/icon-check.svg" alt="Check icon" style=" margin-top: -50%;">
              </span>
        </div>      

        <div v-if="todo.completed == 'active'" class="Flex-2">
            <p class="desktopTodoText">{{todo.description}}</p>
            <p class="mobileTodoText">{{todo.description}}</p>
        </div>
        <div v-if="todo.completed == 'completed'" class="Flex-2" >
              <p class="desktopTodoText" style="  text-decoration: line-through; color: gray; ">{{todo.description}}</p>
              <p class="mobileTodoText" style="  text-decoration: line-through; color: gray;">{{todo.description}}</p>
        </div>

        
        <transition>
            <div class="Flex-3">
                <img src="./assets/images/icon-cross.svg" alt="cross" @click="deleteTodo(index)"  v-if="todo.Xshow" class="DesktopCross"> 
                <img src="./assets/images/icon-cross.svg" alt="cross" @click="deleteTodo(index)" class="MobileCross"> 
            </div>
        </transition>

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
        <p class="FilterOptions" @click="clearCompleted()">Clear Completed</p>
    </div>

</div>

<div class="moibleFilterTop" :class="(Mode == 'light') ? 'lightTopFilter' : ''">
    <p style="margin-top: 3.2%;">{{countActive}} items left</p>
    <div>
        <p class="FilterOptions" style="margin-top: 7%;" @click="clearCompleted()">Clear Completed</p>
    </div>

</div>

<div class="moibleFilterBottom" :class="(Mode == 'light') ? 'lightBottomFilter' : ''">
    <div class="CenterFilter">
        <p @click="this.Filter = 'all'" class="FilterOptions" :class="(Filter == 'all') ? 'activeFilter' : ''">All</p>
        <p @click="this.Filter = 'active'" class="FilterOptions" :class="(Filter == 'active') ? 'activeFilter' : ''">Active</p>
        <p @click="this.Filter = 'completed'" class="FilterOptions" :class="(Filter == 'completed') ? 'activeFilter' : ''">Completed</p>
    </div>
</div>
  
</ul>



</div>

</template>

<script>

export default {
  components: {
  },
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
    deleteTodo(index){
      this.ToDos.splice(index, 1)
    },

    clearCompleted(){
      for(let i = 0; i < this.ToDos.length; i++){
          if(this.ToDos[i].completed === 'completed'){
              this.ToDos.splice(i, 1);
              i--;
          }
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
    h1{
      font-weight: 700;
      letter-spacing: 7px;
    }
    p{
      font-weight: 400;
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
      transform: scale(0); 
      transition: transform .5s ease-in-out;
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
      box-shadow: 10px 70px 25px rgba(32, 34, 54, 0.2);
    border-radius: 3px;

        }
        
  .dot {
  height: 25px;
  width: 25px;
  border: 1px solid white;
  border-radius: 50%;
  display: inline-block;
  cursor: pointer; 
}
.desktopCheckBox{
  display: inline;
}
.mobileCheckBox{
  display: none;

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
.InputFlex-1{
  flex:0.3;
  justify-content: flex-start;

}
.InputFlex-2{
  flex:3;
  text-align: left;
}
.DesktopCross{
  display: inline;
}
.MobileCross{
  display: none;
}
.desktopTodoText{
  display: inline;
  margin-top: 20%;
}
.mobileTodoText{
  display: none;
  margin-top: 1%;
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
.moibleFilterTop{
  display: none;
}
.moibleFilterBottom{
  display: none;
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
.LightToDoInput{
  background: #ffffff;
  color: black;
  border-top: 0px;
  border-radius: 3px;

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

.LightToDoElem{
  background: #ffffff;
  color: black;
  border-top: 0.01em solid rgb(216, 216, 216);

}
.LightToDoInput{
  background: #ffffff;
  color: black;
  border-top: 0px
}
span.LightCircle:hover{
  border: 1px solid black;

}

@media (max-width: 750px){
    .container {
        background: url("./assets/images/bg-mobile-dark.jpg");
        background-repeat: no-repeat;
        background-size: 100%;    
        background-color: rgb(24, 24, 36);
        margin: auto auto;

    }
    p{
      font-size: 14px;
    }
    .light{
      background: url("./assets/images/bg-mobile-light.jpg");
      margin: auto auto;
      background-repeat: no-repeat;
      background-size: 100%;
      background-color: white;

}
    li{
  max-width: 325px;
  margin-left: -2%;
  box-shadow: 10px 60px 25px rgba(32, 34, 54, 0.2);
  
}
.TodoInputField{
      font-size: 14px;

}
.Flex-1{
    margin-left: -4%;
}
.Flex-2{
  margin-left: 2%;
  flex: 5;
}
.Flex-3{
  margin-right: -3%;

}

.InputFlex-1{
      margin-left: -10%;
  flex:0.3;
  justify-content: flex-start;

}
.InputFlex-2{
  flex:3;
  text-align: left;
}
.dot {
  margin-top: 30%;
  height: 20px;
  width: 20px;

}
.desktopTodoText{
  display: none;
}
.mobileTodoText{
  display: inline;
}
.desktopCheckBox{
  display: none;
}
.mobileCheckBox{
  display: inline;

}
.InputDot{
  height: 20px;
  width: 20px;
}
.DesktopCross{
  display: none;
}
.MobileCross{
  display: inline;
  width: 15px;
  height: 15px;
}
.TodoInput{
  max-width: 325px;
  margin-left: -2%;
  border-radius: 3px;

}
.Header{
      max-width: 340px;

}
.filter{
  display: none;
}

.moibleFilterTop{
  max-width: 325px;
  max-height: 50px;
  margin-left: -2%;
  color: gray;
  width: 100%;
  display: flex;
  background: rgb(37, 39, 60);
  padding: 10px 20px;
  justify-content: space-between;
  border-top: 0.01em solid rgb(78, 77, 77);
  border-radius: 3px;

}

.moibleFilterBottom{
    max-width: 325px;
    margin-left: -2%;
    margin-top: 5%;
    width: 100%;
    display: flex;
    background: rgb(37, 39, 60);
    padding: 10px 20px;
    justify-content: space-between;
    border-radius: 3px;
   
  }
.CenterFilter{
  margin: auto auto;
}
.lightTopFilter{
    background: white;
    color: black;
    border-top: 0.01em solid rgb(216, 216, 216);

}
.lightBottomFilter{
    background: white;
    color: black;
    box-shadow: 5px 5px 25px rgba(32, 34, 54, 0.2);
}
}

</style>
