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
                  <input
                      :class="$v.form.surname.$error ? 'is-invalid' : ''"
                      id="surname" type="text" autocomplete="off" v-model.trim="form.surname">
                  <p v-if="$v.form.surname.$error"
                     class="invalid-feedback">Поле обязательное</p>
                </div>
                <div>
                  <label for="name">Имя</label>
                  <input :class="$v.form.name.$error ? 'is-invalid' : ''"
                         id="name" type="text" autocomplete="off" v-model.trim="form.name">
                  <p v-if="$v.form.name.$error"
                     class="invalid-feedback">Поле обязательное</p>
                </div>
              </div>
              <label for="middle-name">Отчество</label>
              <input id="middle-name" type="text" autocomplete="off" v-model.trim="form.middleName">
              <label for="birthday">Дата рождения</label>
              <input :class="$v.form.birthday.$error ? 'is-invalid' : ''"
                     id="birthday" type="date" v-model="form.birthday">
              <p v-if="$v.form.birthday.$error"
                 class="invalid-feedback">Поле обязательное</p>
              <label for="phone">Номер телефона</label>
              <input :class="$v.form.phone.$error ? 'is-invalid' : ''"
                     id="phone" type="tel" v-model="form.phone"
                     placeholder="7">
              <p v-if="$v.form.phone.$error"
                 class="invalid-feedback">Поле обязательное</p>
              <p v-if="!$v.form.phone.minLength"
                 class="invalid-feedback">11 цифр</p>
              <label for="sex">Пол</label>
              <select id="sex" v-model="form.selectedSex">
                <option v-for="(item, index) in formState.sex" :value="item.value" :key='index'>
                  {{ item.label }}
                </option>
              </select>
              <label for="customer-group">Группа клиентов</label>
              <select :class="$v.form.selectedClientGroup.$error ? 'is-invalid' : ''"
                      id="customer-group" multiple v-model="form.selectedClientGroup">
                <option
                    v-for="(item, index) in formState.clientGroup" :value="item.value" :key='index'>
                  {{ item.label }}
                </option>
              </select>
              <p v-if="$v.form.selectedClientGroup.$error"
                 class="invalid-feedback">Поле обязательное</p>
              <label for="primary-doctor">Лечащий врач.</label>
              <select name="primary-doctor" id="primary-doctor" v-model="form.selectedPrimaryDoctor">
                <option v-for="(item, index) in formState.primaryDoctors" :value="item.value" :key='index'>
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
                <option v-for="(item, index) in formState.documentType" :value="item.value" :key='index'>
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
            <button
                v-if="step !== totalSteps"
                class="nextBtn"
                @click.prevent="nextStep">Далее
            </button>
            <button
                v-if="step === totalSteps"
                class="nextBtn"
                @click.prevent="openModal">Проверить
            </button>
          </div>
        </form>
      </div>
    </main>
  </div>
</template>

<script>
import {validationMixin} from 'vuelidate'
import {required, minLength} from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],
  data() {
    return {
      form: {
        name: '',
        surname: '',
        middleName: '',
        birthday: '',
        phone: '',
        selectedSex: 'value not set',
        selectedClientGroup: [],
        selectedPrimaryDoctor: '',
        noSendSms: false,
        cityIndex: '',
        country: '',
        area: '',
        city: '',
        street: '',
        home: '',
        selectedDocumentType: '',
        series: '',
        documentNumber: '',
        issuedBy: ''
      },

      formState: {
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
      },

      step: 1,
      totalSteps: 3
    }
  },

  validations: {
    form: {
      surname: {
        required
      },
      name: {
        required
      },
      birthday: {
        required
      },
      phone: {
        required,
        minLength: minLength(11)
      },
      selectedClientGroup: {
        required
      }
    }
  },

  // this.$v.form.$touch(),

  methods: {
    nextStep() {
      if (this.checkForm()) {
        this.checkForm()
      } else {
        this.step++
      }
    },
    prevStep() {
      this.step--
    },
    openModal() {
      alert('Нужно проверить данные!')
    },
    checkForm() {
      this.$v.form.$touch()
      if (this.$v.form.$error) {
        console.log('Валидация прошла успешно')
        return true
      }
    }
  }
}
// import './SignUp.scss'
</script>

<style lang="scss">
@import "SignUp";
</style>