<template>
  <label class="yy-checkbox" :class="{ 'is-checked': isChecked }">
    <span class="yy-checkbox__input">
      <span class="yy-checkbox__inner"></span>
      <input
        type="checkbox"
        v-model="modelValue"
        class="yy-checkbox__original"
        :value="label"
      />
    </span>
    <span class="yy-checkbox__label">
      <slot></slot>
      <template v-if="!$slots.default">{{ label }}</template>
    </span>
  </label>
</template>

<script>
export default {
  name: "CheckBox",
  inject: {
    CheckBoxGroup: {
      default: "",
    },
  },

  props: {
    value: {
      type: [String, Number, Boolean],
      default: true,
    },
    label: {
      type: [String, Number, Boolean],
      default: false,
    },
  },
  computed: {
    isGroup() {
      return !!this.CheckBoxGroup;
    },

    modelValue: {
      get() {
        return this.isGroup ? this.CheckBoxGroup.value : this.value;
      },
      set(value) {
        this.isGroup
          ? this.CheckBoxGroup.$emit("input", value)
          : this.$emit("input", value);
      },
    },
    isChecked() {
      return this.isGroup
        ? this.modelValue.includes(this.label)
        : this.modelValue;
    },
  },
  methods: {
    changeStatus() {
      this.$emit("change", this.value);
    },
  },
};
</script>

<style lang="scss" scoped>
.yy-checkbox {
  color: #606266;
  font-weight: 500;
  font-size: 14px;
  position: relative;
  cursor: pointer;
  display: inline-block;
  white-space: nowrap;
  user-select: none;
  margin-right: 30px;
  .yy-checkbox__input {
    white-space: nowrap;
    cursor: pointer;
    outline: none;
    display: inline-block;
    line-height: 1;
    position: relative;
    vertical-align: middle;
    .yy-checkbox__inner {
      display: inline-block;
      position: relative;
      border: 1px solid #dcdfe6;
      border-radius: 2px;
      box-sizing: border-box;
      width: 14px;
      height: 14px;
      background-color: #fff;
      z-index: 1;
      transition: border-color 0.25s cubic-bezier(0.71, -0.46, 0.29, 1.46),
        background-color 0.25s, cubic-bezier(0.71, -0.46, 0.29, 1.46);
      &:after {
        box-sizing: content-box;
        content: "";
        border: 1px solid #ffffff;
        border-left: 0;
        border-top: 0;
        height: 7px;
        left: 4px;
        position: absolute;
        top: 1px;
        transform: rotate(45deg) scaleY(0);
        width: 3px;
        transition: transform 0.15s ease-in 0.05s;
        transform-origin: center;
      }
    }
    .yy-checkbox__original {
      opacity: 0;
      outline: none;
      position: absolute;
      left: 10px;
      margin: 0;
      width: 0;
      height: 0;
      z-index: -1;
    }
  }
  .yy-checkbox__label {
    display: inline-block;
    padding-left: 10px;
    line-height: 19px;
    font-size: 14px;
  }
}
// 选中的样式
.yy-checkbox.is-checked {
  .yy-checkbox__input {
    .yy-checkbox__inner {
      background-color: #409eff;
      border-color: #409eff;
      &:after {
        transform: rotate(45deg) scaleY(1);
      }
    }
  }
  .yy-checkbox__label {
    color: #409eff;
  }
}
</style>
