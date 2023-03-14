<template>
    <div class="favorite-offers-background">
        <div class="favorite-offers-container">
            <h1 class="title">Bolsas favoritas</h1>
            <span class="description">Adicione bolsas de cursos e faculdades do seu interesse e receba atualizações com as melhores ofertas disponíveis</span>
            <div class="filters">
                <button class="filter-button" :class="isSelected(0) && 'current-filter'" @click="updateSelectedFilter(0)">Todos os semestres</button>
                <button class="filter-button" :class="isSelected(1) && 'current-filter'" @click="updateSelectedFilter(1)">2º semestre de 2019</button>
                <button class="filter-button" :class="isSelected(2) && 'current-filter'" @click="updateSelectedFilter(2)">1º semestre de 2020</button>
            </div>
            <div class="offers-cards">
                <div class="add-offer-card" @click="isOpen = true">
                    <font-awesome icon="fa-solid fa-circle-plus" class="add-icon" />
                    <strong>Adicionar curso</strong>
                    <span>Clique para adicionar bolsas de cursos do seu interesse</span>
                </div>
                <OfferCard
                    v-for="offerIndex in favoriteOffers"
                    :key="offerIndex"
                    :offers="offers"
                    :offer="offerIndex"
                    @deleteButton="deleteOffer"
                    v-show="showOffer(offerIndex)"
                />
            </div>
        </div>
    </div>
    <AddFavoriteOffers :offers="offers" :open="isOpen" @close="isOpen = !isOpen" @getOffers="addOffers"/>

</template>

<script>
import AddFavoriteOffers from './add-favorite-offers.vue'
import OfferCard from './offer-card.vue'

export default {
    name: 'FavoriteOffers',
    components: {
        AddFavoriteOffers,
        OfferCard
    },
    props: {
        offers: {
            required: true,
            type: Object
        }
    },
    data() {
        return {
            isOpen: false,
            selectedFilter: 0,
            favoriteOffers: []
        }
    },
    mounted() {
        if(localStorage.getItem('favoriteOffers')) {
            try {
                this.favoriteOffers = JSON.parse(localStorage.getItem('favoriteOffers'));
            }
            catch(e) {
                localStorage.removeItem('favoriteOffers');
            }
        }
    },
    watch: {
        name(newName) {
            localStorage.name = newName;
        }
    },
    methods: {
        updateSelectedFilter(filterIndex) {
            this.selectedFilter = filterIndex;
        },
        isSelected(filterIndex) {
            if (this.selectedFilter == filterIndex) {
                return true;
            }
            return false;
        },
        showOffer(offerIndex) {
            let month = this.offers[offerIndex].start_date.slice(3, 5);
            let year = this.offers[offerIndex].start_date.slice(6, 10);
            console.log(year);

            if (this.selectedFilter == 0) {
                return true;
            }
            else if (this.selectedFilter == 1) {
                if (month >= 8 && year == 2019) {
                    return true;
                }
                else {
                    return false;
                }
            }
            else if (this.selectedFilter == 2) {
                if (month >= 1 && year == 2020) {
                    return true;
                }
                else {
                    return false;
                }
            }

            return false;
        },
        addOffers(offers) {
            for (let i = 0; i < offers.length; i++) {
                if (!this.favoriteOffers.includes(offers[i])) {
                    this.favoriteOffers.push(offers[i])
                    this.saveOffers();
                }
            }
        },
        deleteOffer(offer) {
            this.favoriteOffers = this.favoriteOffers.filter(item => item !== offer);
            this.saveOffers();
        },
        saveOffers() {
            let parsed = JSON.stringify(this.favoriteOffers);
            localStorage.setItem('favoriteOffers', parsed);
        }
    }
}
</script>

<style lang="scss" scoped>
.favorite-offers-background {
    background-color: #FBFBFB;
    min-height: 1000px;

    @media (max-width: 992px) {
        min-height: 700px;
    }
}

.favorite-offers-container {
    max-width: 1500px;
    margin: auto;
    padding: 20px 0 20px 40px;

    @media (max-width: 992px) {
        padding: 10px 20px 20px 20px;
    }
}

.title {
    font-size: 2rem;
    color: #1F2D30;
    padding-bottom: 10px;
}

.description {
    color: #1F2D30;
}

.filters {
    padding: 40px 40px 30px 0px;
    display: flex;
    justify-content: end;

    @media (max-width: 992px) {
        flex-direction: column;
        padding: 30px 0px 30px 0px;
    }
}

.filter-button {
    background-color: #FBFBFB;
    color: #007A8D;
    float: left;
    border: 1px solid #007A8D;
    padding: 5px 20px;
    transition-duration: 0.4s;
    cursor: pointer;
}

.filter-button:first-child {
    border-radius: 7px 0px 0px 7px;

    @media (max-width: 992px) {
        border-radius: 7px 7px 0px 0px;
    }
}

.filter-button:last-child {
    border-radius: 0px 7px 7px 0px;

    @media (max-width: 992px) {
        border-radius: 0px 0px 7px 7px;
    }
}

.filter-button:hover {
    background-color: #007A8D;
    color: #FBFBFB;
}

.current-filter {
    background-color: #007A8D;
    color: #FBFBFB;
}

.offers-cards {
    display: flex;
    flex-wrap: wrap;
    column-gap: 30px;
    row-gap: 30px;

    @media (max-width: 992px) {
        flex-direction: column;
    }
}

.add-offer-card {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;

    background-color: white;
    box-shadow: 0px 2px 2px 2px rgba(0, 0, 0, 0.1);
    width: 240px;
    height: 380px;
    padding: 25px 20px;
    cursor: pointer;
    transition: all 0.3s ease-out;

    @media (max-width: 992px) {
        width: auto;
        height: 200px;
    }
}

.add-offer-card:hover {
    transform: translateY(-5px) scale(1.005);
}

.add-icon {
    font-size: 70px;
    color: #18ACC4;
}

.add-offer-card strong {
    color: #1F2D30;
    padding-top: 40px;
    padding-bottom: 5px;
}

.add-offer-card span {
    font-size: 0.8rem;
    padding: 0 30px;
}
</style>