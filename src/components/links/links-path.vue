<template>
    <div class="links-path-background">
        <div class="links-path-desktop">
            <a href="#" class="link">Home</a> / <a href="#" class="link">Minha conta</a> / <a href="#" class="link current-link">Bolsas favoritas</a>
        </div>

        <div class="links-path-mobile">
            <font-awesome icon="fa-solid fa-chevron-left" class="link-icon"/><a href="#" class="link">Minha conta</a>
        </div>
    </div>
</template>

<script>
export default {
    name: 'LinksPath'
}
</script>

<style lang="scss" scoped>
    .links-path-background {
        padding: 20px 0;
        background-color: #FBFBFB;
    }

    .links-path-desktop {
        max-width: 1500px;
        margin: auto;
        padding-left: 40px;

        @media (max-width: 992px) {
            display: none;
        }
    }

    .links-path-mobile {
        margin: auto;
        padding-left: 20px;

        @media (min-width: 992px) {
            display: none;
        }
    }

    .link {
        text-decoration: none;
        color: #007A8D;
        font-size: 0.8rem;
    }

    .current-link {
        color: #1F2D30
    }

    .link-icon {
        font-size: 12px;
        color: #007A8D;
        padding-right: 10px;
    }
</style>