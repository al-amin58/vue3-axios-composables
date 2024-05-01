<h1>Vue3 axios Composables</h1>

`src/composables/useAxios.js`

```javascript
import { ref, onMounted } from 'vue';
import axios from 'axios';

const axiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_URL, //"https://yourdomain.com", //"http://localhost:8000",
  withCredentials: true,
  xsrfHeaderName: "X-XSRF-TOKEN", 
});

export default function useAxios() {
  const loading = ref(false);
  const error = ref(null);

  const sendRequest = async (config) => {
    loading.value = true;

    try {
      const response = await axiosInstance(config);
      return response;
    } catch (err) {
      error.value = err;
    } finally {
      loading.value = false;
    }
  };
  return {
    loading,
    error,
    sendRequest,
  };
}
```




### Use Axios Composables in Vue Component
`src/views/products.vue`

```javascript
<script setup>
    import {onMounted, ref} from 'vue'
    import useAxios from "@/composables/useAxios"
    const { loading, error, sendRequest } = useAxios();

    const products = ref(null);

    onMounted(async () => {
        const responseData = await sendRequest({
            method: 'get',
            url: '/api/product',
        });
        products.value = responseData.data;
        });
</script> 

<template>
    <div v-for="product in products">
        .....
    </div>
</template>
```




### Create Product

`src/views/CreateProduct.vue`

```javascript
<script setup>
    import {ref} from 'vue'
    import useAxios from "@/composables/useAxios"
    const { loading, error, sendRequest } = useAxios();

    const form = ref({
        title: null,
        price: null'
    });

    const saveProduct = await sendRequest({
        method: 'post',
        url: '/api/product',  //Your API Endpoint
        data: form.value
    });
</script> 
```
