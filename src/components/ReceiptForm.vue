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
              v-model="form.receiptType"
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
              v-model="form.receiptType"
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
            :class="{ 'is-invalid': v$.form.firstname.$error }"
            id="name"
            name="name"
            placeholder="example name"
            v-model="form.firstname"
            @input="v$.form.firstname.$touch()"
          />
          <label for="floatingInput">İsim</label>
          <div
            v-if="
              v$.form.firstname.required.$invalid && v$.form.firstname.$error
            "
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
            :class="{ 'is-invalid': v$.form.lastname.$error }"
            v-model="form.lastname"
            @input="v$.form.lastname.$touch()"
          />
          <label for="floatingInput">Soyisim</label>
          <div
            v-for="error of v$.form.lastname.$errors"
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
            :class="{ 'is-invalid': v$.form.email.$error }"
            v-model.trim="form.email"
            @blur="v$.form.email.$touch()"
          />
          <label for="floatingInput">Email</label>
          <div
            v-if="v$.form.email.required.$invalid && v$.form.email.$error"
            class="invalid-feedback"
          >
            Lütfen email adresi giriniz!
          </div>
          <div
            v-if="v$.form.email.email.$invalid && v$.form.email.$error"
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
            v-model.number="form.phone"
            :class="{ 'is-invalid': v$.form.phone.$error }"
            @blur="v$.form.phone.$touch()"
          />
          <label for="floatingInput">Telefon</label>
          <div
            v-if="
              (v$.form.phone.required.$invalid ||
                v$.form.phone.minLength.$invalid) &&
              v$.form.phone.$error
            "
            class="invalid-feedback"
          >
            Lütfen geçerli telefon numarası giriniz!
          </div>
          <div
            v-if="v$.form.phone.numeric.$invalid && v$.form.phone.$error"
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
            v-model="form.city"
            :class="{ 'is-invalid': v$.form.city.$error }"
            @blur="v$.form.city.$touch()"
            @change="selectCity()"
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
          <div
            v-if="v$.form.city.required.$invalid && v$.form.city.$error"
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
            v-model="form.district"
            :class="{ 'is-invalid': v$.form.district.$error }"
            @blur="v$.form.district.$touch()"
          >
            <option disabled>İlçe Seçiniz</option>
            <option
              v-for="district in districts"
              :key="district"
              v-bind:value="district"
            >
              {{ district }}
            </option>
          </select>
          <label for="floatingSelect">İlçe</label>
          <div
            v-if="v$.form.district.required.$invalid && v$.form.district.$error"
            class="invalid-feedback"
          >
            Lütfen ilçe seçiniz
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating">
          <textarea
            class="form-control"
            placeholder="example address"
            id="address"
            style="height: 100px"
            v-model="form.address"
            :class="{ 'is-invalid': v$.form.address.$error }"
            @blur="v$.form.address.$touch()"
          ></textarea>
          <label for="floatingTextarea2">Adres</label>
          <div
            v-if="v$.form.address.required.$invalid && v$.form.address.$error"
            class="invalid-feedback"
          >
            Lütfen adres bilgisi giriniz
          </div>
        </div>
      </div>
      <div class="col-12 mt-5 d-flex justify-content-end">
        <button type="submit" class="btn btn-primary">Gönder</button>
      </div>
      <div class="alert alert-success mt-4" role="alert" v-if="success">
        İşlem Başarılı
      </div>
    </form>
  </div>
</template>

<script>
import citys from "../helper/citys.json";
import useVuelidate from "@vuelidate/core";
import { required, email, minLength, numeric } from "@vuelidate/validators";
import "bootstrap-icons/font/bootstrap-icons.css";
import axios from "axios";

export default {
  name: "ReceiptForm",
  setup() {
    return { v$: useVuelidate() };
  },
  data() {
    return {
      cityList: citys,
      districts: "",
      form: {
        firstname: "",
        lastname: "",
        email: "",
        phone: "",
        city: null,
        district: "",
        address: "",
        receiptType: "personal",
      },
      success: false,
      isSubmit: false,
    };
  },
  methods: {
    submit() {
      var self = this;
      self.isSubmit = true;
      self.v$.$touch();
      if (self.v$.$pendding || self.v$.$error) return;

      axios
        .post("https://jsonplaceholder.typicode.com/posts", self.form, {
          headers: {
            "Content-type": "application/json; charset=UTF-8",
          },
        })
        .then((response) => (self.success = response.data ? true : false))
        .catch((error) => {
          console.log(error);
          self.success = false;
        })
    },
    selectCity() {
      var self = this;
      let cityId = self.form.city;
      let cityList = self.cityList;
      let selectCity = cityList.find((element) => element.plaka == cityId);
      self.districts = selectCity.ilceleri;
    },
  },
  validations() {
    return {
      form: {
        firstname: { required, minLength: minLength(2) },
        lastname: { required },
        email: { required, email },
        phone: { required, numeric, minLength: minLength(3) },
        city: { required },
        district: { required },
        address: { required },
        receiptType: { required },
      },
    };
  },
};
</script>

<style scoped>
.receipt {
  width: 700px;
  box-shadow: rgb(50 50 93 / 25%) 0px 50px 100px -20px,
    rgb(0 0 0 / 30%) 0px 30px 60px -30px;
  padding: 24px;
  border-radius: 4px;
  margin-left: auto;
  margin-right: auto;
  background: #fff;
}
.btn-primary {
  background: #ff6363;
  border-color: #ff6363;
  transition: all 0.2s ease;
}
.btn-primary:active,
.btn-primary:hover {
  background: #f04c4c;
  border-color: #f04c4c;
  transform: translateY(-2px);
}
.btn-primary:focus {
  color: #fff;
  background-color: #ff6363;
  border-color: #ff6363;
  box-shadow: 0 0 0 0.25rem rgb(255 99 99 / 50%);
}
.form-check-input:checked {
  background-color: #ff6363;
  border-color: #ff6363;
}
.form-check-input:focus {
  box-shadow: 0 0 0 0.25rem rgb(255 99 99 / 25%);
}
</style>