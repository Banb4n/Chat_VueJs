<template>
  <div class="Home">
    <div class="containerTchat">
        <div class="left col-sm-3">
            <div class="title">
                <h2>Bienvenue <span>@user</span> sur mon tchat</h2>
            </div>

            <hr />

            <div class="countMessages">
                <p>Total messages posté : <span>{{ messages.length }}</span></p>
            </div>

            <hr />

            <div class="col-xs-12">
                <h4>Listes des utilisateurs :</h4>
                <div class="userConnect">
                    <div v-for="message in messages">
                        <div class="user"><i class="fa fa-user-circle" aria-hidden="true"></i> | {{ message.author }}</div>
                    </div>
                </div>
            </div>
            <div class="new">
            <hr />
                <form @submit.prevent = "Enregistrer()">
                    <div class="form-group">
                        <input v-model="newMessage.author" type="text" class="form-control" id="author" placeholder="@Author" required>
                    </div>
                    <div class="form-group">
                        <textarea v-model="newMessage.content" rows="4" class="form-control" name="content" id="content" placeholder="message" required>
                        </textarea>             
                    </div>
                    <button type="submit" class="btn">Envoyer</button>
                </form>
            </div>
        </div>
        <div class="right col-sm-9">
            <div class="containerMessages">
                <div v-if="messages.length == 0">
                    <img src="https://cdn.dribbble.com/users/1092072/screenshots/3306775/cubeloader2.2.gif" width="100%" />
                </div>
                <div v-else>
                    <div v-for="message in messages">
                        <div class="panel col-xs-10 col-xs-offset-1">
                            <div class="user"><i class="fa fa-user-circle" aria-hidden="true"></i> | {{ message.author }}</div>
                            <p class="content">{{ message.content }}</p>
                                <button><i class="fa fa-trash-o" aria-hidden="true"></i></button>
                                <p class="date">{{ message.date }} - {{ message.time }}</p>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'Home',
    data () {
    return {
      messages: [],
      newMessage: {
        id: '',
        author: '',
        content: '',
        date: new Date(Date.now())
      }
    }
  },
  methods: {
    Enregistrer() {
      axios.post('http://localhost:3000/messages', this.newMessage).then((resultat) => {
        this.messages.push(this.newMessage);
        this.newMessage = {};
      })
    }
  },
  created() {
     axios.get('http://localhost:3000/messages').then((resultat) => this.messages = resultat.data);
  }
}
</script>
<style>
body {
    overflow: hidden;
    background-color: #627184;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;        
}

li {
  list-style-type: none;
}

.containerTchat {
    border: 2px solid lightgrey;
    min-height: 100vh;
    width: 100vw;
    padding: 10px;
    background-color: #394F5D;
    background-image: linear-gradient(rgba( 57, 79, 93 , 0.8), rgba( 57, 79, 93 , 0.8)), url("http://iwallpapers2.free.fr/images/Paysages/Hiver/Magnifique_fond_ecran_HD_-_Hiver.jpg");
    background-position: center;
}

.new {
    text-align: center;
    height: 30%;
    top: 60vh;
    transform: translateY(60%);
}

.left {
    background-color: #111c22;
    height: 100vh;
    margin: -10px 0 0 -20px;
    position: relative;
    color: white;
    text-align: left;
    padding: 15px;    
}
.left h2{
    color: #0f97e2;
}
.left h4 {
    color: #0f97e2;
    font-weight: bold;
    font-style: italic;
} 

.containerMessages {
    height: 97vh;
    overflow-y: scroll;
    overflow-x: hidden;
}
.containerMessages::-webkit-scrollbar { 
    display: none;
}

.form-control {
    background-color: #bdc7d3;
    color: black;
}

.panel {
    background-color: #99a4b2;
    color: black;
    text-align: left;
    padding: 5px;
}

.user i {
    color: #0f97e2;
    text-shadow: 0 1px 2px black;
}
.user {
    color: white;
    text-align: center;
    border-radius: 30px;
    background-color: #627184;
    padding: 5px;
    width: 120px;
    height: 28px;
    font-size: 0.9em;
    font-weight: bold;
    overflow: hidden;
    margin-bottom: 15px;
}
.content {
    padding: 10px 20px;
}
.date {
    color: dimgray;
    font-size: 0.9em;
    text-align: right;
}

.userConnect {
   display: flex;
   flex-wrap: wrap;
   justify-content: space-around;
   padding: 10px; 
}

.title {
    padding: 10px 0;
}
.title span {
    color: white;
}

.countMessages {
    text-align: center;
    color: white;
    font-size: 1.5em;
}
.countMessages span {
    color: #0f97e2;
    font-weight: bold;
}

.btn {
    background-color: rgba(0,0,0,0.1);
    border: 1px solid lightgrey;
}
</style>