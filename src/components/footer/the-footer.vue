<template>
    <footer>
        <div class="top-footer">
            <FooterCard
                fa_icon="fa-brands fa-whatsapp"
                top_text="0800 123 2222"
                bottom_text_desktop="Seg - Sex 8h-22h"
                bottom_text_mobile="Segunda a Sexta de 8h às 22h"
                :small_mobile_card=false
                class="whatsapp"
            />
            <FooterCard
                fa_icon="fa-regular fa-comment"
                top_text="Chat ao vivo"
                bottom_text_desktop="Seg - Sex 8h-22h"
                bottom_text_mobile="Chat"
                :small_mobile_card=true
                class="chat"
            />
            <FooterCard
                fa_icon="fa-regular fa-envelope"
                top_text="Mande um e-mail"
                bottom_text_desktop="Respondemos rapidinho"
                bottom_text_mobile="E-mail"
                :small_mobile_card=true
                class="email"
            />
            <FooterCard
                fa_icon="fa-solid fa-circle-info"
                top_text="Central de ajuda"
                bottom_text_desktop="Encontre todas as respostas"
                bottom_text_mobile="Ajuda"
                :small_mobile_card=true
                class="help"
            />
        </div>
        <div class="bottom-footer">
            <span class="cute-message">Feito com <font-awesome icon="fa-regular fa-heart" /> pela Quero Educação</span>
        </div>
    </footer>
</template>

<script>
import FooterCard from './footer-card.vue'

export default {
    name: 'TheFooter',
    components: {
        FooterCard
    }
}
</script>

<style lang="scss" scoped>

.top-footer {
    background-color: #007A8D;
    padding: 0 5%;
    height: 125px;
    display: flex;
    justify-content: space-between;

    @media (max-width: 992px) {
        display: grid;
        grid-template-areas:
        'whatsapp whatsapp whatsapp'
        'chat email help';
        grid-template-columns: 1fr 1fr 1fr;
        justify-content: center;
        column-gap: 3px;
        row-gap: 3px;

        height: auto;
        padding: 0;
        background-color: #18ACC4;
    }
}

.whatsapp {
    grid-area: whatsapp;
}

.chat {
    grid-area: chat;
}

.email {
    grid-area: email;
}

.help {
    grid-area: help;
}


.bottom-footer {
    background-color: #18ACC4;
    height: 125px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.cute-message {
    color: white;
}
</style>