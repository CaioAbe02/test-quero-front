<template>
    <div class="add-favorite-offers-container" v-if="open">
        <div class="bg"></div>
        <div class="add-favorite-offers-inner">
            <font-awesome icon="fa-solid fa-xmark" class="icon-close" @click="$emit('close')"/>
            <div class="add-offer-title">
                <h2>Adicionar bolsa</h2>
                <span>Filtre e adicione as bolsas de seu interesse</span>
            </div>
            <div class="select-city-container">
                <strong>SELECIONE SUA CIDADE</strong>
                <select v-model="filterCity" @click="uncheckOffers">
                    <option v-for="city in cities" :key="city">{{ city }}</option>
                </select>
            </div>
            <div class="select-course-container">
                <strong>SELECIONE O CURSO DE SUA PREFERÊNCIA</strong>
                <select v-model="filterCourse" @click="uncheckOffers">
                    <option v-for="course in courses" :key="course">{{ course }}</option>
                </select>
            </div>
            <div class="checkbox-kind-container">
                <strong>COMO VOCÊ QUER ESTUDAR?</strong>
                <div class="checkbox-kind-checkboxes">
                    <label>Presencial
                        <input type="checkbox" v-model="filterKindPresencial">
                        <span class="checkbox-kind"></span>
                    </label>
                    <label>A distância
                        <input type="checkbox" v-model="filterKindEad">
                        <span class="checkbox-kind"></span>
                    </label>
                </div>
            </div>
            <div class="slider-value-container">
                <strong>ATÉ QUANTO PODE PAGAR?</strong>
                <span>R${{ filterValue }}</span>
                <input type="range" min="50" max="10000" v-model="filterValue">
            </div>
            <div class="sort-container">
                <strong>Resultado:</strong>
                <strong class="sort">Ordenar por <strong class="sort-type">Nome da Faculdade<font-awesome icon="fa-solid fa-chevron-down" class="sort-icon"/></strong></strong>
            </div>
            <div class="offers-container">
                <div v-for="i in commonIndexes" :key="i">
                    <div class="offer-item">
                        <div class="checkbox-kind-checkboxes">
                            <label>
                                <input type="checkbox" v-model="offersSelected" v-bind:value="i" @click="offerCheckboxClick(i)">
                                <span class="checkbox-kind"></span>
                            </label>
                        </div>
                        <img :src=offers[i].university.logo_url alt="logo da universidade" class="univeristy-logo">
                        <div class="offer-course">
                            <strong>{{ offers[i].course.name }}</strong>
                            <span>{{ this.offers[i].course.level }}</span>
                        </div>
                        <div class="offer-value">
                            <span>Bolsa de
                                <strong>{{ offers[i].discount_percentage }}%</strong>
                            </span>
                            <strong>R$ {{ offers[i].price_with_discount }}/mês</strong>
                        </div>
                    </div>
                </div>
            </div>
            <div class="add-offers-buttons">
                <button class="button-cancel" @click="$emit('close')">Cancelar</button>
                <button class="button-add"
                        :class="!offersSelected.length && 'button-disabled'"
                        @click="
                            sendFavoriteOffers(this.offersSelected);
                            offersSelected.length && $emit('close');
                            uncheckOffers()
                        ">Adicionar bolsa(s)</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AddfavoriteOffer',
    props: {
        offers: {
            required: true,
            type: Object
        },
        open: {
            required: true,
            type: Boolean
        }
    },
    data() {
        return {
            filteredOffers: [],
            filterCity: '',
            filterCourse: '',
            filterKindPresencial: false,
            filterKindEad: false,
            filterValue: 10000,
            offerCheck: false,
            offersSelected: []
        }
    },
    computed: {
        cities() {
            const cities = [];
            for (let i = 0; i < this.offers.length; i++) {
                cities.push(this.offers[i].campus.city);
            }
            return [...new Set(cities)]; //remove duplicated cities
        },
        courses() {
            const courses = [];
            for (let i = 0; i < this.offers.length; i++) {
                courses.push(this.offers[i].course.name);
            }
            return [...new Set(courses)]; //remove duplicated courses
        },
        filteredFromCities() {
            const indexes = [];
            for (let i = 0; i < this.offers.length; i++) {
                if (this.offers[i].campus.city == this.filterCity) {
                    indexes.push(i);
                }
            }
            return indexes;
        },
        filterdFromCourse() {
            const indexes = [];
            for (let i = 0; i < this.offers.length; i++) {
                if (this.offers[i].course.name == this.filterCourse) {
                    indexes.push(i);
                }
            }
            return indexes;
        },
        filterdFromPresencial() {
            const indexes = [];
            for (let i = 0; i < this.offers.length; i++) {
                if (this.filterKindPresencial) {
                    if (this.offers[i]) {
                        if (this.offers[i].course.kind == "Presencial") {
                            indexes.push(i);
                        }
                    }
                }
            }
            return indexes;
        },
        filterdFromEad() {
            const indexes = [];
            for (let i = 0; i < this.offers.length; i++) {
                if (this.filterKindEad) {
                    if (this.offers[i]) {
                        if (this.offers[i].course.kind == "EaD") {
                            indexes.push(i);
                        }
                    }
                }
            }
            return indexes;
        },
        filterdFromKind() {
            return this.filterdFromPresencial.concat(this.filterdFromEad)
        },
        filterdFromValue() {
            const indexes = [];
            for (let i = 0; i < this.offers.length; i++) {
                if (this.offers[i].price_with_discount <= this.filterValue) {
                    indexes.push(i);
                }
            }
            return indexes;
        },
        commonIndexes() {
            const indexes = [
                this.filteredFromCities,
                this.filterdFromCourse,
                this.filterdFromKind,
                this.filterdFromValue
            ]
            for (let i = 0; i < indexes.length; i++) {
                if (indexes[i].length == 0) {
                    return [];
                }
            }
            return indexes.reduce((a, b) => a.filter(c => b.includes(c)));
        }
    },
    methods: {
        offerCheckboxClick(offerIndex) {
            if (this.offersSelected.includes(offerIndex)) {
                this.offersSelected = this.offersSelected.filter(item => item !== offerIndex);
            }
            else {
                this.offersSelected.push(offerIndex);
            }
        },
        sendFavoriteOffers(offers) {
            this.$emit('getOffers', offers)
        },
        uncheckOffers() {
            this.offersSelected.splice(0);
        }
    }
}
</script>

<style lang="scss" scoped>
.bg {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(31, 45, 48, 0.88);
    height: 100%;
}

.add-favorite-offers-container {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;

    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

.add-favorite-offers-inner {
    position: relative;
    top: 200px;
    opacity: 1;
    z-index: 1;
    background-color: #FBFBFB;

    display: grid;
    grid-template-areas:
        'title title'
        'city course'
        'kind value'
        'sort sort'
        'offers offers'
        'buttons buttons';
    grid-template-columns: 1fr 1fr;
    column-gap: 20px;

    padding: 40px;
    color: #1F2D30;

    @media (max-width: 992px) {
        top: 100px;
        width: calc(100% - 80px);
        display: grid;
        grid-template-areas:
            'title'
            'city'
            'course'
            'kind'
            'value'
            'sort'
            'offers'
            'buttons';
            grid-template-columns: 1fr;
    }
}

.icon-close {
    justify-self: end;
    font-size: 35px;
    color: #FBFBFB;
    cursor: pointer;

    position: absolute;
    top: -40px;

    @media (max-width: 992px) {
        padding-right: 20px;
    }
}

.add-favorite-offers-inner strong {
    font-size: 0.8rem;
}

.add-offer-title {
    grid-area: title;
    padding-bottom: 20px;
}

.add-offer-title h2 {
    font-size: 1.7rem;
}

.add-offer-title span {
    font-size: 0.9rem;
}

.select-city-container {
    grid-area: city;
    display: flex;
    flex-direction: column;
    padding: 20px 0 30px 0;

    @media (max-width: 992px) {
        padding: 10px 0;
    }
}

.select-course-container {
    grid-area: course;
    display: flex;
    flex-direction: column;
    padding: 20px 0 30px 0;

    @media (max-width: 992px) {
        padding: 10px 0 20px 0;
    }
}

.checkbox-kind-container {
    grid-area: kind;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.checkbox-kind-container strong {
    @media (max-width: 992px) {
        padding-bottom: 20px;
    }
}

.checkbox-kind-checkboxes {
    display: flex;

    @media (max-width: 992px) {
        justify-content: space-between;
        padding-bottom: 20px;
    }
}

.checkbox-kind-checkboxes label {
    display: block;
    position: relative;
    padding: 0 30px;
    font-size: 0.9rem;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;

    @media (max-width: 992px) {
        padding-right: 0;
    }
}


/* Hide default checkbox */
.checkbox-kind-checkboxes input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
}

/* Create a custom checkbox */
.checkbox-kind {
    position: absolute;
    top: 0;
    left: 0;
    height: 1em;
    width: 1em;
    border: 1px solid rgba(1, 1, 1, 0.2);
    border-radius: 4px;
}
/* checked -> add a blue background */
.checkbox-kind-checkboxes label input:checked ~ .checkbox-kind  {
    background-color: #18ACC4;
}

/* Create and hide the checkmark */
.checkbox-kind:after {
    content: "";
    position: absolute;
    display: none;
}

/* Show the checkmark */
.checkbox-kind-checkboxes label input:checked ~ .checkbox-kind:after {
    display: block;
}

/* Style the checkmark/indicator */
.checkbox-kind-checkboxes label .checkbox-kind:after {
  left: 5px;
  top: 2px;
  width: 5px;
  height: 9px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}

.slider-value-container {
    grid-area: value;
    display: flex;
    flex-direction: column;

}

.slider-value-container input {
    -webkit-appearance: none;
    appearance: none;
    width: 100%;
    height: 5px;
    background: rgba(1, 1, 1, 0.2);
    border-radius: 5px;
}

.slider-value-container input::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    border: 2px solid #18ACC4;
    background: #FBFBFB;
}

.slider-value-container input::-moz-range-thumb {
    background: #18ACC4;
}

.slider-value-container span {
    padding-bottom: 20px;
}

.sort-container {
    grid-area: sort;

    display: flex;
    justify-content: space-between;

    border-bottom: 3px solid rgba(1, 1, 1, 0.1);
    padding: 40px 0 15px 0;
}

.sort {
    text-align: end;
}

.sort-type {
    color: #007A8D;
    cursor: pointer;
}

.sort-icon {
    padding-left: 10px;
}

.offers-container {
    grid-area: offers;
    align-items: center;
    align-self: center;
}

.offer-item {
    display: grid;
    grid-template-columns: 20px min-content auto auto;
    border-bottom: 3px solid rgba(1, 1, 1, 0.1);
    padding: 15px 0;
    align-items: center;

    @media (max-width: 992px) {
        row-gap: 10px;
    }
}

.univeristy-logo {
    grid-column-start: 2;

    width: 125px;
    height: auto;
    max-height: 40px;
    padding: 0 25px;

    @media (max-width: 992px) {
        grid-row-end: span 2;
        width: 75px;
    }
}

.offer-course {
    grid-column-start: 3;

    display: flex;
    flex-direction: column;
    row-gap: 5px;

}

.offer-course strong {
    color: #007A8D;
}

.offer-course span {
    font-size: 0.7rem;
}

.offer-value {
    grid-column-start: 4;
    justify-self: end;

    display: flex;
    flex-direction: column;
    row-gap: 5px;

    @media (max-width: 992px) {
        grid-column-start: 3;
        justify-self: start;
    }
}

.offer-value span {
    font-size: 0.8rem;
}

.offer-value strong {
    color: #0FA866;
    font-size: 0.8rem;
}

.add-offers-buttons {
    grid-area: buttons;
    justify-self: end;

    padding-top: 30px;

    @media (max-width: 992px) {
        justify-self: stretch;
        display: flex;
        justify-content: space-between;
    }
}


.add-offers-buttons button {
    background-color: transparent;
    border: 2px solid;
    border-radius: 3px;
    font-size: 0.8rem;
    padding: 10px 15px;
    margin-left: 20px;
    cursor: pointer;

    @media (max-width: 992px) {
        margin-left: 0;
    }
}

.add-offers-buttons .button-cancel {
    border-color: #007A8D;
    color: #007A8D;
}

.add-offers-buttons .button-add {
    border-color: #DE9E1F;
    color: #1F2D30;
    background-color: #FDCB13;
}

.add-offers-buttons .button-disabled {
    cursor: not-allowed;
    background-color: rgba(1, 1, 1, 0.3);
    color: rgba(1, 1, 1, 0.5);
    border-color: rgba(1, 1, 1, 0.5);
}
</style>