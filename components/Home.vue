<template>
  <div>
    <FireWork/>
    <SpinWheel/>
  </div>
</template>

<script>
import FireWork from './FireWork';
import SpinWheel from './SpinWheel';

export default {
  name: 'Home',
  components: {
    FireWork,
    SpinWheel
  },
  data() {
    return {
    }
  },
  methods: {
  },
}
</script>
