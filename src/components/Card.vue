<template>
  <div class="card" @click="openPopup" target="_blank">
    <div class="card-bg" :style="'background-image: url(' + preview + ');'"></div>

    <div class="card-content">
      <div class="popup">
        <h2>{{ $t('card.click-message') }}</h2>
      </div>

      <div class="true-content">
        <h2>{{ name }}</h2>

        <p class="categories" v-if="categories && categories.length">
          {{ categories.map(cat => $t('card.categories.' + cat)).join(', ') }}
        </p>

        <p>{{ description }}</p>
      </div>
    </div>

    <Popup class="popup-content" v-if="showPopup" :title="name" :content="popupContent" :link="file"
      @close="closePopup" />
  </div>
</template>

<script>
import Popup from './Popup.vue';

export default {
  components: {
    Popup,
  },

  props: ['name', 'description', 'date', 'preview', 'file', 'categories', 'details'],

  data() {
    return {
      showPopup: false,
      popupContent: '',
    };
  },

  methods: {
    openPopup() {
      const lang = this.$i18n.locale;
      this.popupContent = this.details?.[lang] || '';
      this.showPopup = true;
    },
    closePopup() {
      this.showPopup = false;
    },
  }

}
</script>

<style scoped>
.card:hover {
  cursor: pointer;
  transform: scale(1.01);
  border-color: #6785bfff;
}

.card:hover .card-bg {
  transform: scale(1.2);
  filter: brightness(1) blur(0px);
}

.card:hover .true-content {
  transition: .2s ease-out;
  opacity: 0;
}

.card:hover .popup {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1.5);
}

.card {
  position: relative;
  width: 400px;
  height: 500px;
  min-width: 350px;
  border: 3px solid #6785bf55;
  border-radius: 15px;
  margin: 10px;
  align-self: flex-start;
  overflow: hidden;
  font-size: 25px;
  transition: .2s ease-out;
  display: flex;
  flex-direction: column;
  user-select: none;
}

.card-bg {
  transition: .2s ease-out;
  position: absolute;
  object-fit: cover;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-size: cover;
  background-position: center;
  z-index: 0;
}

.card-content {
  position: relative;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  color: rgba(255, 255, 255, 0.87);
  align-items: center;
  text-align: center;
  z-index: 1;
}

.true-content {
  backdrop-filter: brightness(0.85) blur(100px);
  padding-bottom: 16px;
  padding-top: 16px;
  margin-bottom: 0;
  border-top: 3px solid #6785bf55;
  transition: .3s ease-out;
}

.popup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  transition: .2s ease-out;
  opacity: 0;
  text-align: center;
  z-index: 2;
  /* Ensure it appears above other elements */
}

h2 {
  margin: 0;
  margin-bottom: 5px;
  font-size: 1.5em;
  text-align: center;
}

p {
  margin: 0;
  margin: 0 20px;
  text-align: center;
  font-size: 1em;
}

.categories {
  font-size: 0.65em;
  color: #ccccccaa;
  margin: 6px 0 12px 0;
}

@media screen and (max-width: 730px) {
  .card {
    font-size: 20px;
    height: 350px;
    margin: 8px;
  }

  .card:hover {
    transform: scale(1);
    border-color: #6785bfff;
  }

  .card:hover .card-bg {
    transform: scale(1);
    filter: brightness(1) blur(0px);
  }

  .card:hover .true-content {
    transition: 0s;
    opacity: 1;
  }

  .card:hover .popup {
    opacity: 0;
    transform: translate(0%, 0%) scale(1);
  }
}

@media screen and (max-width: 510px) {
  .true-content {
    backdrop-filter: brightness(0.85) blur(15px);
  }
}

@media screen and (max-width: 400px) {
  .card {
    min-width: 95%;
    width: 95%;
  }
}
</style>
