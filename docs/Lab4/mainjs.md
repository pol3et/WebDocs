# main.js

## Vue App Initialization

```javascript
import { createApp } from 'vue';
import { createRouter, createWebHistory } from 'vue-router';
import 'bootstrap/dist/css/bootstrap.min.css';
import axios from 'axios';
import App from './App.vue';
```

- **Description**: Initializes the Vue app, sets up routing, includes Bootstrap styles, and imports the Axios library.

## Axios Configuration

```javascript
axios.defaults.baseURL = 'http://127.0.0.1:8000/';
```

- **Description**: Configures Axios to use 'http://127.0.0.1:8000/' as the base URL for API requests.

## Vue App and Router Configuration

```javascript
const app = createApp(App);
app.config.globalProperties.isAuthenticated = false;

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/login', component: Login },
    { path: '/register', component: Register },
    { path: '/profile', component: Profile, meta: { requiresAuth: true } },
    { path: '/', component: Home },
    { path: '/brokers', component: Brokers, meta: { requiresAuth: true } },
    { path: '/producers', component: Producers, meta: { requiresAuth: true } },
    { path: '/products', component: Products, meta: { requiresAuth: true } },
    { path: '/cosignments', component: Cosignments, meta: { requiresAuth: true } },
  ],
});
```

- **Description**: Configures the Vue app and sets up routes for different components/pages, including authentication checks.

## Route Navigation Guard

```javascript
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    if (!localStorage.getItem('access_token')) {
      next({
        path: '/login',
        query: { redirect: to.fullPath },
      });
    } else {
      next();
    }
  } else {
    next();
  }
});
```

- **Description**: Defines a navigation guard to check if authentication is required for the route. Redirects to the login page if not authenticated.

## Mounting the Vue App

```javascript
app.use(router);
app.mount('#app');
```

- **Description**: Integrates the router into the Vue app and mounts it to the element with the ID 'app'.
## [Login.vue](login.md) is up next!