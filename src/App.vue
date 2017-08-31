<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 & firebase Application</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>

      <div class="panel-body">
          <form id="form" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title: </label>
              <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
            </div>

            <div class="form-group">
              <label for="bookAuthor">Author: </label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
            </div>

            <div class="form-group">
              <label for="bookURL">URL: </label>
              <input type="text" id="bookURL" class="form-control" v-model="newBook.url">
            </div>
            <input type="submit" class="btn btn-primary" value="Add Book"/>
          </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Book Lists</h3>
      </div>

      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="book in books">
              <td>
                <a v-bind:href="book.url">{{book.title}}</a>
              </td>
              <td>
                {{book.author}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    
  </div>
</template>

<script>

import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: 'AIzaSyD1qPGV5EzvjKbhr3LB5reqGPlVRRMonyI',
  authDomain: 'vuejs-firebase-01-d0d29.firebaseapp.com',
  databaseURL: 'https://vuejs-firebase-01-d0d29.firebaseio.com',
  projectId: 'vuejs-firebase-01-d0d29',
  storageBucket: 'vuejs-firebase-01-d0d29.appspot.com',
  messagingSenderId: '1045937484723'
}

let app = Firebase.initializeApp(config)
let db = app.database()
let bookRef = db.ref('book')
export default {
  name: 'app',
  firebase: {
    books: bookRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function () {
      bookRef.push(this.newBook)
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.url = ''
      toastr.success('Book added')
    },
    removeBook: function (book) {
      bookRef.child(book['.key']).remove()
      toastr.success('Book removed')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
