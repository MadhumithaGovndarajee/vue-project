<template>
  <div class="main-block">
    <form>
      <h1>{{customerId ? 'Edit a Customer Information' : 'Create a Customer Information'}}</h1>
      <legend>
          <h3><a href="/customerList">View Customer List </a></h3>
      </legend>
      <fieldset>
        <legend>
          <h3>Customer Details</h3>
        </legend>
        <div class="personal-details">
          <div>
            <div>
              <label>Name*</label>
              <input @input="checkValue($event,'name')" v-model="formData.name" type="text" name="name" required />
            </div>
            <p v-if="save && !formData.name" style="text-align: center;color: red;">Please Enter Name</p>
            <p v-if="formData.name && nameError" style="text-align: center;color: red;">Please Enter Valid Name (Characters Only)</p>
            <div class="textInput">
              <label>Employee Id*</label
              ><input  @input="checkValue($event,'employeeId')"  v-model="formData.employeeId" type="number" name="employeeId" required />
            </div>
            <p v-if="save && !formData.employeeId" style="text-align: center;color: red;">Please Enter Employee Id</p>
            <p v-if="formData.employeeId && checkAleadyExitsId" style="text-align: center;color: red;">Employee Id Already Exists</p>
            <div class="textInput">
              <label>City*</label>
              <select v-model="formData.city">
                <option disabled value="">Please select City </option>

                <option v-for="option in options" :value="option.value" :key="option.value">
                  {{ option.text }}
                </option>
              </select>
            </div>
            <p v-if="save && !formData.city" style="text-align: center;color: red;">Please Enter One City</p>
          </div>
          <div>
            <div>
              <label>Gender*</label>
              <div class="gender">
                <input type="checkbox" id="male" value="Male" v-model="formData.gender" />
                <label for="male"> Male</label>

                <input type="checkbox" id="female" value="Female" v-model="formData.gender" />
                <label for="female"> Female</label>
              </div>

            </div>
            <p v-if="save && formData.gender.length ===0" style="text-align: center;color: red;">Please Select Gender</p>

          </div>
        </div>
      </fieldset>
      <input class="button" @click="getCustomerInfo()" type="button" :value="customerId ? 'Save' :'Submit'"></input>
    </form>
  </div>
</template>

<script>
import { rules } from '../components/helpers/formValidation';

export default {
  auth: false,
  layout: 'blank',
  data() {
    return {
      rules,
      formData: {
        name: '',
        employeeId: '',
        city: '',
        gender: [],
        id: '0'
      },
      options: [
        { text: 'New York', value: 'New York' },
        { text: 'London', value: 'London' },
        { text: 'Tokyo', value: 'Tokyo' }
      ],
      customerId : this.$route.params.customerId,
      nameError : false,
      checkAleadyExitsId: false,
      save: false
    }
  },
  methods: {
    getCustomerInfo() {
      this.save = true
      if(this.nameError || this.checkAleadyExitsId || !this.formData.name || !this.formData.employeeId|| this.formData.gender.length === 0|| !this.formData.city){
        return
      }
      let storeCustomerInfo = []
      if(this.customerId){
        storeCustomerInfo = JSON.parse(localStorage.getItem('customer_info'))
        storeCustomerInfo[this.customerId] = this.formData
      }else{
        if (localStorage.getItem('customer_info') !== undefined && localStorage.getItem('customer_info') !== null && localStorage.getItem('customer_info') !== '') {
          storeCustomerInfo = JSON.parse(localStorage.getItem('customer_info'))
          this.formData.id = storeCustomerInfo.length
        }
        storeCustomerInfo.push(this.formData)
      }
      this.save = false
      localStorage.setItem('customer_info', JSON.stringify(storeCustomerInfo))
      this.$router.push('/customerList')
    },
    checkValue(value,type){
      let regExp = ''
      if(type === 'name'){
        this.nameError = false
        let regExp = /^[a-zA-Z ]*$/; 
        if(!regExp.test(this.formData.name))
          this.nameError = true
      }
      if(type === 'employeeId'){
        var formDataVal = JSON.parse(localStorage.getItem('customer_info'))
        for(var i in formDataVal){
          this.checkAleadyExitsId = false
          if(this.$route.params.customerId){
              if(Number(formDataVal[i].employeeId) === Number(this.formData.employeeId) && this.formData.id !== formDataVal[i].id){
                this.checkAleadyExitsId = true
                return
              }
          }else{
            if(Number(formDataVal[i].employeeId) === Number(this.formData.employeeId)){
              this.checkAleadyExitsId = true
              return
            }
          }
        }
      }
    }
  },
  created() {
    if(this.$route.params.customerId){
      if (localStorage.getItem('customer_info') !== undefined && localStorage.getItem('customer_info') !== null && localStorage.getItem('customer_info') !== '') {
      var formDataVal = JSON.parse(localStorage.getItem('customer_info'))
      this.formData = formDataVal[this.$route.params.customerId]
      }
    }
  }
}
</script>
<style>
html,
body {
  min-height: 100%;
}
body,
div,
form,
input,
select,
p {
  padding: 0;
  margin: 0;
  outline: none;
  font-family: Roboto, Arial, sans-serif;
  font-size: 14px;
  color: #666;
}
h1 {
  margin: 0;
  font-weight: 400;
}
h3 {
  margin: 12px 0;
  color: #8ebf42;
}
.main-block {
  display: flex;
  justify-content: center;
  align-items: center;
  background: #fff;
}
form {
  width: 100%;
  padding: 20px;
}
fieldset {
  border: none;
  border-top: 1px solid #8ebf42;
}
.account-details,
.personal-details {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.account-details > div,
.personal-details > div > div {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  
}
.account-details > div,
.personal-details > div,
input,
label {
  width: 100%;
}
label {
  padding: 0 5px;
  text-align: right;
  vertical-align: middle;
}
input {
  padding: 5px;
  vertical-align: middle;
}
.checkbox {
  margin-bottom: 10px;
}
select,
.children,
.gender,
.bdate-block {
  width: calc(100% + 26px);
  padding: 5px 0;
}
select {
  background: transparent;
}
.gender input {
  width: auto;
}
.gender label {
  padding: 0 5px 0 0;
}
.bdate-block {
  display: flex;
  justify-content: space-between;
}
.birthdate select.day {
  width: 35px;
}
.birthdate select.mounth {
  width: calc(100% - 94px);
}
.birthdate input {
  width: 38px;
  vertical-align: unset;
}
.checkbox input,
.children input {
  width: auto;
  margin: -2px 10px 0 0;
}
.checkbox a {
  color: #8ebf42;
}
.checkbox a:hover {
  color: #82b534;
}
.button {
  width: 100%;
  padding: 10px 0;
  margin: 10px auto;
  border-radius: 5px;
  border: none;
  background: #8ebf42;
  font-size: 14px;
  font-weight: 600;
  color: #fff;
}
.button:hover {
  background: #82b534;
}
@media (min-width: 568px) {
  .account-details > div,
  .personal-details > div {
    width: 50%;
  }
  label {
    width: 40%;
  }
  input {
    width: 60%;
  }
  select,
  .children,
  .gender,
  .bdate-block {
    width: calc(60% + 16px);
  }
  .textInput{
    padding: 10px 0;

  }
}
</style>
