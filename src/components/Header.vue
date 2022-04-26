<template>
    <nav class="navbar mb-5 p-1">
    <a class="navbar-brand" href="#">
        <i class="bi bi-receipt-cutoff icon"></i>
            Receipt Form
    </a>
    </nav>
</template>

<script>
export default {
    name: "HeaderTag",
    props: {
        msg: String,
    },
}
</script>

<style>
    .icon{
        font-size: 24px;
        margin: 0 0.5rem 0 3rem  ;
    }
    .navbar{
        background: rgb(248 180 0 / 45%);
        color: #125B50;
    }
    a.navbar-brand{
        color: #125B50;
    }
</style>