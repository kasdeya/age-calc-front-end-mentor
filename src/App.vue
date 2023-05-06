<template>
    <div class="container">
        <div class="form-container">
            <form>
                <ul>
                    <li> <label :class="{ labelError: !dayValid }" for="day">DAY</label>
                        <input :class="{ inputError: !dayValid }" class="dayInput" type="text" v-model="day" maxlength=2
                            id="day" @input="filterNonNumeric" placeholder="DD"
                            :style="{ '--day-placeholder-color': dayPlaceholderColor }">
                        <b class="error">{{ errors.day }}</b>
                    </li>
                    <li>
                        <label :class="{ labelError: !monthValid }" for="month">MONTH</label>
                        <input :class="{ inputError: !monthValid }" class="monthInput" type="text" v-model="month"
                            maxlength=2 id="month" @input="filterNonNumeric" placeholder="MM"
                            :style="{ '--month-placeholder-color': monthPlaceholderColor }">
                        <b class="error">{{ errors.month }}</b>
                    </li>
                    <li>
                        <label :class="{ labelError: !yearValid }" for="year">YEAR</label>
                        <input :class="{ inputError: !yearValid }" class="yearInput" type="text" v-model="year" id="year"
                            @input="filterNonNumeric" placeholder="YY"
                            :style="{ '--year-placeholder-color': yearPlaceholderColor }">
                        <b class="error">{{ errors.year }}</b>
                    </li>
                </ul>
                <div class="wrapper"><button class="submitBtn" @click.prevent="calculate"><img
                            src="@/assets/images/icon-arrow.svg" alt="button"></button>
                </div>
            </form>
        </div>
        <div class="result-container">
            <ul>
                <li>
                    <span class="age">{{ this.age.years }}</span> <span v-if="this.age.years == 1">year</span> <span
                        v-else>years</span>
                </li>
                <li>
                    <span class="age">{{ this.age.months }}</span> <span v-if="this.age.months == 1">month</span> <span
                        v-else>months</span>
                </li>
                <li>
                    <span class="age">{{ this.age.days }}</span> <span v-if="this.age.days == 1">day</span> <span
                        v-else>days</span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            day: '',
            month: '',
            year: '',
            age: {
                days: '- -',
                months: '- -',
                years: '- -',
            },
            errors: {
                day: '',
                month: '',
            },
            dayValid: true,
            monthValid: true,
            yearValid: true,
            dayPlaceholderColor: 'var(--smokey-grey)',
            monthPlaceholderColor: 'var(--smokey-grey)',
            yearPlaceholderColor: 'var(--smokey-grey)'
        }
    },
    methods: {
        async validation() {

            if (this.day == '') {
                this.errors.day = 'This field is required'
                this.dayValid = false
                this.dayPlaceholderColor = 'var(--light-red)'
            } else if (this.day >= 32 || this.day == 0) {
                this.errors.day = 'Enter a valid day'
                this.dayPlaceholderColor = 'var(--light-red)'
                this.dayValid = false
            } else {
                this.errors.day = ''
                this.dayValid = true
            }

            if (this.month == '') {
                this.errors.month = 'This field is required'
                this.monthPlaceholderColor = 'var(--light-red)'
                this.monthValid = false
            } else if (this.month >= 13 || this.month == 0) {
                this.errors.month = 'Enter a valid month'
                this.monthPlaceholderColor = 'var(--light-red)'
                this.monthValid = false
            } else {
                this.errors.month = ''
                this.monthValid = true
            }

            if (this.year == '') {
                this.errors.year = 'This field is required'
                this.yearPlaceholderColor = 'var(--light-red)'
                this.yearValid = false
            } else if (this.year > new Date().getFullYear() || this.year == 0) {
                this.errors.year = 'Enter a valid year'
                this.yearPlaceholderColor = 'var(--light-red)'
                this.yearValid = false
            } else {
                this.errors.year = ''
                this.yearValid = true
            }

            if (!this.dayValid && !this.monthValid && !this.yearValid) {
                this.errors.day = 'Enter a valid date'
                this.monthPlaceholderColor = 'var(--light-red)'
                this.errors.month = ''
                this.yearPlaceholderColor = 'var(--light-red)'
                this.errors.year = ''
                this.yearPlaceholderColor = 'var(--light-red)'
            }
        },
        filterNonNumeric(e) {
            let item = e.target.id
            let value = this[item].replace(/[^0-9]/g, '')
            this[item] = value;
        },
        async calculate() {
            await this.validation();
            if (this.dayValid && this.monthValid && this.yearValid) {
                const date = new Date()
                const today = date.getDate() - this.day
                const month = date.getMonth() - this.month
                const year = date.getFullYear() - this.year

                let age = (year * 365) + (month * 31) + today
                let ageInYears = Math.floor(age / 365)
                let remainingDays = age % 365
                let ageInMonths = Math.ceil(remainingDays / 31)
                remainingDays = remainingDays % 31

                this.age.days = remainingDays
                this.age.months = ageInMonths
                this.age.years = ageInYears
            }
        }
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,700;1,400;1,800&display=swap');

:root {
    --purple: hsl(259, 100%, 65%);
    --light-red: hsl(0, 100%, 67%);
    --off-white: hsl(0, 0%, 94%);
    --light-grey: hsl(0, 0%, 86%);
    --smokey-grey: hsl(0, 1%, 44%);
    --off-black: hsl(0, 0%, 8%);
}

/*
  1. Use a more-intuitive box-sizing model.
*/
*,
*::before,
*::after {
    box-sizing: border-box;
}

/*
  2. Remove default margin
*/
* {
    padding: 0;
    margin: 0;
}

/*
  3. Allow percentage-based heights in the application
*/
html,
body {
    height: 100%;
}

/*
  Typographic tweaks!
  4. Add accessible line-height
  5. Improve text rendering
*/
body {
    line-height: 1.5;
    -webkit-font-smoothing: antialiased;
}

/*
  6. Improve media defaults
*/
img,
picture,
video,
canvas,
svg {
    display: block;
    max-width: 100%;
}

/*
  7. Remove built-in form typography styles
*/
input,
button,
textarea,
select {
    font: inherit;
}

/*
  8. Avoid text overflows
*/
p,
h1,
h2,
h3,
h4,
h5,
h6 {
    overflow-wrap: break-word;
}

/*
  9. Create a root stacking context
*/
#root,
#__next {
    isolation: isolate;
}

#app {
    font-family: 'Poppins', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

body {
    background-color: var(--off-white);
    display: grid;
    place-items: center;
}

* {
    font-family: 'Poppins', sans-serif;
}

#app input {
    /* font-family: 'Poppins', sans-serif; */
    font-size: 32px
}

#app ul {
    list-style: none;
}

.container {
    background-color: white;
    margin: auto;
    width: 45%;
    padding: 3rem 3rem 2rem 3rem;
    border-radius: 1rem 1rem 10rem 1rem;
}

.form-container ul {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
}

.form-container ul li {
    display: flex;
    flex-direction: column;
    text-align: left;
}

.form-container input {
    border: 1px solid;
    width: 80%;
    border-radius: 5px;
    padding: 10px 10px 10px 20px;
    margin: 10px 0px;
    border-color: var(--light-grey);
    outline: none;
    cursor: pointer;
}

.form-container input:active,
.form-container input:focus-visible {
    border-color: var(--purple);
}

.dayInput::placeholder {
    color: var(--day-placeholder-color);
}

.monthInput::placeholder {
    color: var(--month-placeholder-color);
}

.yearInput::placeholder {
    color: var(--year-placeholder-color);
}

/* .placeholderError { */
/*     color: var(--light-red) */
/* } */

.result-container {
    margin-top: 1.2rem;
    text-align: left;
    font-style: italic;
    font-weight: 800;
    color: black;
    font-size: 78px;
}

.form-container button {
    background-color: var(--purple);
    border: transparent;
    border-radius: 100%;
    cursor: pointer;
    padding: 1rem;
}

.wrapper {
    position: relative;
}

.submitBtn {
    position: absolute;
    right: 0;
}

.wrapper:after {
    content: " ";
    width: 100%;
    margin-top: 39px;
    height: 2px;
    background-color: var(--light-grey);
    vertical-align: center;
    display: inline-block;
}

.form-container button:hover {
    background-color: black;
}

.age {
    color: var(--purple)
}

.error {
    color: var(--light-red);
    font-style: italic;
    font-weight: 100;
    font-size: 12px;
}

.inputError {
    border: 1px solid var(--light-red) !important;
    outline-color: var(--light-red);
}

.form-container label {
    font-size: 12px;
    letter-spacing: 3px;
}

.labelError {
    color: var(--light-red);
}

@media (max-width: 1300px) {
    .container {
        width: 90%;
        padding: 1rem;
    }

    .form-container ul {
        display: flex;
    }

    .submitBtn {
        right: 0;
        left: 0;
        margin: auto;
        /* transform: translateX(50%, 50%); */
        width: fit-content;
        padding: 10px;
    }

    .submitBtn img {
        width: 20px;
    }

    .wrapper::after {
        margin-top: 25px;
    }

    .form-container label {
        font-size: 10px;
    }

    .error {
        font-size: 8px;
    }

    #app input {
        font-size: 18px;
    }

    .result-container {
        font-size: 45px;
    }
}
</style>
