<template>
  <div
    :class="[{'is-dark': dark}]"
    class="datepicker-buttons-container flex justify-content-right"
  >
    <button
      v-if="showCloseBtn"
      class="datepicker-button close flex align-center justify-content-center"
      :class="{'right-margin': hasButtonValidate}"
      tabindex="-1"
      type="button"
      @click="emitClose()"
    >
      <span
        :style="[bgStyle]"
        class="datepicker-button-effect"
      />
      <span

        class="datepicker-button-content"
        :style="[colorStyle]"
      >
        <svg
          height="15"
          viewBox="0 0 365.71733 365"
          width="15"
          xmlns="http://www.w3.org/2000/svg"
        >
          <g fill="#f44336">
            <path d="m356.339844 296.347656-286.613282-286.613281c-12.5-12.5-32.765624-12.5-45.246093 0l-15.105469 15.082031c-12.5 12.503906-12.5 32.769532 0 45.25l286.613281 286.613282c12.503907 12.5 32.769531 12.5 45.25 0l15.082031-15.082032c12.523438-12.480468 12.523438-32.75.019532-45.25zm0 0" />
            <path d="m295.988281 9.734375-286.613281 286.613281c-12.5 12.5-12.5 32.769532 0 45.25l15.082031 15.082032c12.503907 12.5 32.769531 12.5 45.25 0l286.632813-286.59375c12.503906-12.5 12.503906-32.765626 0-45.246094l-15.082032-15.082032c-12.5-12.523437-32.765624-12.523437-45.269531-.023437zm0 0" />
          </g>
        </svg>
      </span>
    </button>
    <div
      v-if="showCloseBtn"
      style="flex: 1"
    />
    <button
      v-if="hasButtonNow"
      class="datepicker-button now flex align-center justify-content-center"
      :class="{'right-margin': hasButtonValidate}"
      tabindex="-1"
      type="button"
      @click="emitNow()"
    >
      <span
        :style="[bgStyle]"
        class="datepicker-button-effect"
      />
      <span
        class="datepicker-button-content"
        :style="[colorStyle]"
      >
        {{ buttonNowTranslation || 'Now' }}
      </span>
    </button>
    <button
      v-if="hasButtonValidate"
      type="button"
      tabindex="-1"
      class="datepicker-button validate flex align-center justify-content-center"
      @click.stop="$emit('validate')"
    >
      <span
        class="datepicker-button-effect"
        :style="[bgStyle]"
      />
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        :style="[colorStyle]"
      >
        <path
          d="M0 0h24v24H0z"
          fill="none"
        />
        <path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z" />
      </svg>
    </button>
  </div>
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'ButtonValidate',
    props: {
      /**
       * TODO: Remove wrong default values
       */
      dark: { type: Boolean, default: null },
      buttonColor: { type: String, default: null },
      buttonNowTranslation: { type: String, default: null },
      onlyTime: { type: Boolean, default: null },
      noButtonNow: { type: Boolean, default: null },
      range: { type: Boolean, default: null },
      showCloseBtn: { type: Boolean, default: false },
      hasButtonValidate: { type: Boolean, default: null }
    },
    computed: {
      colorStyle () {
        return {
          color: this.buttonColor,
          fill: this.buttonColor
        }
      },
      bgStyle () {
        return {
          backgroundColor: this.buttonColor
        }
      },
      hasButtonNow () {
        return !this.onlyTime && !this.noButtonNow && !this.range
      }
    },
    methods: {
      emitNow () {
        this.$emit('now', moment().format('YYYY-MM-DD HH:mm'))
      },
      emitClose () {
        this.$emit('close')
      }
    }
  }
</script>

<style lang="scss" scoped>
  .datepicker-buttons-container {
    padding: 5px;
    border-top: 1px solid #EAEAEA;
    background-color: #FFF;
    z-index: 1;
    display: flex !important;
    .datepicker-button {
      padding: 0px 20px;
      position: relative;
      background-color: white;
      border: 1px solid transparent;
      border-radius: 4px;
      height: 30px;
      font-size: 14px;
      outline: none;
      cursor: pointer;
      -webkit-transition: all 0.25s cubic-bezier(0.645, 0.045, 0.355, 1);
      color: #FFF;
      font-weight: 500;
      &-content {
        position: relative;
      }
      svg {
        position: relative;
        -webkit-transition: all 450ms cubic-bezier(0.23, 1, 0.32, 1) 0ms;
        transition: all 450ms cubic-bezier(0.23, 1, 0.32, 1) 0ms;
        fill: #00C853;
      }
      .datepicker-button-effect {
        position: absolute;
        background: #00C853;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        height: 30px;
        border-radius: 4px;
        width: 100%;
        -webkit-transition: all 450ms cubic-bezier(0.23, 1, 0.32, 1) 0ms;
        transition: all 450ms cubic-bezier(0.23, 1, 0.32, 1) 0ms;
        transform: scale(0);
      }
      &:hover {
        border: 1px solid transparent;
        .datepicker-button-effect {
          transform: scale(1);
        }
        svg {
          fill: white !important;
        }
        .datepicker-button-content {
          color: #fff !important;
        }
      }
      &.now {
        &.right-margin {
          margin-right: 10px;
        }
        .datepicker-button-content {
          color: dodgerblue;
        }
        .datepicker-button-effect {
          background: dodgerblue;
        }
      }
      &.validate, &.close {
        border: 1px solid #eaeaea;
      }
      &.close:hover .datepicker-button-effect {
        background-color: #eaeaea;
      }
    }
    &.is-dark, &.is-dark .datepicker-button {
      background-color: #424242;
      &:not(.now) {
        border-color: lighten(#424242, 20%);
      }
      svg {
        fill: white !important;
      }
    }
  }
</style>
