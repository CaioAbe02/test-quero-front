<template>
    <div class="card" :class="small_mobile_card && 'card-mobile'">
        <font-awesome :icon=fa_icon class="card-icon"/>
        <span class="card-top-text" :class="small_mobile_card && 'hide-mobile'">{{ top_text }}</span>
        <span class="card-bottom-text" :class="small_mobile_card && 'hide-mobile'">{{ bottom_text_desktop }}</span>
        <span class="card-bottom-text hide-desktop" :class="!small_mobile_card && 'hide-mobile'">{{ bottom_text_mobile }}</span>
    </div>
</template>

<script>
export default {
    name: 'FooterCard',
    props: {
        fa_icon: {
            required: true,
            type: String
        },
        top_text: {
            required: true,
            type: String
        },
        bottom_text_desktop: {
            required: true,
            type: String
        },
        bottom_text_mobile: {
            required: true,
            type: String
        },
        small_mobile_card: {
            required: true,
            type: Boolean
        }
    }
}
</script>

<style lang="scss" scoped>
.card {
    display: grid;
    grid-template-areas:
        'icon top-text'
        'icon bottom-text';
    align-items: center;
    align-content: center;
    justify-items: start;
    color: white;
    padding: 2.5% 0;

    @media (max-width: 992px) {
        background-color: #007A8D;
        column-gap: 15px;
    }
}

.card-mobile {
    @media (max-width: 992px) {
        display: flex;
        flex-direction: column;
    }
}

.card-icon {
    grid-area: icon;
    font-size: 40px;
    padding-right: 20px;
    justify-self: end;

    @media (max-width: 992px) {
        padding-right: 0;
    }
}

.card-top-text {
    grid-area: top-text;
    font-size: 1rem;
}

.card-bottom-text {
    grid-area: bottom-text;
}

.hide-mobile {
    @media (max-width: 992px) {
        display: none;
    }
}

.hide-desktop {
    @media (min-width: 992px) {
        display: none;
    }
}
</style>