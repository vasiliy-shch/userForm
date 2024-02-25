<template>
    <div class="userForm">
      <div v-if="isReg">
        <div class="userForm__mainForm userForm__mainForm_marginTop">
          <div class="userForm__logo"></div>
          <div class="userForm__span">
            Поздравляем, Ваш аккаунт успешно создан!
          </div>
          <button type="submit" class="userForm__button userForm__button_registration">Войти в аккаунт</button>
        </div>
      </div>
      <div v-if="!isReg" class="userForm__mainForm">
        <div class="userForm__logo"></div>
        <div class="userForm__header">Создать нового пользователя</div>
        <form 
          class="userForm__form" 
          @submit.prevent="formValid()"
          novalidate="true"
        >
          <input
            placeholder="Фамилия"
            oninput="this.value=this.value.replace(/[^а-яА-Я]/g,'');"
            class="userForm__input"
            :class="itemErrors.surnameError ? 'userForm__input_error' : ''"
            type="text"
            v-model="item.surname"
          >
          <span class="span__absolute">*</span>
          <span v-if="itemErrors.surnameError" class="userForm__errorMessage">Фамимлия указана некорректно</span>
          <input
            placeholder="Имя"
            oninput="this.value=this.value.replace(/[^а-яА-Я]/g,'');"
            class="userForm__input"
            :class="itemErrors.nameError ? 'userForm__input_error' : ''"
            type="text"
            v-model="item.name"
          >
          <span class="span__absolute">*</span>
          <span v-if="itemErrors.nameError" class="userForm__errorMessage">Имя указано некорректно</span>
          <input
            placeholder="Отчество"
            oninput="this.value=this.value.replace(/[^а-яА-Я]/g,'');"
            class="userForm__input"
            type="text"
            v-model="item.surn"
          >
          <div class="userForm__label"> Введите дату рождения </div>
            <input
              placeholder="Дата рождения"
              class="userForm__input"
              :class="itemErrors.dateOfBirthError ? 'userForm__input_error' : ''"
              type="date"
              v-model="item.dateOfBirth"
            >
            <span class="span__absolute">*</span>
            <span v-if="itemErrors.dateOfBirthError" class="userForm__errorMessage">Введите дату ррождения</span>
          <input
            placeholder="Номер телефона"
            class="userForm__input"
            :class="itemErrors.telephoneError ? 'userForm__input_error' : ''"
            type="number"
            v-model="item.telephone"
          >
          <span class="span__absolute">*</span>
          <span v-if="itemErrors.telephoneError" class="userForm__errorMessage">Номер телефона указан некорректно</span>
          <div class="radioBox__row">
            <input type=checkbox value="false" v-model="item.noSms">
            <span class="userForm__span">не отправлять смс</span>
          </div>
          <div class="userForm__radioBox">
            <label class="userForm__label"> Выберите ваш пол </label>
            <div class="radioBox__row">
              <input
                class="userForm__input userForm__input_checkBox"
                type="radio"
                name="radio"
                v-model="item.gender"
                value="men"
              >
              <label class="radioBox__label">Мужской</label>
            </div>
            <div class="radioBox__row">
              <input
                class="userForm__input userForm__input_checkBox"
                type="radio"
                name="radio"
                v-model="item.gender"
                value="women"
              >
              <label class="radioBox__label">Женский</label>
            </div>
          </div>
          <span class="span__absolute">*</span>
          <selected-component
            v-model="item.clientGroup"
            :items="clientsGroups"
            :error="itemErrors.clientGroupError"
            placeholder="Выберите группу клиентов"
          />
          <span v-if="itemErrors.clientGroupError" class="userForm__errorMessage">Выберите группу клиентов</span>
          <selected-component
            v-model="item.doctor"
            :items="doctors"
            placeholder="выберите лечащего врача"
          />
          <input
            placeholder="Индекс"
            class="userForm__input"
            type="number"
            v-model="item.index"
          >
          <input
            placeholder="Страна"
            oninput="this.value=this.value.replace(/[^а-яА-Я]/g,'');"
            class="userForm__input"
            type="text"
            v-model="item.country"
          >
          <input
            placeholder="Область"
            oninput="this.value=this.value.replace(/[^а-яА-Я]/g,'');"
            class="userForm__input"
            type="text"
            v-model="item.region"
          >
          <input
            placeholder="Город"
            oninput="this.value=this.value.replace(/[^а-яА-Я]/g,'');"
            class="userForm__input"
            :class="itemErrors.cityError ? 'userForm__input_error' : ''"
            type="text"
            v-model="item.city"
          >
          <span class="span__absolute">*</span>
          <span v-if="itemErrors.cityError" class="userForm__errorMessage">Укажите город</span>
          <input
            placeholder="Улица"
            class="userForm__input"
            type="text"
            v-model="item.street"
          >
          
          <input
            placeholder="Дом"
            class="userForm__input"
            type="number"
            v-model="item.house"
          >
          <div class="span__absolute">*</div>
          <selected-component
            v-model="item.typeDoc"
            :error="itemErrors.typeDocError"
            :items="typesDocuments"
            placeholder="Выберите тип документа"
          ></selected-component>
          <span v-if="itemErrors.typeDocError" class="userForm__errorMessage">Выберите документ</span>
          <input
            placeholder="Серия"
            class="userForm__input"
            type="number"
            v-model="item.seriesDoc"
          >
          <input
            placeholder="Номер"
            class="userForm__input"
            type="number"
            v-model="item.numberDoc"
          >
          <input
            placeholder="Кем выдан"
            class="userForm__input"
            type="text"
            v-model="item.whoIssuance"
          >
          <div class="userForm__label"> Введите дату выдачи документа</div>
          <input
            placeholder="Дата выдачи"
            class="userForm__input"
            :class="itemErrors.dateIssuanceError ? 'userForm__input_error' : ''"
            type="date"
            v-model="item.dateIssuance"
          >
          <span class="span__absolute">*</span>
          <span v-if="itemErrors.dateIssuanceError" class="userForm__errorMessage">Укажите дату выдачи документа</span>
          <button type="submit" class="userForm__button">Создать аккаунт</button>
          <div class="userForm__span">
            Обращаем ваше внимание на то, что поля со звездочкой * 
            - обязательны для заполнения.
          </div>
        </form>
      </div>
    </div>
</template>

 <script>
import SelectedComponent from './SelectComponent.vue'
export default {
  components: {
    SelectedComponent
  },
  data() {
    return {
      isReg: false,
      globalError: null,
      item: {
        surname: null,
        name: null,
        surn: null,
        dateOfBirth: null,
        telephone: null,
        noSms: null,
        gender: null,
        clientGroup: null,
        doctor: null,
        index: null,
        country: null,
        region: null,
        city: null,
        street: null,
        house: null,
        typeDoc: null,
        seriesDoc: null,
        numberDoc: null,
        whoIssuance: null,
        dateIssuance: null,
      },
      itemErrors: {
        surnameError: false,
        nameError: false,
        dateOfBirthError: false,
        telephoneError: false,
        clientGroupError: false,
        cityError: false,
        typeDocError: false,
        dateIssuanceError: false,
      },
      doctors: [
      {
          id: 1,
          title: 'Иванов'
        },
        {
          id: 2,
          title: 'Захаров'
        },
        {
          id: 3,
          title: 'Чернышева'
        }
      ],
      clientsGroups: [
      {
          id: 1,
          title: 'VIP'
        },
        {
          id: 2,
          title: 'Проблемные'
        },
        {
          id: 3,
          title: 'ОМС'
        }
      ],
      typesDocuments: [
      {
          id: 1,
          title: 'Паспорт'
        },
        {
          id: 2,
          title: 'Свидетельство о рождении'
        },
        {
          id: 3,
          title: 'Вод. удостоверение'
        }
      ]
    }
  },
  methods: {
    isValueValid(value) {
      if (value !== '' && value !== null) {
        return true;
      } else {
        return false;
      }
    },
    isMobilePhoneNumberValid (value = '') {
      const regexp = new RegExp(/^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/im)
      return regexp.test(value) && String(value).replace(/\D/g, '').length === 11
    },
    formValid() {
      if (this.globalError !== false) {
        for (let prop in this.item) {
          if (!this.isValueValid(this.item[prop])) {
            this.itemErrors[prop+'Error'] = true;
            this.globalError = true;
          }
          if (this.isValueValid(this.item[prop])) {
            this.itemErrors[prop+'Error'] = false;
            this.globalError = false;
          }
          if (prop === 'telephone') {
            if (this.isMobilePhoneNumberValid(this.item[prop])) {
              this.itemErrors[prop+'Error'] = false;
              this.globalError = true;
            } else {
              this.itemErrors[prop+'Error'] = true;
              this.globalError = false;
            }
          }
        }
      };

      if (this.globalError === false) {
        this.isReg = true;
      }
    }
  }
}
</script>

<style scoped lang="scss">
.span__absolute {
  position: absolute;
  right: 25px;
}
.userForm {
    border-radius: 12px;
    width: 320px;
    position: relative;
    margin: 50px auto;

    &__mainForm {
      border-radius: 12px;
      background-color: white;
      padding: 32px 24px;
    }

    &__logo {
      text-align: center;
    }

    &__logo {
      background-image: url(../components/icons/logoIcon.svg);
      width: 48px;
      height: 48px;
      margin: 0 auto;
    }

    &__header {
      font-size: 20px;
      line-height: 24px;
      text-align: center;
      margin-top: 16px;
      font-weight: 500;
      -webkit-font-smoothing: subpixel-antialiased;
    }

    &__form {
      margin: 0;
      padding: 0;
    }

    &__label {
      margin-top: 20px;
      font-size: 14px;
    }

    &__input {
      box-sizing: border-box;
      width: 100%;
      height: 36px;
      margin-top: 20px;
      padding: 8px 12px;
      line-height: 20px;
      font-size: 13px;
      border-radius: 8px;
      border: 1px solid rgba(0, 0, 0, 0.12);
      text-overflow: ellipsis;
      color: #29277d;
      background-color: rgb(240, 242, 245);
      outline: none;
    }

    &__input:focus {
      border: 1px solid #5181b8
    }

    &__radioBox {
      display: flex;
      flex-direction: column;
      margin: 0;
    }

    &__input_checkBox {
      width: 20px;
      height: 20px;
      margin: 0 10px 0 0;
    }

    &__input_error {
      background-color: rgb(250, 235, 235);
      border: 1px solid rgb(230, 70, 70);
    }

    &__select {
      display: flex;
    }

    &__errorMessage {
      font-size: 13px;
      margin-top: 0px;
      color: rgb(230, 70, 70);
    }

    &__button {
      width: 100%;
      height: 36px;
      margin: 30px 0 13px 0;
      padding: 0 16px;
      font-size: 14px;
      text-align: center;
      box-sizing: border-box;
      color: white;
      border-radius: 8px;
      border: none;
      background-color: rgb(0, 119, 255);
      cursor: pointer;
    }

    &__button:hover {
      opacity: 0.9;
    }

    &__mainForm_marginTop {
      margin-top: 16px;
    }

    &__button_registration {
      margin: 0;
      background-color: rgb(75, 179, 75);
    }

    &__span {
      color: #757575;
      line-height: 18px;
      font-size: 13px;
      text-align: center;
    }
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
.radioBox {

  &__row {
    display: flex;
    align-items: center;
    margin: 5px 0;
  }

  &__label {
    font-size: 13px;
  }
}
</style>