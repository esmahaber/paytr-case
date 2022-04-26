<template>
  <div class="container">
    <form
      class="row receipt needs-validation"
      novalidate
      @submit.prevent="submit"
    >
      <div class="row ml-1">
        <div class="col-3 mb-2">
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="receiptType"
              id="personalReceipt"
              value="personal"
              v-model="receiptType"
            />
            <label class="form-check-label" for="flexRadioDefault1">
              Bireysel Fatura
            </label>
          </div>
        </div>
        <div class="col-3 mb-2">
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="receiptType"
              id="companyReceipt"
              value="company"
              v-model="receiptType"
            />
            <label class="form-check-label" for="flexRadioDefault2">
              Şirket Faturası
            </label>
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3 has-validation">
          <input
            type="text"
            class="form-control"
            :class="{ 'is-invalid': v$.name.$error }"
            id="name"
            name="name"
            placeholder="example name"
            v-model="name"
            @input="v$.name.$touch()"
          />
          <label for="floatingInput">İsim</label>
          <div
            v-for="error of v$.name.$errors"
            :key="error.$uid"
            class="invalid-feedback"
          >
            Lütfen geçerli isim giriniz!
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3 has-validation">
          <input
            type="text"
            class="form-control"
            id="floatingInput"
            name="lastName"
            placeholder="example lastname"
            :class="{ 'is-invalid': v$.lastname.$error }"
            v-model="lastname"
            @input="v$.lastname.$touch()"
          />
          <label for="floatingInput">Soyisim</label>
          <div
            v-for="error of v$.lastname.$errors"
            :key="error.$uid"
            class="invalid-feedback"
          >
            Lütfen soyisim giriniz
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3 has-validation">
          <input
            type="email"
            class="form-control"
            id="floatingInput"
            placeholder="name@example.com"
            :class="{ 'is-invalid': v$.email.$error }"
            v-model.trim="email"
            @blur="v$.email.$touch()"
          />
          <label for="floatingInput">Email</label>
          <div
            v-if="v$.email.required.$invalid && v$.email.$error"
            class="invalid-feedback"
          >
            Lütfen email adresi giriniz!
          </div>
          <div
            v-if="v$.email.email.$invalid && v$.email.$error"
            class="invalid-feedback"
          >
            Lütfen geçerli bir email adresi giriniz!
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <input
            type="number"
            class="form-control"
            id="phone"
            placeholder="example number"
            v-model.number="phone"
            :class="{ 'is-invalid': v$.phone.$error }"
            @blur="v$.phone.$touch()"
          />
          <label for="floatingInput">Telefon</label>
          <div
            v-if="
              (v$.phone.required.$invalid || v$.phone.minLength.$invalid) &&
              v$.phone.$error
            "
            class="invalid-feedback"
          >
            Lütfen geçerli telefon numarası giriniz!
          </div>
          <div
            v-if="v$.phone.numeric.$invalid && v$.phone.$error"
            class="invalid-feedback"
          >
            Lütfen telefon numaranızı kontrol ediniz!
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <select
            class="form-select"
            id="floatingSelect"
            aria-label="Floating label select example"
            v-model="city"
            :class="{ 'is-invalid': v$.phone.$error }"
            @blur="v$.city.$touch()"
          >
            <option disabled>Şehir Seçiniz</option>
            <option
              v-for="city in cityList"
              :key="city.plaka"
              v-bind:value="city.plaka"
            >
              {{ city.il }}
            </option>
          </select>
          <label for="floatingSelect">Şehir</label>
          <div class="invalid-feedback">Lütfen şehir seçiniz</div>
          <div
            v-if="v$.city.required.$invalid && v$.city.$error"
            class="invalid-feedback"
          >
            Lütfen şehir seçiniz
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <select
            class="form-select"
            id="floatingSelect"
            aria-label="Floating label select example"
            v-model="district"
          >
            <option disabled>İlçe Seçiniz</option>
            <option></option>
          </select>
          <label for="floatingSelect">İlçe</label>
          <div class="invalid-feedback">Lütfen ilçe seçiniz</div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating">
          <textarea
            class="form-control"
            placeholder="example address"
            id="address"
            style="height: 100px"
            v-model="address"
            :class="{ 'is-invalid': v$.address.$error }"
            @blur="v$.address.$touch()"
          ></textarea>
          <label for="floatingTextarea2">Adres</label>
          <div
            v-if="v$.address.required.$invalid && v$.address.$error"
            class="invalid-feedback"
          >
            Lütfen adres bilgisi giriniz
          </div>
        </div>
      </div>
      <div class="col-12 mt-5 d-flex justify-content-end">
        <button type="submit" class="btn btn-primary">Gönder</button>
      </div>
    </form>
  </div>
</template>

<script>
import citys from "../helper/citys.json";
import useVuelidate from "@vuelidate/core";
import { required, email, minLength, numeric } from "@vuelidate/validators";
import 'bootstrap-icons/font/bootstrap-icons.css'

export default {
  name: "ReceiptForm",
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      cityList: citys,
      districts: [],
      name: "",
      lastname: "",
      email: "",
      phone: "",
      city: "",
      district: "",
      address: "",
      receiptType: "personal",
    };
  },
  methods: {
    submit: function () {
      var self = this;

      self.$v.$touch();
      if (this.$v.$pedding || this.$v.$error) return;
    },
  },
  validations() {
    return {
      name: { required, minLength: minLength(2) }, // Matches this.firstName
      lastname: { required }, // Matches this.lastName
      email: { required, email }, // Matches this.contact.email
      phone: { required, numeric, minLength: minLength(3) },
      city: { required },
      district: { required },
      address: { required },
      receiptType: { required },
    };
  },
};
</script>
<style scoped>
  .receipt{
    width: 700px;
    box-shadow: rgb(50 50 93 / 25%) 0px 50px 100px -20px, rgb(0 0 0 / 30%) 0px 30px 60px -30px;
    padding: 24px;
    border-radius: 4px;
    margin-left: auto;
    margin-right: auto;
    background: #fff;
  }
  .btn-primary{
    background: #FF6363;
    border-color:#FF6363;
    transition: all .2s ease;
  }
  .btn-primary:active, .btn-primary:hover{
    background: #f04c4c;
    border-color:#f04c4c;
    transform: translateY(-2px);
  }
  .btn-primary:focus {
    color: #fff;
    background-color: #FF6363;
    border-color: #FF6363;
    box-shadow: 0 0 0 0.25rem rgb(255 99 99 / 50%);
  } 
  .form-check-input:checked {
    background-color: #FF6363;
    border-color: #FF6363;
  }
  .form-check-input:focus {
    box-shadow: 0 0 0 0.25rem rgb(255 99 99 / 25%);
  }

</style>