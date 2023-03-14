<template>
    <nav class="nav-bar-background">
        <div class="buttons-container">
            <button class="nav-button ">Minha Conta</button>
            <button class="nav-button mobile-hide">Pré-matrículas</button>
            <button class="nav-button button-pressed mobile-hide">Bolsas favoritas</button>
            <button class="nav-button menu-button">Menu<font-awesome icon="fa-solid fa-chevron-down" class="menu-icon"/></button>
        </div>
    </nav>
</template>

<script>
export default {
    name: "NavigationBar"
}
</script>

<style lang="scss" scoped>
.nav-bar-background {
    display: flex;
    justify-content: center;
    background-color: #18ACC4;
    box-shadow: inset 0 13px 10px -10px rgba(0, 0, 0, 0.3);
    width: 100%;
}

.buttons-container {
    width: 1500px;
    display: flex;
    flex-direction: row;

    @media (max-width: 992px) {
        justify-content: space-between;
    }
}

.nav-button {
    background-color: #18ACC4;
    color: white;
    border: none;
    text-align: center;
    font-size: 1rem;
    padding: 10px 20px;
    cursor: pointer;
    box-shadow: inset 0 13px 10px -10px rgba(0, 0, 0, 0.3);
    transition-duration: 0.4s;
}

.nav-button:hover {
    background-color: #007A8D;
}

.button-pressed {
    background-color: #007A8D;
}

.menu-button {
    @media (min-width: 992px) {
        display: none;
    }
}

.menu-icon {
    font-size: 16px;
    padding-left: 7px;
}

.mobile-hide {
    @media (max-width: 992px) {
        display: none;
    }
}
</style>