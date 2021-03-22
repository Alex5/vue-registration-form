<template>
  <div class="main-container">
    <aside class="presentation">
      <div class="presentation__header">
        <h1 class="description__text">Регистрация нового пользователя</h1>
      </div>
      <img src="../assets/img/Account-amico.svg" alt="">
    </aside>
    <main class="registration__wrapper">
      <div class="registration__block">
        <div class="registration__header ">
          <h2 v-if="step === 1" class="header__descr">Шаг 1. Личные данные</h2>
          <h2 v-if="step === 2" class="header__descr">Шаг 2. Адресные данные</h2>
          <h2 v-if="step === 3" class="header__descr">Шаг 3. Паспортные данные</h2>
        </div>
        <form class="registration__form">
          <section v-if="step === 1">
            <div class="form_field">
              <div class="form_field--group">
                <div>
                  <label for="surname">Фамилия</label>
                  <input id="surname" type="text" autocomplete="off" v-model.trim="form.surname">
                </div>
                <div>
                  <label for="name">Имя</label>
                  <input id="name" type="text" autocomplete="off" v-model.trim="form.name">
                </div>
              </div>
              <label for="middle-name">Отчество</label>
              <input id="middle-name" type="text" autocomplete="off" v-model.trim="form.middleName">
              <label for="birthday">Дата рождения</label>
              <input id="birthday" type="date" v-model="form.birthday">
              <label for="phone">Номер телефона</label>
              <input id="phone" type="tel" pattern="2[0-9]{3}-[0-9]{3}" v-model="form.phone">
              <label for="sex">Пол</label>
              <select id="sex" v-model="form.selectedSex">
                <option v-for="(item, index) in form.sex" :value="item.value" :key='index'>
                  {{ item.label }}
                </option>
              </select>
              <label for="customer-group">Группа клиентов</label>
              <select id="customer-group" multiple v-model="form.selectedClientGroup">
                <option v-for="(item, index) in form.clientGroup" :value="item.value" :key='index'>
                  {{ item.label }}
                </option>
              </select>
              <label for="primary-doctor">Лечащий врач.</label>
              <select name="primary-doctor" id="primary-doctor" v-model="form.selectedPrimaryDoctor">
                <option v-for="(item, index) in form.primaryDoctors" :value="item.value" :key='index'>
                  {{ item.label }}
                </option>
              </select>
              <div class="form_field--sms">
                <label for="sms">Не отправлять СМС</label>
                <input type="checkbox" id="sms" name="sms" v-model="form.noSendSms">
              </div>
            </div>
          </section>
          <section v-if="step === 2">
            <div class="form_field">
              <label for="index">Индекс</label>
              <input id="index" type="text" autocomplete="off" v-model.trim="form.cityIndex">
              <label for="country">Страна</label>
              <input id="country" type="text" autocomplete="off" v-model.trim="form.country">
              <label for="area">Область</label>
              <input id="area" type="text" autocomplete="off" v-model.trim="form.area">
              <label for="city">Город</label>
              <input id="city" type="text" autocomplete="off" v-model.trim="form.city">
              <label for="street">Улица</label>
              <input id="street" type="text" autocomplete="off" v-model.trim="form.street">
              <label for="home">Дом</label>
              <input id="home" type="number" autocomplete="off" v-model.trim="form.home">
            </div>
          </section>
          <section v-if="step === 3">
            <div class="form_field">
              <label for="documentType">Тип документа</label>
              <select id="documentType" v-model.trim="form.selectedDocumentType">
                <option v-for="(item, index) in form.documentType" :value="item.value" :key='index'>
                  {{ item.label }}
                </option>
              </select>
              <label for="series">Серия</label>
              <input id="series" type="text" autocomplete="off" v-model.trim="form.series">
              <label for="documentNumber">Номер</label>
              <input id="documentNumber" type="number" autocomplete="off" v-model.trim="form.documentNumber">
              <label for="issuedBy">Кем выдан</label>
              <textarea id="issuedBy" autocomplete="off" v-model.trim="form.issuedBy"></textarea>
              <label for="dateOfIssue">Дата выдачи</label>
              <input type="date" id="dateOfIssue">
            </div>
          </section>
          <div class="form-buttons">
            <button v-if="step !== 1" class="prevBtn" @click.prevent="prevStep">Назад</button>
            <button v-if="step !== totalSteps" class="nextBtn" @click.prevent="nextStep">Далее</button>
            <button v-if="step === totalSteps" class="nextBtn" @click.prevent="openModal">Проверить</button>
          </div>
        </form>

      </div>
    </main>
  </div>
</template>

<script>

export default {
  data() {
    return {
      form: {
        name: '',
        surname: '',
        middleName: '',
        birthday: '',
        phone: '',
        selectedSex: 'value not set',
        sex: [
          {
            label: 'Мужской',
            value: 'male'
          }, {
            label: 'Женский',
            value: 'female'
          },
          {
            label: 'Другой',
            value: 'another'
          }
        ],
        selectedClientGroup: [],
        clientGroup: [
          {
            label: 'VIP',
            value: 'vip'
          }, {
            label: 'Проблемные',
            value: 'problematic'
          },
          {
            label: 'ОМС',
            value: 'omc'
          }
        ],
        selectedPrimaryDoctor: '',
        primaryDoctors: [
          {
            label: 'Иванов',
            value: 'ivanov'
          }, {
            label: 'Захаров',
            value: 'zaharov'
          },
          {
            label: 'Чернышева',
            value: 'chernysheva'
          }
        ],
        noSendSms: false,
        cityIndex: '',
        country: '',
        area: '',
        city: '',
        street: '',
        home: '',
        selectedDocumentType : '',
        documentType: [
          {
            label: 'Паспорт',
            value: 'passport'
          }, {
            label: 'Свидетельство о рождении',
            value: 'birthСertificate '
          },
          {
            label: 'Вод. удостоверение',
            value: 'driversLicence'
          }
        ],
        series: '',
        documentNumber: '',
        issuedBy: ''
      },

      step: 1,
      totalSteps: 3
    }
  },

  methods: {
    nextStep: function () {
      this.step++
    },
    prevStep: function () {
      this.step--
    },
    openModal: function () {
      alert('Нужно проверить данные!')
    }
  }
}
// import './SignUp.scss'
</script>

<style lang="scss">
@import "SignUp";
</style>