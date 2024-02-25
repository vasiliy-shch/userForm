<template>
  <div>
    <div v-if="!isEdit" @click="show">
      <div class="selectForm" :class="this.error ? 'selectForm_error' : ''">
          <span class="selectForm__topTitle selectForm__topTitle_visible"
            v-show="isSelected"
          >
            {{ placeholder }}
          </span>
          <span class="selectForm__title" :class="isSelected ? 'selectForm__title_selected' : ''">
            {{ title }}
          </span>
          <button class="selectForm__button selectForm__button_deRotate" :class="this.error ? 'selectedForm__button_error' : ''"></button>
      </div>
    </div>
    <div v-else-if="isEdit" @click="hide">
      <div class="selectForm__selectList">
        <div class="selectForm selectForm_inList">
          <span class="selectForm__title">{{ title }}</span>
          <button class="selectForm__button selectForm__button_rotate"></button>
        </div>
        <div class="selectForm__list">
          <div
            v-for="item in items"
            :key="item.id"
            :class="['selectForm__listItem', 
              isSelected && modelValue.id === item.id ?
              'selectForm__selectedListItem' : '']"
            @click="select(item)"
          >
            <span class="selectedForm__listItemTitle">{{ item.title }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  model: {
    prop: 'modelValue',
    event: 'update:modelValue'
  },
  props: {
    items: {
      type: Array,
      required: true,
    },
    placeholder: {
      type: String,
      default: 'Выберете элемент'
    },
    modelValue: {
      type: Object,
      default: null
    },
    error: {
      type: Boolean,
      default: false
    }
  },
  emits: ['update:modelValue'],
  data () {
    return {
      isEdit: false,
    }
  },
  computed: {
    isSelected() {
      return !(this.modelValue === null);
    },
    title() {
      return this.isSelected ? this.modelValue.title : this.placeholder;
    }
  },
  methods: {
    show () {
      this.isEdit = true;
    },
    hide () {
      this.isEdit = false;
    },
    select (item) {
      this.$emit('update:modelValue', item);
      this.show;
      console.log(this.isEdit)
    }
  }
}
</script>

<style scoped>

.selectForm {
  display: flex;
  position: relative;
  align-items: center;
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
  background-color: rgb(240, 242, 245);
  outline: none;
  cursor: pointer;
}
.selectForm_error {
  background-color: rgb(250, 235, 235);
  border: 1px solid rgb(230, 70, 70);
}
.selectForm__title {
  padding: 5px;
  font-size: 13px;
  line-height: 20px;
  color: rgba(41, 39, 125, 0.40);
  width: 100%;
}
.selectForm__title_selected {
  color: #29277d;
}
.selectForm__button {
  background-image: url(../components/icons/arrowSelect.svg);
  width: 16px;
  height: 16px;
  border: none;
  background-color: rgb(240, 242, 245);
  color:  #5F88F4;
  margin-right: 5px;
  cursor: pointer;
}
.selectedForm__button_error {
  background-color: rgb(250, 235, 235);
}
.selectForm__button_rotate {
  animation: rotation 0.3s linear 1;
  animation-fill-mode: forwards;
}
@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(180deg);
  }
}
.selectForm__button_deRotate {
  animation: deRotation 0.3s linear 1;
  animation-fill-mode: forwards;
}
@keyframes deRotation {
  from {
    transform: rotate(180deg);
  }
  to {
    transform: rotate(0deg);
  }
}
.selectForm__selectList {
  display: flex;
  flex-direction: column;
  margin: 20px 0 0 0;
  border: 1px solid rgba(0, 0, 0, 0.12);
  border-radius: 5px;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
}
.selectForm_inList {
  border: none;
  border-bottom: 1px solid rgba(0, 0, 0, 0.12);
  border-radius: 5px 5px 0 0;
  margin: 0;
}
.selectForm__list {
  font-size: 13px;
  color: #29277d;
  width: 100%;
}
.selectForm__listItem {
  cursor: pointer;
}
.selectForm__listItem:hover {
  background-color: #DADEFE;
  transition: background-color 0.25s, color 0.15s;
}
.selectForm__listItem {
    padding: 5px;
}
.selectedForm__listItemTitle {
  margin: 2px 5px;
}
.selectForm__selectedListItem {
  background-color: #DADEFE;
  color: rgb(0, 119, 255);
}
.selectForm__topTitle {
    font-size: 13px;
    position: absolute;
    top: -15px;
    left: 0;
    display: none;
    color: #757575
}
.selectForm__topTitle_visible {
    display: block;
}
</style>
