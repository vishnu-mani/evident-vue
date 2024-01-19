<template>
    <div id="app">
      <CustomTable
        :data="flattenedUserData"
        :columns="tableColumns"
        @sort="handleSort">
      </CustomTable>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  import CustomTable from './components/CustomTable.vue';
  
  const userData = ref([]);
  const flattenedUserData = ref([]);

  const tableColumns = ref([
    { key: 'slno', label: 'Sl.No', sortable: false, sortOrder: 'desc' },
    { key: 'name', label: 'Name', sortable: true, sortOrder: 'desc' },
    { key: 'dob', label: 'DOB', sortable: true, sortOrder: 'desc' },
    { key: 'email', label: 'Email', sortable: true, sortOrder: 'desc' },
    { key: 'location', label: 'Location', sortable: true, sortOrder: 'desc' },
    { key: 'phone', label: 'Phone', sortable: true, sortOrder: 'desc' },
    { key: 'picture', label: 'Picture', sortable: false, sortOrder: 'desc'  }
  ]);
  
  onMounted(async () => {
    // Fetch data from the API
    const response = await fetch('https://randomuser.me/api/?results=50');
    const data = await response.json();
    userData.value = data.results;
    
    // Flatten user data structure
    flattenedUserData.value = flattenUserData(userData.value);

    const nameColumn = tableColumns.value.find(col => col.key === 'name');
    handleSort(nameColumn);
  });
  
  const flattenUserData = (users) => {
    return users.map((user, index) => ({
      slno: index + 1,
      name: `${user.name.first} ${user.name.last}`,
      dob: user.dob.date.split('T')[0],
      email: user.email,
      location: flattenObj(user.location),
      phone: user.phone,
      picture: user.picture.thumbnail,
    }));
  };
  
  const handleSort = (column) => {
    flattenedUserData.value.sort((a, b) => {
      const aValue = column.key === 'location' ? a.location.city : a[column.key];
      const bValue = column.key === 'location' ? b.location.city : b[column.key];

      if (column.sortOrder === 'asc') {
        return aValue > bValue ? 1 : (aValue < bValue ? -1 : 0);
      }
      return bValue > aValue ? 1 : (bValue < aValue ? -1 : 0);
    });

    // For static serial number 
    // ie; Serial number shouldn't be changed while sorting
    flattenedUserData.value.map((user, index) => user.slno = index+1);
  };

  const flattenObj = (obj) => {
    let result = {};
    for (const i in obj) {
        if ((typeof obj[i]) === 'object' && !Array.isArray(obj[i])) {
            const temp = flattenObj(obj[i]);
            for (const j in temp) {
                result[i + '.' + j] = temp[j];
            }
        }
        else {
            result[i] = obj[i];
        }
    }
    return result;
  }
  </script>