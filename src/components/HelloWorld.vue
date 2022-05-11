<script>
export default {

  data() {
    return {
      varName: '',
      varValue: '',
      variables: []
    }
  },

  props: {

  },

  methods: {
    onSend() {
      fetch(process.env.API_DOMAIN + "/set/" + this.varName + "?value=" + this.varValue, {
        method: "POST"
      }).then(resp => {
        console.log(resp);
      }).catch(err => {
        alert(err);
      });
      this.varName = '';
      this.varValue = '';
    },
    isFormEmpty() {
      return this.varName.length === 0 || this.varValue.length === 0;
    },
    updateTable() {
      fetch(process.env.API_DOMAIN + "/all",{
        method: "GET"
      }).then(resp => {
        resp.text()
            .then(txt => this.variables = JSON.parse(txt))
            .catch(err => alert(err));
      }).catch(err => {
        alert(err);
      })
    }
  }
}

</script>

<template>
  <div class="container" style="max-width: 700px">
    <div>
      <table class="table table-hover">
        <thead>
        <tr>
          <th scope="col">Name</th>
          <th scope="col">Value</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="item of variables" :key="item.value">
          <th>{{item.name}}</th>
          <th>{{item.value}}</th>
        </tr>
        </tbody>
      </table>
    </div>
    <div class="col-auto">
      <button class="btn btn-primary" @click="updateTable">Обновить</button>
    </div>
    <hr/>
    <div>
      <form @submit.prevent="onSend">
        <div class="row p-3 justify-content-center align-items-center">
          <h4>Добаить переменную</h4>
        </div>
        <div class="row p-3 justify-content-center">
          <div class="col-auto">
            <input v-model="varName" type="text" class="form-control" id="variableName" placeholder="Variable name"/>
          </div>
          <div class="col-auto align-self-center">
            <b>:</b>
          </div>
          <div class="col-auto">
            <input v-model="varValue" type="text" class="form-control" id="variableValue" placeholder="Variable value"/>
          </div>
        </div>
        <div class="row p-3 justify-content-center align-items-center">
          <div class="col-auto">
            <p>
              <kbd>{{ varName }}</kbd>:<kbd>{{ varValue }}</kbd>
            </p>
          </div>
        </div>
        <div class="col-auto">
          <button class="btn btn-primary" type="submit" :disabled="isFormEmpty()">Обновить значение</button>
        </div>
      </form>
    </div>
  </div>
</template>