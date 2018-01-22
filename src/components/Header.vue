<template>
    <header>
        <div class="container">
            <h1 class="header-title"><a href="/" class="navbar-brand">Address Discover</a></h1>
        </div>
    </header>
</template>

<script>
export default {
    name: 'Header',
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
header {
    background-color: #514F5E;
}
header .container {
    height: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
}
header .header-title {
    margin: 0;
}
header .header-title a {
    color: white;
    text-decoration: none;
    font-size: 25px;
}
</style>
