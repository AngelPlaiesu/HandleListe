<script setup>
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import ColumnGroup from 'primevue/columngroup';   // optional
import Row from 'primevue/row';             
import { onMounted, ref,onBeforeUnmount } from 'vue';

const productName = ref('');
const productQuantity = ref(0);

const products = ref([]);

const addProduct = () => {
    products.value.push({id: crypto.randomUUID() ,name: productName.value, quantity: Number(productQuantity.value)});
    return;
}

const removeProduct = (id) => {
    products.value = products.value.filter(product => product.id !== id);
    return;
}

const saveList = () => {
    localStorage.setItem('products', JSON.stringify(products.value));
    return;
}

const getList = () => {
    const list = localStorage.getItem('products');
    if(list){
      products.value = JSON.parse(list);
    }
    return;
}

onMounted(() => {
    getList();
});

onBeforeUnmount(() => {
    saveList();
});

setInterval(() => {
    saveList();
}, 7000);

</script>

<template>
  <main>
    <span class="imputGrup ">
    <InputText type="text" v-model="productName"  style="flex: 2;"></InputText>
      <InputText type="number" v-model="productQuantity " style="flex: 2;"></InputText>
      <Button @click="addProduct()" class="addBtn" >Add</Button>
    </span>
    <DataTable :value="products">
    <Column field="name" header="Name"></Column>
    <Column field="quantity" header="Quantity"></Column>
    <Column>
    <template #body="{data}">
        <Button icon="pi pi-trash" class="p-button-rounded p-button-danger" @click="removeProduct(data.id)"></Button>
    </template>
    </Column>

    </DataTable>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  width: 50vw;
  margin: 0 auto; 
  padding-top: 2rem;
  gap: 1rem;
}
.imputGrup {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;  
  gap: 1rem;
}
.p-datatable{
  width: 100%;
  border-radius: 6rem;
}
</style>
