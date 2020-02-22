<template>
  <q-carousel
    transition-prev="slide-right"
    transition-next="slide-left"
    swipeable
    animated
    v-model="slide"
    control-color="primary"
    navigation-icon="radio_button_unchecked"
    navigation
    padding
    height="200px"
    style="width: 30rem"
    class="shadow-1 rounded-borders"
    arrows
    autoplay=""
  >
    <q-carousel-slide :name="1" class="column no-wrap flex-center">
      <q-icon name="style" color="primary" size="56px" />
      <div class="q-mt-md text-center">
        Strona korzysta z angielsko języcznego API, więc nazwy potraw oraz przepisów należy wpisywać po angielsku
      </div>
    </q-carousel-slide>
    <q-carousel-slide :name="2" class="column no-wrap flex-center">
      <q-icon name="live_tv" color="primary" size="56px" />
      <div class="q-mt-md text-center">
        Składniki do wyszukiwania można dodawać bezpośrednio z innych przepisów, wszystkich na raz poprzez wciśnięcie przycisku podobne lub wciśnięcie poszczególnego składnika
      </div>
    </q-carousel-slide>
    <q-carousel-slide :name="3" class="column no-wrap flex-center">
      <q-icon name="layers" color="primary" size="56px" />
      <div class="q-mt-md text-center">
        Pierwsza pomoc
      </div>
    </q-carousel-slide>
    <q-carousel-slide :name="4" class="column no-wrap flex-center">
      <q-icon name="terrain" color="primary" size="56px" />
      <div class="q-mt-md text-center">
        Pierwsza pomoc
      </div>
    </q-carousel-slide>
  </q-carousel>
</template>

<script>
export default {
  name: "help-modal-carousel",
  data() {
    return {
      carousel: false,
      card: false,
      sliders: false,
      slide: 1
    };
  }
};
</script>
