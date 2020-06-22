<template>
  <div>
      <h1>Game of Thrones - Books</h1>
      
      <!-- <button @click="getBooksData">Get Weather Data</button>
      <div>{{BooksDataList}}</div> -->

      <table>
            <tr>
                <th>Book No#</th>
                <th>Book</th>
                <th>No of Pages</th>
                <th>No of Characters</th>
            </tr>
            <tr v-for="BooksData in BooksDataList" :key="BooksData.id">
                <td>{{BooksData.url.substr(-1)}}</td>
                <td><router-link :to="{ name: 'BooksDetails', params: {id: BooksData.url.substr(-1) } }">{{BooksData.name}}</router-link></td>
                <td>{{BooksData.numberOfPages}}</td>
                <td>{{BooksData.characters.length}}</td>
            </tr>
        </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Books',

  data() {
    return {
      BooksDataList: [],
    };
  },

   mounted() {
        axios
            .get('https://anapioficeandfire.com/api/books')
            .then(response => (
                this.BooksDataList = response.data
                ))
      
    },

}
</script>

<style scoped>
table, td, th {  
  border: 1px solid #ddd;
  text-align: left;
}

table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  padding: 15px;
}
</style>