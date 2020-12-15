<!--
    Name: Chase Cook
    Assignment: Final Project - Current Weather
    Class: Web Dev II Markley
-->

<template>

<div class="container" id="cardDiv">
    <div class="row row-cols-1 row-cols-md-3 g-4">
        <div class="col" id="cardCol">
            <div class="card" id="theCard">
                <!-- URL set up for weather icon to be displayed with pIcon using the icon code within the json -->
                <img v-bind:src="'http://openweathermap.org/img/wn/' + pIcon + '@2x.png'" class="card-img-top" id="cardImg">
                <div class="card-body" id="cardBody">
                    <!-- Displays the temp along with the degree unit of the users choice (F or C) -->
                    <h5 class="card-title">Temperature: {{pTemp}} °{{pUnit}}</h5>
                    <!-- Displays the city name -->
                    <p class="card-text">{{pCity}}</p>
                    <!-- Displays the weather condition in the selected area -->
                    <p class="card-text">Current conditions: {{pConditions}}</p>
                </div>
            </div>
        </div>
    </div>
</div>

</template>

<script>
export default {
    name: 'Card',
    // props holding api data that are then placed into html template above using vue
    props: ['pIcon', 'pTemp', 'pUnit', 'pCity', 'pConditions']
}
</script>

<style scoped>
#cardDiv
{
    color: black;
    margin-bottom: 20px;
}

#cardBody
{
    background: honeydew;
}

#cardImg
{
    background: #2A2A2E;
}

#cardCol
{
    margin: auto;
    margin-right: 240px;
}

#theCard
{
    width: 260px;
}
</style>

