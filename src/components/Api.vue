<template>
  <div>
    <form @submit.prevent="save">
      <input type="hidden" v-model="form.id" name="" value="" />
      <input type="text" v-model="form.name" name="" value="" />
      <input type="number" v-model="form.amount" name="" value="" /><br />
      <button type="submit" v-show="!updateSubmit" name="button">submit</button>
      <button
        type="submit"
        v-show="updateSubmit"
        v-on:click="update(form)"
        name="button"
      >
        update
      </button>
    </form>

    <!---
      <h1>Todo List</h1>
       <ul v-for="item in list" :key="item.id">
         <li>{{item.name}} <br/>
         <button type="button" v-on:click="edit(item)">Edit</button> ||
          <button type="button" v-on:click="Delete(item)" >delete</button>

         </li>

       </ul>
       -->
    <div class="expenses">
      <div class="balance">
        <span>Balance</span>
        <p>{{ expences.balance }}</p>
      </div>
      <div class="balance">
        <span>income</span>
        <p>{{ expences.income }}</p>
      </div>
      <div class="balance">
        <span>Expense</span>
        <p>{{ expences.expence }}</p>
      </div>
    </div>

    <div class="containerbox">
      <h1>List</h1>
      <table>
        <tr>
          <th>Title</th>
          <th>Amount</th>
          <th>Action</th>
        </tr>
        <tr v-for="item in list" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.amount }}</td>
          <td>
            <button type="button" v-on:click="edit(item)">Edit</button>
            <button type="button" v-on:click="Delete(item)">delete</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
import Vue from "vue";
// eslint-disable-next-line no-unused-vars
import VueAxios from "vue-axios";
import axios from "axios";

export default {
  name: "Api",

  data() {
    return {
      form: {
        id: "",
        name: "",
        amount: "",
      },
      expences: {
        balance: 0,
        income: 0,
        expence: 0,
      },
      list: [],
      updateSubmit: false,
    };
  },
  methods: {
    load() {
      axios
        .get("/todos")
        .then((res) => {
          this.list = res.data;
          // console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    save() {
      axios
        .post("/todos", this.form)

        .then(() => {
          this.load();
          //this.form.name = "";
          //this.form.amount = "";
          const amount = parseInt(this.form.amount);
          if (amount > 1) {
            this.expences = {
              ...this.expences,
              income: this.expences.income + amount,
              balance: this.expences.balance + amount,
            };
          } else if (amount < 1) {
            this.expences = {
              ...this.expences,
              expence: this.expences.expence + amount,
              balance: this.expences.balance + amount,
            };
          }
          alert("saving");
        })
        .catch(() => {
          alert("saving ");
        });
    },
    edit(item) {
      this.updateSubmit = true;
      this.form.id = item.id;
      this.form.name = item.name;
      this.form.amount = item.amount;
    },

    update(form) {
      axios
        .put("/todos/" + form.id, {
          name: this.form.name,
          amount: this.form.amount,
        })
        .then(() => {
          this.load();
          this.form.name = "";
          this.form.amount = "";
          this.updateSubmit = false;
          alert("updated");
        })
        .catch((err) => {
          console.log(err);
        });
    },
    Delete(item) {
      axios
        .delete("/todos/" + item.id)
        .then(() => {
          this.load();
          alert("deleted");
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  mounted() {
    this.load();
  },
};
</script>

<style>
.containerbox {
  text-align: center;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  margin: 0 auto;
  width: 800px;
}
td,
th {
  text-align: left;
  padding: 8px;
  border: 1px solid #c39c9c;
}
tr:nth-child(even) {
  background-color: #dddddd;
}
ul {
  list-style-type: none;
}

.expenses {
  width: 700px;
  margin: 70px auto;
  display: flex;
  justify-content: space-between;
}
.balance {
  position: relative;
  background: #ffff;
  padding: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  width: 200px;
  height: 150px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}
.balance span {
  position: absolute;
  top: 0;
  left: 0;
  background: chocolate;
  color: #fff;
  font-size: 12px;
  padding: 5px;
}
</style>
