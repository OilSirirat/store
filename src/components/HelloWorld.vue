<template>
  <div class="hello">
    <div class="tabs is-toggle  is-right">
  <ul>
    <router-link to="/">
    <li>
      <a>
        <span class="icon is-small"><i class="fas fa-film" aria-hidden="true"></i></span>
        <span>Order</span>
      </a>
    </li>
    </router-link>
    <router-link to="/Stock">
    <li>
      <a>
        <span class="icon is-small"><i class="far fa-file-alt" aria-hidden="true"></i></span>
        <span>Stock</span>
      </a>
    </li>
    </router-link>
  </ul>
</div>  <br><br><br>

<table class="table is-fullwidth" >
  <thead>
          <tr>
                <th scope="col">Name</th>
                <th scope="col">Number</th>
                <th scope="col">Price</th>
                <th scope="col"></th>
                <th scope="col">Update</th>
                <th scope="col">Delete</th>
            </tr>
  </thead>
  <tbody v-for =" (stock,key) in final2"  >
    <tr >
      <td>  <img :src="stock.image"style="width: 100px; height: 100px;"></td>
      <td> </br> </br>{{stock.name}}</td>
      <td> </br></br> {{stock.number}}</td>
      <td> </br></br> {{stock.price}}</td>
   </tr>
      </tbody>
</table>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      final2: ''
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
