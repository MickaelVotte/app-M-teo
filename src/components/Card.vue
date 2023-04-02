<script>
import { Icon } from '@iconify/vue'
export default {
    computed: {
        fetchIcon() {
            if (this.weatherData) {
                const icon = this.weatherData.weather[0].icon
                return `https://openweathermap.org/img/wn/${icon}@2x.png`
            }
            return ''
        },


    },
    created() {
        this.fetchWeather()

    },
    data() {
        return {
            location: 'Paris',
            weatherData: {},
        }

    },

    methods: {
        async fetchWeather() {

            const apiKey = import.meta.env.VITE_API_KEY
            const apiUrl = import.meta.env.VITE_BASE_URL

            let response = await fetch(`${apiUrl}weather?q=${this.location}&appid=${apiKey}&units=metric&lang=fr`)
            this.weatherData = await response.json()
        }

    }
}



</script>


<template>
    <p class="text-center m-3 mt-5 fs-3 fw-bold">ApliMétéo</p>

    <div class="col d-flex justify-content-center mt-3">

        <div class="card cardCss p-2 rounded-4 shadow-lg p-3 mb-5 bg-body rounded border border-white">
            <div class="mx-auto mt-3 rounded-4 ">
                <div class="d-flex justify-content-center ">
                    <div class="input-group ">
                        <input type="text" placeholder="Ex: Paris" aria-label="Ex: Paris" aria-describedby="button-addon2"
                            v-model="location">
                        <button class="btnCss" type="button" @click="fetchWeather">

                            Valider
                        </button>
                    </div>
                </div>
            </div>
            <div class="card-body">
                <p class="fs-2 fw-bold"> {{ location }}</p>
                <p class="temp">{{ weatherData.main.temp }}°C</p>
                <div class="row">
                    <div class="col-6"><img class="imgCss" sizes="50" :src="fetchIcon" alt="Météo"></div>
                    <div class="col-6 text-center description">
                        <p>{{ weatherData.weather[0].description }}</p>
                    </div>
                </div>
                <div class="bottomCard">
                    <p>Humidité: {{ weatherData.main.humidity }}%</p>
                    <p>Vent: {{ weatherData.wind.speed }}km/h</p>
                </div>

            </div>
        </div>
    </div>
</template>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500&display=swap');

* {
    font-family: 'Poppins', sans-serif;

}

.temp {
    font-size: 3rem;
    font-weight: 600;

}

.description {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
}

.bottomCard {
    line-height: 1.5;

}

.cardCss {
    width: 28rem;
    height: 28rem;
}


.imgCss {
    width: 8rem;
    height: auto;
}

.btnCss {
    border: none;
    border-radius: 1rem;
    background-color: black;
    color: white;
    padding: 0.5rem;
    transition: ease 0.3s;

}


.btnCss:hover {
    background-color: rgb(35, 225, 51);
    color: white;
}
</style>