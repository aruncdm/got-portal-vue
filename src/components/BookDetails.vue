  <template>
  <div id="book-details">
      <h3>{{BookDataList.name}}</h3>
      <div class="book-detail">
          <strong>No of Pages: {{BookDataList.numberOfPages}} </strong>
          <strong>Publisher: {{BookDataList.publisher}}</strong>
          <strong>Country: {{BookDataList.country}}</strong>
          <strong>Media Type: {{BookDataList.mediaType}}</strong>
          <strong>Released: {{BookDataList.released}}</strong>
          <strong>Total Characters: {{BookDataList.characters.length}}</strong>
      </div>
      <div class="review">
          <textarea name="" id="submitted" cols="50" rows="5" v-model="reviewBox" placeholder="add review" />
          <div class="review-btn">
          <button id="book-review" class="btn-primary" v-on:click.prevent="addReview" v-bind="BookDataList.isbn.attrs">Add Review</button>
          </div>
      </div>

        <div class="show-reveiw" style="margin-top: 40px;">
          <h1>Reviews</h1>
          <div id="comment">

            <div class="showing-review reviews" v-if="show">
                <li class="collection-item" v-for="book in books" :key="book.id">
                {{this.reviewBox}}
                </li>
            </div>
            <div class="no-review" v-else>
              <strong>Be the first to review</strong>
            </div>
          </div>

        </div>
  </div>
  </template>

 <script>
    import axios from 'axios';
    import firebase from 'firebase';


    var firebaseConfig = {
        apiKey: "AIzaSyDlabNDbr3gEsTz1xfDso8buuz1JqjDrU4",
        authDomain: "veu-firebase-ef3c0.firebaseapp.com",
        databaseURL: "https://veu-firebase-ef3c0.firebaseio.com",
        projectId: "veu-firebase-ef3c0",
        storageBucket: "veu-firebase-ef3c0.appspot.com",
        messagingSenderId: "512357689714",
        appId: "1:512357689714:web:9782f449603c3190a455eb"
    };
    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);

    var database = firebase.database();
   
    
    export default {
        name: 'BookDetails',
        data() {
            return {
            BookDataList: [],
            books: [],
            reviewBox: '',
            submitted: false,
            show: false
            };
        },
        mounted() {
            const queryString = this.$route.params.id;
            axios
                .get('https://anapioficeandfire.com/api/books/'+ queryString )
                .then(response => (
                    this.BookDataList = response.data
                ))
            
        },
        methods: {
          addReview: function(){

                /* Saving Data */
                var ref = firebase.database().ref('books');
                var bookRef = database.ref('books');
                var isbn =  this.BookDataList.isbn;
                console.log(this.reviewBox);
                console.log(isbn);
                bookRef.push({
                   isbn:isbn,
                    review: this.reviewBox,
                });

                /* retrieving data */
                 ref.onSnapshot(snap => {
                    this.books = [];
                    snap.forEach(doc => {
                        var books = doc.data();
                        books.id = doc.id;
                        this.books.push(books);
                    });
                });
            }
        }
    }
</script>

<style scoped>
    h1, h2 {
        font-weight: normal;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
    .book-detail, .review{
        width: 45%;
        text-align: left;
        display: inline-block;
    }
    .book-detail{
        margin-left: 30px;
        margin-top: 30px;
    }
    .review{
        margin-right: 30px;
    }
    .book-detail strong{
        display: block;
        margin-bottom: 20px;
    }
    textarea#reviewBox {
        height: 180px;
        display: block;
        margin-bottom: 40px;
    }
    .review-btn{
        padding-left: 20%;
    }
    .btn-primary{
        background:rgb(7, 163, 253);
        color: #fff;
        border: none;
        padding: 15px 50px;
        font-size: 16px;
        font-weight: 600;
    }
    .show-reveiw{
        text-align: left;
        margin-left: 60px;
    }
</style>
