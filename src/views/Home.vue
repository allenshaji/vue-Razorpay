<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <form>
          <div class="form-group">
            <label>Name</label>
            <input v-model="name" type="text" class="form-control" />
          </div>
          <div class="form-group">
            <label>Description</label>
            <input v-model="desc" type="text" class="form-control" />
          </div>

          <div class="form-group">
            <label>Price</label>
            <input v-model="price" type="number" class="form-control" />
          </div>

          <button @click="makePayment" class="btn btn-success">Submit</button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "home",
  components: {},
  data() {
    return {
      price: 0,
      order_id: "",
      name: "",
      desc: ""
    };
  },
  methods: {
    //Function to create order ID
    makePayment: function(ev) {
      ev.preventDefault();
      var data = {};
      (data["name"] = this.name),
        (data["desc"] = this.desc),
        (data["amount"] = 500),
        axios({
          method: "post",
          url: "http://YOUR URL TO CREATE ORDER ID/",
          data: data,
        })
          .then(res => {
            this.order_id = res.data.order_id;
            //Initiation of Razorpay PG
            var rzp1 = new Razorpay({
              key: "--RAZORPAY KEY--",
              amount: res.data.amount,
              name: res.data.name,
              currency: "INR",
              description: res.data.desc,
              image: "Link Of Image",

              //Uncomment if you are using handler function

              // "handler": function (response){
              //   alert(response.razorpay_payment_id);
              // },
              //callback_url: 'http://13.126.183.214/razorpay/checkstatus/?orderid='+this.order_id,

              prefill: {
                name: "Allen Shaji",
                email: "allenshaji10@gmail.com"
              },

              notes: {
                address: ""
              },

              theme: {
                color: "#00ffff"
              },
              order_id: res.data.order_id,
              callback_url: "http://domain.com/#/about/" + this.order_id,
              redirect: true
            });
            rzp1.open();
          })
          .catch(err => {
            console.log("ERR", err);
          });
    }
    //if you are using handler function

    // verify: function(response) {

    // }
  }
};
</script>

