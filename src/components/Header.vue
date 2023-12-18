<template>
    <header class=" d-flex flex-wrap justify-content-center py-3 mb-4 border-bottom position-sticky top-0 bg-light " style="z-index : 1;">
        <a href="index.html" class="d-flex align-items-center text-dark text-decoration-none ms-3 me-md-auto " >
            <img src="https://icones.pro/wp-content/uploads/2021/07/icone-meteo-noir.png" alt="Weather" width="50" height="50" >
            <span class="fs-4 ms-3">Weather</span>
        </a>
        <button type="button" class="navbar-toggler" >
            <span class="navbar-toggler-icon"></span>
        </button>
        
        <ul class="nav nav-pills ">
            <li href="index.html" class="nav-item">
                <a href="index.html" class="nav-link active" aria-current="page">Home</a>
            </li>
            <li href="" class="nav-item" >
                <a href="#" class="nav-link" aria-current="page">Contact</a>
            </li>
            <li href="" class="nav-item">
                <a href="#" class="nav-link" aria-current="page">Help</a>
            </li>
        </ul>
    </header>

</template>

<script>

export default {
    name: 'vue-header',
    props: {
        msg: String
    }
}

</script>