<template>
    <header>
        <div class="infos">
            <div class="help">
                <font-awesome icon="fa-solid fa-circle-info" class="info-icon"/>
                <span class="help-text-desktop">Como funciona</span>
                <span class="help-text-mobile">Ajuda</span>
            </div>
            <div class="whatsapp">
                <font-awesome icon="fa-brands fa-whatsapp" class="whatsapp-icon"/>
                <span class="whatsapp-number">0800 123 2222</span>
                <span class="whatsapp-text">Envie mensagem ou ligue</span>
            </div>
        </div>
        <img alt="Vue logo" src="../../assets/logo-querobolsa.svg" class="quero-bolsa-logo">
        <div class="user">
            <span class="user-name-desktop">Nome Sobrenome</span>
            <font-awesome icon="fa-regular fa-circle-user" class="user-icon"/>
            <span class="user-name-mobile">Conta</span>
        </div>
    </header>
</template>

<script>
export default {
    name: 'TheHeader'
}
</script>

<style lang="scss" scoped>
header {
    display: grid;
    grid-template-columns: 1fr auto 1fr;
    align-items: center;

    background-color: white;
    max-width: 1500px;
    max-height: 100px;
    margin: auto;
    font-size: 1rem;
    font-weight: 600;
    color: #007A8D;
    white-space: nowrap;

    @media (max-width: 992px) {
        display: flex;
        justify-content: space-between;
    }
}
.quero-bolsa-logo {
    justify-self: center;
    width: 150px;
    height: 100px;

    @media (max-width: 992px) {
        width: 120px;
        height: 70px;
    }
}

.infos {
    justify-self: start;
    display: flex;
    align-items: center;
    padding-left: 20px;
}

.help {
    display: flex;
    align-items: center;
    align-self: stretch;
    padding-right: 35px;
    border-right: 5px solid rgba(0, 0, 0, 0.1);

    @media (max-width: 992px) {
        flex-direction: column;
        padding-right: 20px;
        border-right: 3px solid rgba(0, 0, 0, 0.1);
    }
}
.info-icon {
    font-size: 27px;
    padding-right: 5%;
}

.help-text-desktop {
    @media (max-width: 992px) {
        display: none;
    }
}

.help-text-mobile {
    @media (min-width: 992px) {
        display: none;
    }
}

.whatsapp {
    display: grid;
    grid-template-areas:
        "icon number"
        "icon text";
    align-items: center;
    justify-items: start;

    @media (max-width: 992px) {
        display: none;
    }
}

.whatsapp-icon {
    grid-area: icon;
    font-size: 27px;
    padding-left: 35px;
    padding-right: 10px;
    color: #0FA866;
}

.whatsapp-number {
    grid-area: number;
    padding-bottom: 3px;
}

.whatsapp-text {
    grid-area: text;
    font-size: 0.7rem;
}

.user {
    justify-self: end;
    display: flex;
    align-items: center;
    padding-right: 20px;

    @media (max-width: 992px) {
        flex-direction: column;
        border-left: 3px solid rgba(0, 0, 0, 0.1);
        padding-left: 20px;
    }
}

.user-name-desktop {
    padding-right: 10px;

    @media (max-width: 992px) {
        display: none;
    }
}

.user-name-mobile {
    @media (min-width: 992px) {
        display: none;
    }
}

.user-icon {
    font-size: 27px;
}
</style>