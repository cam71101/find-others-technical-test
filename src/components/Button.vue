<template>
  <Button> {{ data }} </Button>
</template>

<script>
export default {
  name: 'Card',
  // load in props
  props: ['data'],
};
</script>

<style>
.card-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  width: 15rem;
}

.image {
  width: 100%;
}
.subtitle {
  margin: 0.6rem;
  height: 2rem;
}
</style>
