<template>
    <footer class="bg-light">
        <p>&copy; {{ new Date().getFullYear() }} Marcin Strzelec & Bartosz Wiśniewski</p>
    </footer>
</template>
  
<script>
export default {
    name: 'AppFooter'
};
</script>
  
<style>
footer {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-right: -100px;
    margin-left: -100px;
    height: 50px;
    padding: 20px;
    color: plum;
}
</style>
  