<template>
  <div class="container">
    <form class="row receipt">
      <div class="row ml-1">
        <div class="col-3 mb-2">
          <div class="form-check">
            <input
              class="form-check-input"
              type="radio"
              name="receiptType"
              id="personalReceipt"
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
            />
            <label class="form-check-label" for="flexRadioDefault2">
              Şirket Faturası
            </label>
          </div>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <input
            type="text"
            class="form-control"
            id="name"
            name="name"
            placeholder="example name"
          />
          <label for="floatingInput">İsim</label>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <input
            type="text"
            class="form-control"
            id="floatingInput"
            name="lastName"
            placeholder="example lastname"
          />
          <label for="floatingInput">Soyisim</label>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <input
            type="email"
            class="form-control"
            id="floatingInput"
            placeholder="name@example.com"
          />
          <label for="floatingInput">Email</label>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <input
            type="number"
            class="form-control"
            id="number"
            placeholder="example number"
          />
          <label for="floatingInput">Telefon</label>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <select
            class="form-select"
            id="floatingSelect"
            aria-label="Floating label select example"
          >
            <option selected>Şehir Seçiniz</option>
            <option v-for="city in citys" :key="city.plaka">
              {{ city.il }}
            </option>
          </select>
          <label for="floatingSelect">Şehir</label>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating mb-3">
          <select
            class="form-select"
            id="floatingSelect"
            aria-label="Floating label select example"
          >
            <option selected>İlçe Seçiniz</option>
            <option></option>
          </select>
          <label for="floatingSelect">İlçe</label>
        </div>
      </div>
      <div class="col-6">
        <div class="form-floating">
          <textarea
            class="form-control"
            placeholder="example address"
            id="address"
            style="height: 100px"
          ></textarea>
          <label for="floatingTextarea2">Adres</label>
        </div>
      </div>
      <div class="col-12 mt-5 d-flex justify-content-end">
          <button type="button" class="btn btn-primary">Gönder</button>
      </div>
    </form>
  </div>
</template>

<script>
import citys from "../helper/citys.json";

console.log(citys);
export default {
  name: "ReceiptForm",
  data() {
    return {
      citys: citys,
      district: [],
    };
  },
};
</script>