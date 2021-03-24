<template>
  <div class="main-container">
    <div v-if="isDone" class="form-container">
      <aside class="presentation">
        <div class="presentation__header">
          <h1 class="description__text">Регистрация нового пользователя</h1>
          <h1 v-if="step === 4" class="description__text alert">Проверьте правильность данных!</h1>
        </div>
        <img :src="step === 4
      ? require('../assets/img/done.svg')
      : require('../assets/img/account.svg')">
      </aside>
      <main class="registration__wrapper">
        <div class="registration__block">
          <div class="registration__header ">
            <h2 v-if="step === 1" class="header__descr">Шаг 1. Личные данные</h2>
            <h2 v-if="step === 2" class="header__descr">Шаг 2. Адресные данные</h2>
            <h2 v-if="step === 3" class="header__descr">Шаг 3. Паспортные данные</h2>
            <h2 v-if="step === 4" class="header__descr">Шаг 4. Проверка данных</h2>
          </div>
          <form ref="registrationForm" class="registration__form">
            <section v-if="step === 1">
              <div class="form_field">
                <div class="form_field--group">
                  <div>
                    <label for="surname">Фамилия</label>
                    <input
                        :class="$v.form.personalData.surname.$error ? 'is-invalid' : ''"
                        id="surname" type="text" autocomplete="off" v-model.trim="form.personalData.surname">
                    <p v-if="$v.form.personalData.surname.$error"
                       class="invalid-feedback">Поле обязательное</p>
                  </div>
                  <div>
                    <label for="name">Имя</label>
                    <input :class="$v.form.personalData.name.$error ? 'is-invalid' : ''"
                           id="name" type="text" autocomplete="off" v-model.trim="form.personalData.name">
                    <p v-if="$v.form.personalData.name.$error"
                       class="invalid-feedback">Поле обязательное</p>
                  </div>
                </div>
                <label for="middle-name">Отчество</label>
                <input id="middle-name" type="text" autocomplete="off" v-model.trim="form.personalData.middleName">
                <label for="birthday">Дата рождения</label>
                <input :class="$v.form.personalData.birthday.$error ? 'is-invalid' : ''"
                       id="birthday" type="date" v-model="form.personalData.birthday">
                <p v-if="$v.form.personalData.birthday.$error"
                   class="invalid-feedback">Поле обязательное</p>
                <label for="phone">Номер телефона</label>
                <input :class="$v.form.personalData.phone.$error ? 'is-invalid' : ''"
                       id="phone" type="tel" v-model="form.personalData.phone"
                       placeholder="7">
                <p v-if="$v.form.personalData.phone.$error"
                   class="invalid-feedback">Поле обязательное</p>
                <p v-if="!$v.form.personalData.phone.minLength"
                   class="invalid-feedback">11 цифр</p>
                <label for="sex">Пол</label>
                <select id="sex" v-model="form.personalData.selectedSex">
                  <option v-for="(item, index) in formState.sex" :value="item.value" :key='index'>
                    {{ item.label }}
                  </option>
                </select>
                <label for="customer-group">Группа клиентов</label>
                <select :class="$v.form.personalData.selectedClientGroup.$error ? 'is-invalid' : ''"
                        id="customer-group" multiple v-model="form.personalData.selectedClientGroup">
                  <option
                      v-for="(item, index) in formState.clientGroup" :value="item.value" :key='index'>
                    {{ item.label }}
                  </option>
                </select>
                <p v-if="$v.form.personalData.selectedClientGroup.$error"
                   class="invalid-feedback">Поле обязательное</p>
                <label for="primary-doctor">Лечащий врач.</label>
                <select name="primary-doctor" id="primary-doctor" v-model="form.personalData.selectedPrimaryDoctor">
                  <option v-for="(item, index) in formState.primaryDoctors" :value="item.value" :key='index'>
                    {{ item.label }}
                  </option>
                </select>
                <div class="form_field--sms">
                  <label for="sms">Не отправлять СМС</label>
                  <input type="checkbox" id="sms" name="sms" v-model="form.personalData.noSendSms">
                </div>
              </div>
            </section>
            <section v-if="step === 2">
              <div class="form_field">
                <label for="index">Индекс</label>
                <input id="index" type="text" autocomplete="off" v-model.trim="form.addressData.cityIndex">
                <label for="country">Страна</label>
                <input id="country" type="text" autocomplete="off" v-model.trim="form.addressData.country">
                <label for="area">Область</label>
                <input id="area" type="text" autocomplete="off" v-model.trim="form.addressData.area">
                <label for="city">Город</label>
                <input :class="$v.form.addressData.city.$error ? 'is-invalid' : ''"
                       id="city" type="text" autocomplete="off" v-model.trim="form.addressData.city">
                <p v-if="$v.form.addressData.city.$error"
                   class="invalid-feedback">Поле обязательное</p>
                <label for="street">Улица</label>
                <input id="street" type="text" autocomplete="off" v-model.trim="form.addressData.street">
                <label for="home">Дом</label>
                <input id="home" type="number" autocomplete="off" v-model.trim="form.addressData.home">
              </div>
            </section>
            <section v-if="step === 3">
              <div class="form_field">
                <label for="documentType">Тип документа</label>
                <select :class="$v.form.passportData.selectedDocumentType.$error ? 'is-invalid' : ''"
                        id="documentType" v-model.trim="form.passportData.selectedDocumentType">
                  <option v-for="(item, index) in formState.documentType" :value="item.value" :key='index'>
                    {{ item.label }}
                  </option>
                </select>
                <p v-if="$v.form.passportData.selectedDocumentType.$error"
                   class="invalid-feedback">Поле обязательное</p>
                <label for="series">Серия</label>
                <input id="series" type="text" autocomplete="off" v-model.trim="form.passportData.series">
                <label for="documentNumber">Номер</label>
                <input id="documentNumber" type="number" autocomplete="off"
                       v-model.trim="form.passportData.documentNumber">
                <label for="issuedBy">Кем выдан</label>
                <textarea id="issuedBy" autocomplete="off" v-model.trim="form.passportData.issuedBy"></textarea>
                <label for="dateOfIssue">Дата выдачи</label>
                <input :class="$v.form.passportData.dateOfIssue.$error ? 'is-invalid' : ''"
                       type="date" id="dateOfIssue" v-model.trim="form.passportData.dateOfIssue">
                <p v-if="$v.form.passportData.dateOfIssue.$error"
                   class="invalid-feedback">Поле обязательное</p>
              </div>
            </section>
            <section v-if="step === 4" class="check-data">
              <h3>Данные формы: </h3>
              <h5>Личные данные </h5>
              <ul>
                <li>Фамилия: {{ form.personalData.name || `Значение не выбрано` }}</li>
                <li>Имя: {{ form.personalData.surname || `Значение не выбрано` }}</li>
                <li>Отчество: {{ form.personalData.middleName || `Значение не выбрано` }}</li>
                <li>День рождение: {{ form.personalData.birthday || `Значение не выбрано` }}</li>
                <li>Телефон: {{ form.personalData.phone || `Значение не выбрано` }}</li>
                <li>Пол: <b>{{ form.personalData.sex || `Значение не выбрано` }}</b></li>
                <li>Группа клиентов: {{ form.personalData.selectedClientGroup || `Значение не выбрано` }}</li>
                <li>Лечащий врач: {{ form.personalData.selectedPrimaryDoctor || `Значение не выбрано` }}</li>
                <li>Отправлять смс: {{ form.personalData.noSendSms || `Значение не выбрано` }}</li>
              </ul>
              <h5> Адресные данные</h5>

              <ul>
                <li>Индекс: {{ form.addressData.cityIndex || `Значение не выбрано` }}</li>
                <li>Страна: {{ form.addressData.country || `Значение не выбрано` }}</li>
                <li>Область: {{ form.addressData.area || `Значение не выбрано` }}</li>
                <li>Город: {{ form.addressData.city || `Значение не выбрано` }}</li>
                <li>Улица: {{ form.addressData.street || `Значение не выбрано` }}</li>
                <li>Дом: {{ form.addressData.home || `Значение не выбрано` }}</li>
              </ul>
              <h5>Паспортнные дданные</h5>
              <ul>
                <li>Тип документа: {{ form.passportData.selectedDocumentType || `Значение не выбрано` }}</li>
                <li>Серия: {{ form.passportData.series || `Значение не выбрано` }}</li>
                <li>Номер: {{ form.passportData.documentNumber || `Значение не выбрано` }}</li>
                <li>Кем выдан: {{ form.passportData.issuedBy || `Значение не выбрано` }}</li>
                <li>Дата выдачи: {{ form.passportData.dateOfIssue || `Значение не выбрано` }}</li>
              </ul>
            </section>
            <div class="form-buttons">
              <button
                  v-if="step !== 1" class="prevBtn" @click.prevent="prevStep">Назад
              </button>
              <button
                  v-if="step !== totalSteps && step !== 3"
                  class="nextBtn"
                  @click.prevent="nextStep"
              >Далее
              </button>
              <button
                  v-if="step === 3"
                  class="nextBtn"
                  @click.prevent="nextStep">Проверить
              </button>
              <button
                  v-if="step === 4"
                  class="nextBtn"
                  @click.prevent="sendForm">Зарегистрировать
              </button>
            </div>
          </form>
        </div>
      </main>
    </div>
    <div v-else class="success-registration">
      <div class="success-registration--body">
         <h1>Пользователь успешно зарегистрирован!</h1>
        <button @click="isDone = true">Продолжить регистрацию</button>
      </div>

    </div>
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
        personalData: {
          name: '',
          surname: '',
          middleName: '',
          birthday: '',
          phone: '',
          selectedSex: 'value not set',
          selectedClientGroup: [],
          selectedPrimaryDoctor: '',
          noSendSms: false
        },
        addressData: {
          cityIndex: '',
          country: '',
          area: '',
          city: '',
          street: '',
          home: ''
        },
        passportData: {
          selectedDocumentType: '',
          series: '',
          documentNumber: '',
          issuedBy: '',
          dateOfIssue: ''
        }
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
      totalSteps: 4,
      isDone: true
    }
  },

  validations: {
    form: {
      personalData: {
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
        },
      },
      addressData: {
        city: {
          required
        }
      },
      passportData: {
        selectedDocumentType: {
          required
        },
        dateOfIssue: {
          required
        }
      }
    }
  },

  methods: {
    nextStep() {
      switch (this.step) {
        case 1: {
          if (this.checkPersonalData()) {
            this.checkPersonalData()
          } else {
            this.step++
          }
          break
        }
        case 2: {
          if (this.checkAddressData()) {
            this.checkAddressData()
          } else {
            this.step++
          }
          break
        }
        case 3: {
          if (this.checkPassportData()) {
            this.checkPassportData()
          } else {
            this.step++
          }
          break
        }
      }
    },
    prevStep() {
      this.step--
    },
    sendForm() {
      console.log('Make API request.')
      this.form.personalData = {}
      this.form.addressData = {}
      this.form.passportData = {}
      this.isDone = false
      this.step = 1

    },
    checkPersonalData() {
      this.$v.form.personalData.$touch()
      if (this.$v.form.personalData.$error) {
        console.log('Валидация личных данных не прошла')
        return true
      }
    },
    checkAddressData() {
      this.$v.form.addressData.$touch()
      if (this.$v.form.addressData.$error) {
        console.log('Валидация адресных данных не прошла')
        return true
      }
    },
    checkPassportData() {
      this.$v.form.passportData.$touch()
      if (this.$v.form.passportData.$error) {
        console.log('Валидация паспортных данных не прошла')
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