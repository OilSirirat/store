<template>
  <div class="hello">
    <div class="a" align= "center">GOT7 &nbsp;&nbsp;STORE</div>
    <div class="topnav">
    <div class="tabs is-toggle  is-right">
  <ul>
    <li  @click="typeOrder()">
      <a>
        <span ><i class="fas fa-film" aria-hidden="true"></i></span>
        <span>Order</span>
      </a>
    </li>
    <li  @click="typestock()">
      <a>
        <span ><i class="far fa-file-alt" aria-hidden="true"></i></span>
        <span>Stock</span>
      </a>
    </li>
  </ul>
</div>
</div><br><br>


<div v-if = "type === 'stock'" >

<div class="columns">
  <div class="column"></div>
    <div class="column is-two-thirds">
      <table>
        <tr>
          <td> <input size=60 class="input is-focused" type="text" placeholder="ชื่อสินค้า"  v-model="data.name" ></td>  &nbsp; &nbsp; &nbsp; &nbsp;
          <td> <input size="20" class="input is-focused" type="text" placeholder="จำนวน"  v-model="data.number"></td>&nbsp; &nbsp; &nbsp; &nbsp;
          <td> <input size="20" class="input is-focused" type="text" placeholder="ราคา"  v-model="data.price"></td> &nbsp; &nbsp; &nbsp; &nbsp;
          <td>
            <div class="file">
                  <label class="file-label">
                  <input class="file-input" type="file" name="resume"  @change="onFileChange($event.target.files[0])"required>
                    <span class="file-cta">
                      <span class="file-icon">
                        <i class="fas fa-upload"></i>
                      </span>
                      <span class="file-label">
                        Choose a file…
                      </span>
                    </span>
                  </label>
                </div>
          </td> &nbsp; &nbsp; &nbsp; &nbsp;
          <td> <a class="button is-primary is-outlined" @click="insert()">+ADD</a> </td>
        </tr>
      </table>
    </div>
  <div class="column"></div>
</div>
<div class="columns">
  <div class="column"></div>
      <table class="table is-fullwidth" >
        <thead>
                <tr>
                      <th scope="col"> </th>
                      <th scope="col">Name</th>
                      <th scope="col">Number</th>
                      <th scope="col">Price</th>
                      <th scope="col">Update</th>
                      <th scope="col">Delete</th>
                  </tr>
        </thead>
        <tbody v-for =" (stock,key) in final2"  >
          <tr  v-if = "sw !== key" >
            <td>  <img :src="stock.image"style="width: 100px; height: 100px;"></td>
            <td> </br> </br>{{stock.name}}</td>
            <td> </br></br> {{stock.number}}</td>
            <td> </br></br> {{stock.price}}</td>

            <td> </br></br><a class="button is-primary is-outlined" @click="swap(key)">Update</a> </td>
            <td> </br></br><a class="button is-primary is-outlined" @click="Delete(key)">Delete</a> </td>
         </tr>
            <tr  v-else >
              <td>  <input class="input is-focused" type="text"   v-model="stock.name" ></td>
              <td>   <input class="input is-focused" type="text"  v-model="stock.number" ></td>
              <td>  <input class="input is-focused" type="text"   v-model="stock.price" ></td>
              <td>
                <div class="file">
                      <label class="file-label">
                      <input class="file-input" type="file" name="resume"  @change="onFileChange($event.target.files[0])"required>  &nbsp; &nbsp; &nbsp; &nbsp;
                        <span class="file-cta">
                          <span class="file-icon">
                            <i class="fas fa-upload"></i>
                          </span>
                          <span class="file-label">
                            Choose a file…
                          </span>
                        </span>
                      </label>
                    </div>
              </td>
              <td> <a class="button is-primary is-outlined" @click="Update(key,stock.name,stock.number,stock.price)">Save</a> </td>
              <td> <a class="button is-primary is-outlined" @click="cancel()">Cancle</a> </td>
           </tr>
            </tbody>
      </table>
    </div>
  <div class="column"></div>


</div>
<!-- จบstock -->
<div class="order"  v-if = "type === 'Order'" >

      <table class="table is-striped is-narrow is-hoverable is-fullwidth" >
        <tbody v-for =" (stock,key) in final2"  >
          <tr>
            <td >  <img :src="stock.image"style="width: 150px; height: 200px;"></td>
            <td>  </br></br></br><b>  {{stock.name}} </b>
            </br>  จำนวน  {{stock.number}}
            </br> ราคา {{stock.price}}
            </td>
            <td> </br></br></br></br>
                <div class="select">
                  <select v-model="selected">
                    <option disabled value="">select</option>
                    <option>1</option>
                    <option>2</option>
                    <option>3</option>
                    <option>4</option>
                    <option>5</option>
                  </select>
                </div>
            </td>
            <td></td><td></td>
            <td> </br></br></br> </br>
               <!-- @click="bye(key,stock.number)" -->
                  <a  class="button is-success" @click="confirm(stock.name,stock.number,selected,stock.price,key)">
                    <span class="icon is-small">
                      <i class="fas fa-cart-plus"></i>
                    </span>
                    <span>BUY</span>
                  </a>
            </td>
         </tr>
            </tbody>
      </table>
</div>
</div>
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyCcgsJGHXZvsVZGrYUyr1akyhAIG0Iwwh0',
  authDomain: 'final2-2ff21.firebaseapp.com',
  databaseURL: 'https://final2-2ff21.firebaseio.com',
  projectId: 'final2-2ff21',
  storageBucket: 'final2-2ff21.appspot.com',
  messagingSenderId: '576814123200'
  }
  firebase.initializeApp(config)
export default {
  name: 'HelloWorld',
  data () {
    return {
      data: {
        name: '',
        number: '',
        price: '',
        image: ''
      },
      showstock: '',
      sw: '',
      type: 'Order'
    }
  },
  created: function () {
    this.pullData()
  },
  methods: {
    pullData: function () {
      let that = this
      firebase.database().ref('/stock/').once('value').then(function (snapshot) {
        that.final2 = snapshot.val()
      })
    },
    async insert () {
      let urlsImg = await this.createImage()
      this.data.image = urlsImg.downloadURL
      firebase.database().ref('stock').push(this.data)
      this.data.name = ''
      this.data.number = ''
      this.data.price = ''
      this.data.image = ''
      this.pullData()
    },
    async Update (key, name, number, price) {
      this.sw = ''
      let urlsImg = await this.createImage()
      firebase.database().ref('/stock/').child(key).update({
        name: name,
        number: number,
        price: price,
        image: urlsImg.downloadURL
      })
      this.sw = 'update'
      this.pullData()
    },
    Delete (key) {
      firebase.database().ref('stock').child(key).remove()
      this.pullData()
    },
    swap: function (key) {
      this.sw = key
    },
    cancel () {
      this.sw = ''
    },
    onFileChange (fileImg) {
      this.dataImg = fileImg
    },
    createImage () {
      const storageRef = firebase.storage().ref('image/' + this.dataImg.name.toLowerCase().split(' ').join('-'))
      const uploadTask = storageRef.put(this.dataImg)
      return uploadTask
    },
    typestock () {
      this.type = 'stock'
    },
    typeOrder () {
      this.type = 'Order'
    },
    bye (key, number) {
      firebase.database().ref('/stock/').child(key).update({
        number: number - 1
      })
      this.pullData()
    },
    confirm(name,number,selected,price,key) {
      if (selected<=number) {
        this.$dialog.confirm({
          title: 'Privacy Politics',
          message: name +  '</br>ราคา &nbsp;&nbsp;' + price + '</br>จำนวน &nbsp;&nbsp;' + selected + '</br>ราคารวม &nbsp;&nbsp;' + (selected*price),
          onConfirm: () => {
          firebase.database().ref('/stock/').child(key).update({
            number: number - selected
          })
          this.pullData()
        }
        })
        this.pullData()
      }
      else {
        this.$dialog.alert({
          title: 'Error',
          message: 'Insufficient Product',
          type: 'is-danger',
          hasIcon: true,
          icon: 'times-circle',
          iconPack: 'fa'
        })
        this.pullData()
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
.order{
  margin-left: 20%;
  width: 60%;
}
.header {
    background-color: #f1f1f1;
    padding: 20px;
    text-align: center;
}
.topnav {
    overflow: hidden;
    background-color: #333;
}
.topnav a {
    float: left;
    display: block;
    color: #f2f2f2;
    text-align: center;
    padding: 14px 16px;
    text-decoration: none;
}
.topnav a:hover {
    background-color: #ddd;
    color: black;
}
.a {
    font-family:  Impact, Charcoal, sans-serif;
    font-size:60px;
    align-items: center;
    color:#282828;
}
.column {
    float: left;
    width: 33.33%;
    padding: 15px;
}
</style>
