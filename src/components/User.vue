<template>
    <div class="users-container">
        <span>{{user.id}}</span>
        <span>{{user.username}}</span>
        <span>{{user.email}}</span>
        <span>{{user.address.street}}</span>
        <span>{{user.company.name}}</span>
    </div>
</template>

<script>
export default {
    name: 'User',
    props: ['user']
}
</script>

<style scoped>
    .users-container {
        display: grid;
        grid-gap: 20px;
        grid-template-columns: 30px repeat(4, 1fr);
        padding: 10px;
        background-color: #ffd9b3;
    }
</style>