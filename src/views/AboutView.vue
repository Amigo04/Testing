<template>
  <div class="main" v-for="(item, i) in data">
      <p class="title"> {{ item.id }}) {{ item.question }}</p>
      <div class="fl" v-for="variant in item.variants">
              <input type="radio" :name="item.id" :id="variant" :value="variant" v-model="picked[i].value">
              <label :for="variant">{{ variant }}</label>
      </div>       
          <p class="answer" v-if="end">{{ result[i].checked? 'ответ верный' : 'ответ не верный' }}</p>
  </div>
      <button class="btn1" @click="send">send</button>
  
</template>
  <script>
  export default {           
      data: () => ({
        data: null,
        answer: null,
        picked: null,
        result: null,
        end: false
          
      }),
      methods: {
        async getdata() {
          const response = await fetch(`http://localhost:3000/questions`)
          this.data = await response.json()
          this.picked = this.data.map(item => ({
              value: null
          }));
      
        },
        async getanswer() {
          const response = await fetch(`http://localhost:3000/answers`) 
          this.answer = await response.json()
        }, 

        send() {
          this.result = this.answer.map((item, i) => ({
              ...item,
              checked: this.picked[i].value === item.answer
          }))
          this.end = true;
        }
        
      },
      mounted(){  
      this.getanswer() 
      this.getdata()
       
      }
    }
    
  </script>

<style scoped>

.main {
  border: 1px solid rgb(0, 0, 0);
  margin: 20px 60px;
  background-color: rgb(54, 54, 248);
}

.title {
  font-size: 30px;
  background-color:  rgb(54, 54, 248);
  margin-left: 10px;
}
label {
  font-weight: bold;
  font-size: 20px;
  color: #001021;
  margin: 30px 10px;
}
.answer {
  background-color: rgb(0, 0, 0);
  color: azure;
}
.fl {
  margin-left: 10px;
  margin-top: 10px;
}
.btn1 {
  border:none;
  width: 100px;
  height: 40px;
  margin-left: 60px;
  border-radius: 10px;
  background-color: black;
  color: white;
  cursor: pointer;
}



</style>

