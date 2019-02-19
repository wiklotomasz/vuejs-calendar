<template>
    <div>
        <div id="day-bar">
            <div>Pn</div>
            <div>Wt</div>
            <div>Śr</div>
            <div>Czw</div>
            <div>Pt</div>
            <div>Sob</div>
            <div>Nd</div>
        </div>
        <div id="calendar">
            <div v-for="week in weeks" v-bind:key="week" class="calendar-week">
                <calendar-day v-for="day in week" v-bind:key="day" :day="day">{{day}}</calendar-day>
            </div>
        </div>
    </div>
</template>
<script>
    import CalendarDay from './CalendarDay.vue';

    export default {
        data() {
            return {
                month: 2,
                year: 2019,
            };
        },
        computed: {
            days() {

                //generating days of current month
                let days = [];
                let currentDay = this.$moment( `${this.year}-${this.month}-1`, 'YYYY-M-D');
                do {
                    days.push(currentDay);
                    currentDay = this.$moment(currentDay).add(1, 'days');
                } while (currentDay.month() + 1 === this.month);


                const MONDAY = 0;
                const SUNDAY = 1;

                //add next days to end
                if (currentDay.day() !== MONDAY) {
                    if (this.month === 12) {
                        currentDay = this.$moment( `${this.year+1}-1-1`, 'YYYY-M-D');
                    } else {
                        currentDay = this.$moment( `${this.year}-${this.month+1}-1`, 'YYYY-M-D');
                    } 
                    do {
                        days.push(currentDay);
                        currentDay = this.$moment(currentDay).add(1, 'days');
                    } while (currentDay.day() !== SUNDAY);
                }

                if (currentDay.day() !== MONDAY) {
                    //add previous days to start
                    currentDay = this.$moment(days[0]);
                    do {
                        currentDay = this.$moment(currentDay).subtract(1, 'days');
                        days.unshift(currentDay);
                    } while (currentDay.day() !== SUNDAY);
                }

                return days;
            },

            weeks() {
                let weeks = [];
                let week = [];

                for (let day of this.days) {
                    week.push(day);
                    if (week.length === 7) {
                        weeks.push(week);
                        week = [];
                    }

                }

                return weeks;
            }
        },
        components: {
            CalendarDay
        }
    }
</script>
