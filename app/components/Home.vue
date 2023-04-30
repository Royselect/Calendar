<template>
    <Page>
        <ActionBar>
            <Label text="Календарь"/>
        </ActionBar>
        <FlexboxLayout flexDirection="column" class="calendar-today-box">
            <FlexboxLayout @tap="nowDate" flexDirection="row" class="today-date-info">
                <label :text="dayNow" />
                <label text=" "/>
                <label :text="monthesForNow[monthNow]"/>
                <label :text="yearNow"/>
                <label text=" г."/>
            </FlexboxLayout>
            <FlexboxLayout flexDirection="column" class="calendar-main">
                <FlexboxLayout flexDirection="row" class="month-year-info"> 
                    <FlexboxLayout flexDirection="row">
                        <label :text="monthes[month]"/>
                        <label :text="year"/>
                    </FlexboxLayout>
                    <FlexboxLayout flexDirection="row" class="arrows">
                        <label text="ᐸ" @tap="decrease"/>
                        <label text="ᐳ" @tap="increase"/>
                    </FlexboxLayout>
                </FlexboxLayout>
                <GridLayout class="calendar" rows="auto" :columns="colForWeek">
                    <label v-for="(item, index) in weekdays" :text="item" :key="index"
                    row="0" :col="index"/>
                </GridLayout>
                <template>
                    <GridLayout class="calendar-main" v-for="(item, index) in days" :key="index" :rows="rowIndex" columns="*,*,*,*,*,*,*">
                        <template v-if="index == 0 && item.length < 7">
                            <label class="dayNumber" v-for="(day, idx) in item" :key="idx" :text="day.index" :row="index" :col="idx + (6-item.length+1)" :style="{'background-color': day.dateNow}"/> 
                        </template>
                        <template v-else>
                            <label class="dayNumber" v-for="(day, idx) in item" :key="idx" :text="day.index" :row="index" :col="idx" :style="{'background-color': day.dateNow}"/> 
                        </template>
                    </GridLayout>
                </template>
                
            </FlexboxLayout>
        </FlexboxLayout>
      
        
    </Page>
</template>

<script>
// На главном экране должнен быть чисто календарь - готов
// Нужен селектор выбора месяца, где нибудь сверху как в конвертере - готово
// Текущий день мы должны подсвечивать, также и месяц указывать - готово
// Кнопка возврата к текущему месяцу - готово
// Сопоставить числа и дни недели
  export default{
    data() {
        return {
            month: "",
            year: "",
            days: [],
            week: 0,
            dayFirstMonth: "1",
            weekdays: ["Пн", "Вт", "Ср", "Чт", "Пт", "Сб", "Вс"],
            monthesForNow: ["Января ", "Февраля ", "Марта ", "Апреля ", "Мая ", "Июня ", "Июля ", "Августа ", "Сентября ", "Октября ", "Ноября ", "Декабря "],
            monthes: ["Январь ", "Февраль ", "Март ", "Апрель ", "Май ", "Июнь ", "Июль ", "Август ", "Сентябрь ", "Октябрь ", "Ноябрь ", "Декабрь "],
            date: "",
            dayNow: "",
            monthNow: "",
            yearNow: "",
        };
    },
    mounted() {
        // Даты для календаря
        this.month = new Date().getMonth();
        this.year = new Date().getFullYear();
        this.date = new Date();
        this.days = [];
        this.week = 0;
        this.days[this.week] = [];
        this.calendar();
        // сохраняем даты для последующего возврата к текущей дате
        this.dayNow = this.date.getDate();
        this.monthNow = this.month;
        this.yearNow = this.year;
    },
    computed: {
        colForWeek: function () {
            const cols = [];
            for (let i = 0; i < this.weekdays.length; i++) {
                cols.push("*");
            }
            return cols.join(",");
        },
        rowsForMonth: function () {
            const rows = [];
            for (week in calendar()) {
                rows.push("auto");
            }
            return rows.join(",");
        },
        rowIndex: function () {
            const rows = [];
            for(let i = 0; i< this.days.length; i++){
                rows.push("auto");
            }
            return rows.join(",");
        }
    },
    methods: {
        nowDate() {
            this.month = this.monthNow;
            this.year = this.yearNow;
            this.days = [];
            this.week = 0;
            this.days[this.week] = [];
            this.calendar();
        },
        decrease() {
            this.month--;
            if (this.month < 0) {
                this.month = 12;
                this.month--;
                this.year--;
            }
            this.days = [];
            this.week = 0;
            this.days[this.week] = [];
            this.calendar();
        },
        increase() {
            this.month++;
            if (this.month > 11) {
                this.month = -1;
                this.month++;
                this.year++;
            }
            this.days = [];
            this.week = 0;
            this.days[this.week] = [];
            this.calendar();
        },
        calendar() {
            // this.days = [];
            // this.week = 0;
            // this.days[this.week] = [];
            var daysCount = new Date(this.year, this.month + 1, 0).getDate();
            for (let i = 1; i <= daysCount; i++) {
                if (new Date(this.year, this.month, i).getDay() != this.dayFirstMonth) {
                    var a = { index: i };
                    this.days[this.week].push(a);
                    // Красим сегодняшний день в календаре в синий
                    if (i == new Date().getDate() && this.year == new Date().getFullYear() && this.month == new Date().getMonth()) {
                        a.dateNow = "#0079d8";
                    }
                    else {
                        a.dateNow = "#1f1f1f";
                    }
                    // с понедельника начинается новая неделя, поэтому, когда мы доходим до его индекса
                    // мы должны перейти на новую неделю, то есть week++
                }
                else {
                    this.week++;
                    this.days[this.week] = [];
                    a = { index: i };
                    this.days[this.week].push(a);
                    if (i == new Date().getDate() && this.year == new Date().getFullYear() && this.month == new Date().getMonth()) {
                        a.dateNow = "#0079d8";
                    }
                    else {
                        a.dateNow = "#1f1f1f";
                    }
                }
            }
            
        },
    },
}
  
</script>

<style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';

    // Custom styles
    .fas {
        @include colorize($color: accent);
    }

    .info {
        font-size: 20;
        horizontal-align: center;
        vertical-align: center;
    }
</style>
