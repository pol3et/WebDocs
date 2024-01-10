
# Home.vue

## Home Component

```html
<template>
  <div>
    <h1 class="title">Stock, Click, and Two Smoking Barcodes</h1>
  </div>
</template>
```

- **Description**: Vue template for the home component. Displays a title with a catchy phrase related to stock management.

## Home Component Script

```html
<script>
export default {
  name: 'Home',
};
</script>
```

- **Description**: Vue script for the home component. Provides the component name.

## Home Component Styles

```html
<style scoped>
.title {
  text-align: center;
  margin-top: 50vh;
  transform: translateY(-50%);
  animation: fadeIn 1.5s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
```

- **Description**: Vue styles for the home component. Centers the title, adds a margin from the top, and includes a fadeIn animation for visual appeal.

## [Brokers.vue](brokers.md) next