<template>
  <div class="main-wireframe">
    <main class="app-content">
      <form @submit.prevent>
        <label for="register-name" class="form-label-group">
          <h2 class="required">What's your friends name?</h2>
          <span />
          <input required
            type="text"
            id="register-name"
            placeholder="Name"
            @change="getName">
        </label>

        <label for="register-file" class="form-label-group">
          <h2>Upload photo</h2>
          <span>You can either select a photo from your gallery or take one now.</span>
          <file-upload
            name="register-file"
            @file-selected="getFile"
            @loading-file="handleLoadingState" />
        </label>

        <button
          @click="validateForm"
          :disabled="!canRegisterPlant"
          type="submit">
          <svgicon
            icon="leaf"
            width="16"
            height="24"
            color="#000" />
          <span>Add plant</span>
        </button>
      </form>
    </main>
  </div>
</template>

<script>
  import { mapActions } from 'vuex'
  import FileUpload from '@/components/FileUpload'
  import { getPlantStructure } from '@/app/plant/utils'
  import '@/assets/icons/leaf'

  export default {
    name: 'RegisterPlant',

    meta: {
      title: 'Add a new friend'
    },

    components: {
      'file-upload': FileUpload
    },

    computed: {
      canRegisterPlant () {
        return this.name !== '' || this.isUploadingFile
      }
    },

    data () {
      return {
        name: '',
        blob: undefined,
        isUploadingFile: false,
        showDialog: false
      }
    },

    created () {
      this.updateAppHeader({
        title: 'Add a new friend',
        backBtn: true,
        backBtnPath: this.returnRoutePath
      })
    },

    methods: {
      ...mapActions([
        'addPlant',
        'updateAppHeader'
      ]),
      handleLoadingState ({ loading }) {
        this.isUploadingFile = loading
      },
      getFile (data) {
        this.blob = data.blob
      },
      getName (event) {
        if (!event.target.value) return
        this.name = event.target.value
      },
      validateForm () {
        const config = {
          ...getPlantStructure(),
          blob: this.blob,
          name: this.name
        }
        this.addPlant(config)
          .then(guid => this.$router.push(`/plant/${guid}`))
      }
    }
  }
</script>

<style lang="postcss" scoped>
  .register-dialog {
    & input {
      width: 100%;
    }
  }

  .main-wireframe {
    background: var(--background-secondary);

    & .app-content {
      padding: var(--base-gap) 0;
      line-height: 150%;
    }
  }

  form {
    position: relative;
    z-index: 1;
    padding: 0 var(--base-gap);
    width: 100%;

    & input {
      width: 100%;
    }

    & select {
      padding: calc(var(--base-gap) + 5px) var(--base-gap);
    }
  }

  .form-order {
    display: flex;
    background: white;
    padding: 0;
    margin-bottom: var(--base-gap);
    border: none;
    border-radius: var(--border-radius);
    box-shadow: 4px 4px 0 rgba(0, 0, 0, 0.12);
    width: 100%;
  }

  .form-label-group {
    display: block;
    width: 100%;
    margin-bottom: calc(var(--base-gap) * 2);
  }

  label h2 {
    margin-bottom: calc(var(--base-gap) / 2);

    &.required::after {
      color: var(--text-color-button);
      content: "*";
      font-size: var(--text-size-medium);
      background: var(--brand-red);
      border-radius: 50%;
      width: var(--icon-size-small);
      height: var(--icon-size-small);
      display: inline-flex;
      justify-content: center;
      align-content: center;
      margin-left: calc(var(--base-gap) / 2);
    }
  }

  label span {
    display: block;
    color: var(--text-color-secondary);
    font-size: var(--text-size-small);
    margin-bottom: calc(var(--base-gap) / 2);
    padding: 0 1px;
  }
</style>
