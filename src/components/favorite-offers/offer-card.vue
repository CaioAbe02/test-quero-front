<template>
    <div class="offer-card">
        <div class="offer-card-top">
            <img :src=offers[offer].university.logo_url alt="logo da universidade" class="offer-card-university-logo">
            <strong>{{ offers[offer].university.name }}</strong>
            <strong id="course-name">{{ offers[offer].course.name }}</strong>
            <div id="university-score-container">
                <strong>{{ offers[offer].university.score }}</strong>
                <font-awesome icon="fa-solid fa-star" id ="star-icon"/>
            </div>
        </div>
        <div class="offer-card-middle">
            <strong>{{ offers[offer].course.kind }} - {{ offers[offer].course.shift }}</strong>
            <span id="start-date">Início das aulas em {{ offers[offer].start_date }}</span>
        </div>
        <div class="offer-card-bottom">
            <strong id="card-top-text">Mensalidade com o Quero Bolsa:</strong>
            <span id="full-price">R$ {{ offers[offer].full_price }}</span>
            <div id="price-with-discount-container">
                <strong>R$ {{ offers[offer].price_with_discount }}</strong>
                <span> /mês</span>
            </div>
            <div id="offer-card-buttons">
                <button id="delete-button" @click="this.$emit('deleteButton', offer)">Excluir</button>
                <button id="see-offer-button" v-if="offers[offer].enabled">Ver oferta</button>
                <button id="unavailable-button" v-else>Indisponível</button>
            </div>
        </div>
    </div></template>

<script>
export default {
    name: 'OfferCard',
    props: {
        offers: {
            required: true,
            type: Array
        },
        offer: {
            required: true,
            type: Number
        }
    }
}
</script>

<style lang="scss" scoped>
.offer-card {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;

    background-color: white;
    box-shadow: 0px 2px 2px 2px rgba(0, 0, 0, 0.1);
    width: 240px;
    height: 380px;
    padding: 25px 20px;

    @media (max-width: 992px) {
        width: auto;
    }
}

.offer-card strong {
    font-size: 0.9rem;
}

.offer-card-top {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.offer-card-university-logo {
    width: auto;
    height: 40px;
    padding-bottom: 30px;
}

#course-name {
    padding-top: 5px;
    padding-bottom: 10px;
    color: #007A8D;
}

#university-score-container {
    display: flex;
    align-items: center;
    column-gap: 5px;
    padding-bottom: 10px;
}

#star-icon {
    font-size: 12px;
    color: #FDCB13;
}

.offer-card-middle {
    display: flex;
    flex-direction: column;
    align-items: center;

    border-top: 1px solid rgba(1, 1, 1, 0.2);
    border-bottom: 1px solid rgba(1, 1, 1, 0.2);
    padding: 15px 0;
    row-gap: 10px;
}

#start-date {
    font-size: 0.7rem;
}


.offer-card-bottom {
    display: flex;
    flex-direction: column;
    align-items: center;
}

#card-top-text {
    font-size: 0.7rem;
    padding: 15px 0;
}
#full-price {
    font-size: 0.7rem;
    text-decoration: line-through;
}

#price-with-discount-container {
    padding-bottom: 15px;
}

#price-with-discount-container span{
    font-size: 0.7rem;
}

#price-with-discount-container strong {
    font-size: 1rem;
    color: #0FA866
}

#offer-card-buttons {
    display: flex;
    column-gap: 7px;
}
#offer-card-buttons button {
    border-radius: 3px;
    font-size: 0.8rem;
    padding: 5px 15px;
    margin-left: 20px;
}

#delete-button {
    background-color: transparent;
    border: 2px solid #007A8D;
    color: #007A8D;
    cursor: pointer;
}

#see-offer-button {
    background-color: #FDCB13;
    border: 2px solid #DE9E1F;
    color: #1F2D30;
    font-weight: 700;
    cursor: pointer;
}
#unavailable-button {
    cursor: not-allowed;
    background-color: rgba(1, 1, 1, 0.2);
    color: rgba(1, 1, 1, 0.3);
    border-color: rgba(1, 1, 1, 0.3);
}
</style>